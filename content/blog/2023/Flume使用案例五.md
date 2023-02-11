---
title: "Flume使用案例五"
date: 2018-09-16 20:20:40
draft: false
---
**Flume与Flume之间数据传递，多Flume汇总数据到单Flume**

flume-1 监控文件 hive.log，flume-2 监控某一个端口的数据流，flume-1 与 flume-2 将数据发送给 flume-3，flume3 将最终数据写入到 HDFS

1)创建 flume-1.conf，用于监控 hive.log 文件，同时 sink 数据到 flume-3
/# Name the components on this agent a1.sources = r1 a1.sinks = k1 a1.channels = c1 /# Describe/configure the source a1.sources.r1.type = exec a1.sources.r1.command = tail -F /tmp/root/hive.log a1.sources.r1.shell = /bin/bash -c /# Describe the sink a1.sinks.k1.type = avro a1.sinks.k1.hostname = 172.17.0.2 a1.sinks.k1.port = 4141 /# Describe the channel a1.channels.c1.type = memory a1.channels.c1.capacity = 1000 a1.channels.c1.transactionCapacity = 100 /# Bind the source and sink to the channel a1.sources.r1.channels = c1 a1.sinks.k1.channel = c1

2)创建 flume-2.conf，用于监控端口 44444 数据流，同时 sink 数据到 flume-3

/# Name the components on this agent a2.sources = r1 a2.sinks = k1 a2.channels = c1 /# Describe/configure the source a2.sources.r1.type = netcat a2.sources.r1.bind = 172.17.0.2 a2.sources.r1.port = 44444 /# Describe the sink a2.sinks.k1.type = avro a2.sinks.k1.hostname = 172.17.0.2 a2.sinks.k1.port = 4141 /# Use a channel which buffers events in memory a2.channels.c1.type = memory a2.channels.c1.capacity = 1000 a2.channels.c1.transactionCapacity = 100 /# Bind the source and sink to the channel a2.sources.r1.channels = c1 a2.sinks.k1.channel = c1

3)创建 flume-3.conf,用于接收 flume-1 与 flume-2 发送过来的数据流,最终合并后 sink 到HDFS

/# Name the components on this agent a3.sources = r1 a3.sinks = k1 a3.channels = c1 /# Describe/configure the source a3.sources.r1.type = avro a3.sources.r1.bind = 172.17.0.2 a3.sources.r1.port = 4141 /# Describe the sink a3.sinks.k1.type = hdfs a3.sinks.k1.hdfs.path = hdfs://172.17.0.2:8020/flume3/%Y%m%d/%H /#上传文件的前缀 a3.sinks.k1.hdfs.filePrefix = flume3- /#是否按照时间滚动文件夹 a3.sinks.k1.hdfs.round = true /#多少时间单位创建一个新的文件夹 a3.sinks.k1.hdfs.roundValue = 1 /#重新定义时间单位 a3.sinks.k1.hdfs.roundUnit = hour /#是否使用本地时间戳 a3.sinks.k1.hdfs.useLocalTimeStamp = true /#积攒多少个 Event 才 flush 到 HDFS 一次 a3.sinks.k1.hdfs.batchSize = 100 /#设置文件类型,可支持压缩 a3.sinks.k1.hdfs.fileType = DataStream /#多久生成一个新的文件 a3.sinks.k1.hdfs.rollInterval = 600 /#设置每个文件的滚动大小大概是 128M a3.sinks.k1.hdfs.rollSize = 134217700 /#文件的滚动与 Event 数量无关 a3.sinks.k1.hdfs.rollCount = 0 /#最小冗余数 a3.sinks.k1.hdfs.minBlockReplicas = 1 /# Describe the channel a3.channels.c1.type = memory a3.channels.c1.capacity = 1000 a3.channels.c1.transactionCapacity = 100 /# Bind the source and sink to the channel a3.sources.r1.channels = c1 a3.sinks.k1.channel = c1

4)测试： 分别开启对应 flume-job(依次启动 flume-3,flume-2,flume-1)，同时产生文件变动并观察结果

bin/flume-ng agent --conf conf/ --name a3 --conf-file flume-3.conf bin/flume-ng agent --conf conf/ --name a2 --conf-file flume-2.conf bin/flume-ng agent --conf conf/ --name a1 --conf-file flume-1.conf

如图所示：

![](https://img-blog.csdn.net/20180916202017285?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)