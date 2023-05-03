---
title: "Kafka中Broker保存消息的方式"
date: 2022-09-19 18:34:01
draft: false
featured_image: "https://hugo-ys.oss-cn-hangzhou.aliyuncs.com/static/img/kafka.png"
---
**1.存储方式**

物理上把topic分成一个或多个patition(对应 server.properties 中的num.partitions=3配置)，每个patition物理上对应一个文件

(该文件夹存储该patition的所有消息和索引文件)

**2.存储策略**

无论消息是否被消费，kafka都会保留所有消息。有两种策略可以删除旧数据：

1)基于时间：log.retention.hours=168

2)基于大小：log.retention.bytes=1073741824

需要注意的是，因为Kafka读取特定消息的时间复杂度为O(1)，即与文件大小无关，所以这里删除过期文件与提高 Kafka 性能无关

注意：

producer不在zookeeper中注册，消费者在zookeeper中注册