---
title: "Flume使用案例四"
date: 2018-09-16 11:18:57
draft: false
---
**Flume与Flume之间传递数据： 单Flume，多channel，sink**

使用 flume-1 监控文件变动，flume-1 将变动内容传递给 flume-2，flume-2 负责存储到HDFS，同时 flume-1 将变动内容传递给 flume-3，flume-3 负责输出到本地文件目录

1)创建 flume-1.conf，用于监控 hive.log 文件的变动,同时产生两个 channel 和两个 sink 分别输送给 flume-2 和 flume-3
/# Name the components on this agent a1.sources = r1 a1.sinks = k1 k2 a1.channels = c1 c2 /# 将数据流复制给多个 channel a1.sources.r1.selector.type = replicating /# Describe/configure the source a1.sources.r1.type = exec a1.sources.r1.command = tail -F /tmp/root/hive.log a1.sources.r1.shell = /bin/bash -c /# Describe the sink a1.sinks.k1.type = avro a1.sinks.k1.hostname = 172.17.0.2 a1.sinks.k1.port = 4141 a1.sinks.k2.type = avro a1.sinks.k2.hostname = 172.17.0.2 a1.sinks.k2.port = 4142 /# Describe the channel a1.channels.c1.type = memory a1.channels.c1.capacity = 1000 a1.channels.c1.transactionCapacity = 100 a1.channels.c2.type = memory a1.channels.c2.capacity = 1000 a1.channels.c2.transactionCapacity = 100 /# Bind the source and sink to the channel a1.sources.r1.channels = c1 c2 a1.sinks.k1.channel = c1 a1.sinks.k2.channel = c2

2)创建 flume-2.conf,用于接收 flume-1 的 event,同时产生 1 个 channel 和 1 个 sink,将数据输送给 hdfs

/# Name the components on this agent a2.sources = r1 a2.sinks = k1 a2.channels = c1 /# Describe/configure the source a2.sources.r1.type = avro a2.sources.r1.bind = 172.17.0.2 a2.sources.r1.port = 4141 /# Describe the sink a2.sinks.k1.type = hdfs a2.sinks.k1.hdfs.path = hdfs://172.17.0.2:8020/flume2/%Y%m%d/%H /#上传文件的前缀 a2.sinks.k1.hdfs.filePrefix = flume2- /#是否按照时间滚动文件夹 a2.sinks.k1.hdfs.round = true /#多少时间单位创建一个新的文件夹 a2.sinks.k1.hdfs.roundValue = 1 /#重新定义时间单位 a2.sinks.k1.hdfs.roundUnit = hour /#是否使用本地时间戳 a2.sinks.k1.hdfs.useLocalTimeStamp = true /#积攒多少个 Event 才 flush 到 HDFS 一次 a2.sinks.k1.hdfs.batchSize = 100 /#设置文件类型,可支持压缩 a2.sinks.k1.hdfs.fileType = DataStream /#多久生成一个新的文件 a2.sinks.k1.hdfs.rollInterval = 600 /#设置每个文件的滚动大小大概是 128M a2.sinks.k1.hdfs.rollSize = 134217700 /#文件的滚动与 Event 数量无关 a2.sinks.k1.hdfs.rollCount = 0 /#最小冗余数 a2.sinks.k1.hdfs.minBlockReplicas = 1 /# Describe the channel a2.channels.c1.type = memory a2.channels.c1.capacity = 1000 a2.channels.c1.transactionCapacity = 100 /# Bind the source and sink to the channel a2.sources.r1.channels = c1 a2.sinks.k1.channel = c1

3)创建 flume-3.conf,用于接收 flume-1 的 event,同时产生 1 个 channel 和 1 个 sink,将数据输送给本地目录

/# Name the components on this agent a3.sources = r1 a3.sinks = k1 a3.channels = c1 /# Describe/configure the source a3.sources.r1.type = avro a3.sources.r1.bind = 172.17.0.2 a3.sources.r1.port = 4142 /# Describe the sink a3.sinks.k1.type = file_roll a3.sinks.k1.sink.directory = /opt/package/flume/test /# Describe the channel a3.channels.c1.type = memory a3.channels.c1.capacity = 1000 a3.channels.c1.transactionCapacity = 100 /# Bind the source and sink to the channel a3.sources.r1.channels = c1 a3.sinks.k1.channel = c1

4)测试：分别开启对应 flume-job(依次启动 flume-3,flume-2,flume-1),同时产生文件变动并观察结果

bin/flume-ng agent --conf conf/ --name a3 --conf-file flume-3.conf bin/flume-ng agent --conf conf/ --name a2 --conf-file flume-2.conf bin/flume-ng agent --conf conf/ --name a1 --conf-file flume-1.conf

注意： 输出的本地目录必须是已经存在的目录,如果该目录不存在,并不会创建新的目录