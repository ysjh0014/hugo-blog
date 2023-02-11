---
title: "Flume初识"
date: 2018-09-14 15:18:56
draft: false
---
Flume官方文档： [http://flume.apache.org/FlumeUserGuide.html](http://flume.apache.org/FlumeUserGuide.html)

**1.Flume简介**

1)Flume 提供一个分布式的,可靠的,对大数据量的日志进行高效收集、聚集、移动的服务
2)Flume 只能在 Unix 环境下运行
3)Flume 基于流式架构,容错性强,也很灵活简单
4)Flume、Kafka 用来实时进行数据收集,Spark、Storm 用来实时处理数据,impala 用来实时查询

**2.Flume中的角色**

![](https://img-blog.csdn.net/20180914151413352?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

Source：

用于采集数据, Source 是产生数据流的地方,同时 Source 会将产生的数据流传输到 Channel，这个有点类似于 Java IO 部分的 Channel

Channel：

用于桥接 Sources 和 Sinks,类似于一个队列

Sink：

从 Channel 收集数据，将数据写到目标源(可以是下一个 Source,也可以是 HDFS 或者 HBase)

Event：

传输单元,Flume 数据传输的基本单元,以事件的形式将数据从源头送至目的地

**3.Flume传输过程**

source 监控某个文件或数据流,数据源产生新的数据,拿到该数据后,将数据封装在一个Event 中,并 put 到 channel 后 commit 提交,channel 队列先进先出,sink 去 channel 队列中拉取数据,然后写入到 HDFS 中