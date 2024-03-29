---
title: "Kafka Streams概述"
date: 2022-09-23 15:24:33
draft: false
featured_image: "https://hugo-ys.oss-cn-hangzhou.aliyuncs.com/static/img/kafka1.png"
tags:
- kafka
categories:
- kafka
---
**1.什么是Kafaka Streams**

Kafka Streams： Apache Kafka开源项目的一个组成部分，是一个功能强大，易于使用的库，用于在Kafka上构建高可分布式、拓展性，容错的应用程序

**2.Kafka Streams的优势**

1)功能强大

高扩展性，弹性，容错

2)轻量级

无需专门的集群

一个库，而不是框架

3)完全集成

100%的Kafka 0.10.0版本兼容

易于集成到现有的应用程序

4)实时性

毫秒级延迟

并非微批处理

窗口允许乱序数据

允许迟到数据

**3.为什么要有Kafka Streams**

当前已经有非常多的流式处理系统，最知名且应用最多的开源流式处理系统有Spark Streaming和Apache Storm，Apache Storm

发展多年，应用广泛，提供记录级别的处理能力，当前也支持SQL on Stream，而Spark Streaming基于Apache Spark，可以非

常方便与图计算，SQL处理等集成，功能强大，对于熟悉其它Spark应用开发的用户而言使用门槛低，另外，目前主流的Hadoop

发行版，如Cloudera和Hortonworks，都集成了Apache Storm和Apache Spark，使得部署更容易

既然Apache Spark与Apache Storm拥用如此多的优势，那为何还需要Kafka Stream呢，主要有如下原因：

1)Spark和Storm都是流式处理框架，而Kafka Stream提供的是一个基于Kafka的流式处理类库，框架要求开发者按照特定的方

式去开发逻辑部分，供框架调用。开发者很难了解框架的具体运行方式，从而使得调试成本高，并且使用受限，而Kafka Stream

作为流式处理类库，直接提供具体的类给开发者调用，整个应用的运行方式主要由开发者控制，方便使用和调试。

2)虽然Cloudera与Hortonworks方便了Storm和Spark的部署，但是这些框架的部署仍然相对复杂，而Kafka Stream作为类库，

可以非常方便的嵌入应用程序中，它对应用的打包和部署基本没有任何要求。

3)就流式处理系统而言，基本都支持Kafka作为数据源，例如Storm具有专门的kafka-spout，而Spark也提供专门的spark-

streaming-kafka模块。事实上，Kafka基本上是主流的流式处理系统的标准数据源，换言之，大部分流式系统部署了Kafka，

此时使用Kafka Stream的成本非常低。

4)使用Storm或Spark Streaming时，需要为框架本身的进程预留资源，如Storm的supervisor和Spark on YARN的nodemanager

，即使对于应用实例而言，框架本身也会占用部分资源，如Spark Streaming需要为shuffle和storage预留内存，但是Kafka作为类

库不占用系统资源

5)由于Kafka本身提供数据持久化，因此Kafka Stream提供滚动部署和滚动升级以及重新计算的能力

6)由于Kafka Consumer Rebalance机制，Kafka Stream可以在线动态调整并行度