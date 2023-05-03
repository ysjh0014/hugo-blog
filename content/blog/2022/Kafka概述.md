---
title: "Kafka概述"
date: 2022-09-18 15:26:52
draft: false
featured_image: "https://hugo-ys.oss-cn-hangzhou.aliyuncs.com/static/img/kafka.png"
---
1)Apache Kafka是一个开源消息系统，由Scala写成，是由Apache软件基金会开发的一个开源消息系统项目

2)Kafka最初是由LinkedIn公司开发，并于 2011年初开源，该项目的目标是为处理实时数据提供一个统一、高通量、低等待的平台

3)Kafka是一个分布式消息队列**，**Kafka对消息保存时根据Topic进行归类，发送消息者称为Producer，消息接受者称为Consumer，此外kafka集群有多个kafka实例组成，每个实例(server)成为broker

4)无论是kafka集群，还是producer和consumer都依赖于zookeeper集群保存一些meta信息，来保证系统可用性

在流式计算中，Kafka一般用来缓存数据，Storm通过消费Kafka的数据进行计算

这里举个例子来形象的说明Kafka：

Kafka和消息系统类似，会有生产者和消费者

举例：

这里有做馒头的和你和馒头，做馒头的相当于生产者，你吃馒头相当于消费者，馒头相当于数据流，一般情况下，生产者会一直生产馒头，而你，也就是消费者，会来不及吃馒头，或者卡住，都会造成数据丢失，所以这里需要一个篮子来盛放生产者生产的馒头，你要吃的时候去篮子里取，这里的篮子就相当于Kafka，当一个篮子装不下的时候，多准备几个篮子，这就相当于Kafka的扩容