---
title: "Flume使用案例三"
date: 2018-09-15 20:48:57
draft: false
---
**实时读取目录文件到HDFS**

使用flume监听整个目录的文件

1)创建Job的job_flume2.conf文件
a3.sources = r3 a3.sinks = k3 a3.channels = c3 /# Describe/configure the source a3.sources.r3.type = spooldir a3.sources.r3.spoolDir = /opt/package/flume/test a3.sources.r3.fileSuffix = .COMPLETED a3.sources.r3.fileHeader = true a3.sources.r3.ignorePattern = ([^ ]/*\.tmp) /# Describe the sink a3.sinks.k3.type = hdfs a3.sinks.k3.hdfs.path = hdfs://172.17.0.2:8020/flume/upload/%Y%m%d/%H a3.sinks.k3.hdfs.filePrefix = upload- a3.sinks.k3.hdfs.round = true a3.sinks.k3.hdfs.roundValue = 1 a3.sinks.k3.hdfs.roundUnit = hour a3.sinks.k3.hdfs.useLocalTimeStamp = true a3.sinks.k3.hdfs.batchSize = 100 a3.sinks.k3.hdfs.fileType = DataStream a3.sinks.k3.hdfs.rollInterval = 600 a3.sinks.k3.hdfs.rollSize = 134217700 a3.sinks.k3.hdfs.rollCount = 0 a3.sinks.k3.hdfs.minBlockReplicas = 1 /# Use a channel which buffers events in memory a3.channels.c3.type = memory a3.channels.c3.capacity = 1000 a3.channels.c3.transactionCapacity = 100 /# Bind the source and sink to the channel a3.sources.r3.channels = c3 a3.sinks.k3.channel = c3

2)执行测试

bin/flume-ng agent \ --conf conf/ \ --name a3 \ --conf-file conf/job_flume2.conf

向/opt/package/flume/test中添加文件进行测试

注意：

上传完成的文件会以.COMPLETED结尾，被监控文件夹每 500 毫秒扫描一次文件变动

在HDFS的web界面中如果没有发现有相应的目录生成，可能是因为你监控的目录文件太小，没有达到你编写的.conf文件中的大小要求，所以不会向HDFS发送