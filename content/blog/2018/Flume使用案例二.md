---
title: "Flume使用案例二"
date: 2018-09-15 18:16:27
draft: false
---
之前的案例一只算是一个小demo，在实际生产环境中是不会用Flume来做这种需求的

**实时监控hive日志，并上传到HDFS中**

1)拷贝Hadoop相关jar包到Flume的lib目录下（将监控日志实时上传到HDFS中，相当于持有Hadoop的API，才能操作Hadoop）
hadoop/common/lib/hadoop-auth-2.5.0-cdh5.3.6.jar hadoop/common/lib/commons-configuration-1.6.jar hadoop/mapreduce1/lib/hadoop-hdfs-2.5.0-cdh5.3.6.jar hadoop/common/hadoop-common-2.5.0-cdh5.3.6.jar

将上边所属目录下的jar包拷贝到Flume的lib目录下

上边目录下的jar包是HadoopCDH5.3.6版本的，不同版本的Hadoop上边的jar包所在位置不一样，可以根据以下命令在Hadoop目录下查找
find ./ -name 'hadoop-auth/*'

你会发现相同的jar包在不同的目录下都存在，这时候只需要拷贝其中一份到Flume的lib目录即可

注意： 以下的jar为1.99版本的Flume必须引用的jar
hadoop/hdfs/lib/htrace-core-3.1.0-incubating.jar hadoop/hdfs/lib/commons-io-2.4.jar

2)编写Job的flume-hdfs.conf文件

/# Name the components on this agent a2.sources = r2 a2.sinks = k2 a2.channels = c2 /# Describe/configure the source a2.sources.r2.type = exec a2.sources.r2.command = tail -F /tmp/root/hive.log a2.sources.r2.shell = /bin/bash -c /# Describe the sink a2.sinks.k2.type = hdfs a2.sinks.k2.hdfs.path = hdfs://cdh0:8020/flume/%Y%m%d/%H /#上传文件的前缀 a2.sinks.k2.hdfs.filePrefix = logs- /#是否按照时间滚动文件夹 a2.sinks.k2.hdfs.round = true /#多少时间单位创建一个新的文件夹 a2.sinks.k2.hdfs.roundValue = 1 /#重新定义时间单位 a2.sinks.k2.hdfs.roundUnit = hour /#是否使用本地时间戳 a2.sinks.k2.hdfs.useLocalTimeStamp = true /#积攒多少个Event才向HDFSflush一次，这个值不能超过channels的阈值 a2.sinks.k2.hdfs.batchSize = 1000 /#设置文件类型,可支持压缩(DataStream指不支持压缩) a2.sinks.k2.hdfs.fileType = DataStream /#多久生成一个新的文件(这里最好让每个文件的大小接近127M) a2.sinks.k2.hdfs.rollInterval = 600 /#设置每个文件的滚动大小(为0则不设置此规则) a2.sinks.k2.hdfs.rollSize = 134217700 /#文件的滚动按照数据量的个数，为0则表示与Event数量无关 a2.sinks.k2.hdfs.rollCount = 0 /#最小副本数，为1表示不产生副本，这里如果不这样设置的话，前面的滚动设置都不生效 a2.sinks.k2.hdfs.minBlockReplicas = 1 /# Use a channel which buffers events in memory a2.channels.c2.type = memory a2.channels.c2.capacity = 1000 a2.channels.c2.transactionCapacity = 100 /# Bind the source and sink to the channel a2.sources.r2.channels = c2 a2.sinks.k2.channel = c2

注意：

tail -F

上边命令是用来查看实时文件的，默认查看最后10行数据，-F是指当查看实时文件失败的时候重新查看，-f则是指在查看失败后不重新查看，tail -F的命令表面是由Flume执行的，实际是由java中的Runtime().getXXX.exec("tail -F XXXx")执行的

3)执行
bin/flume-ng agent \ --conf conf/ \ --name a2 \ --conf-file conf/job_flume1.conf