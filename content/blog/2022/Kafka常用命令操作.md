---
title: "Kafka常用命令操作"
date: 2022-09-19 14:12:48
draft: false
---
**1.创建topic(主题)**
bin/kafka-topics.sh --zookeeper cdh0:2181 --create --replication-factor 3 --partitions 3 --topic first

说明：

--topic：定义topic名

--replication-factor： 定义副本数

--partitions： 定义分区数

**2.查看当前服务器中所有topic**
bin/kafka-topics.sh --zookeeper cdh0:2181 --list

**3.删除topic**

bin/kafka-topics.sh --zookeeper cdh0:2181 --delete --topic first

注意：

新版本的可以直接删除，旧版本的Kafka需要server.properties中设置delete.topic.enable=true，否则只是标记删除或者直接重启

**4.发送消息**
bin/kafka-console-producer.sh --broker-list cdh1:9092 --topic test

**5.消费消息**

bin/kafka-console-consumer.sh --bootstrap-server cdh0:9092 --topic test --from-beginning

注意：

--from-beginning： 会把first主题中以往所有的数据都读取出来，应根据业务场景选择是否需要该配置

**6.查看某个topic的详情**
bin/kafka-topics.sh --zookeeper cdh0:2181 --describe --topic test