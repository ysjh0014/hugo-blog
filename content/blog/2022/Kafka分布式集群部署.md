---
title: "Kafka分布式集群部署"
date: 2022-09-19 13:21:21
draft: false
featured_image: "https://hugo-ys.oss-cn-hangzhou.aliyuncs.com/static/img/kafka.png"
tags:
- kafka
categories:
- kafka
---
Kafka下载地址： [http://mirrors.tuna.tsinghua.edu.cn/apache/kafka/2.0.0/kafka_2.12-2.0.0.tgz](http://mirrors.tuna.tsinghua.edu.cn/apache/kafka/2.0.0/kafka_2.12-2.0.0.tgz)

1.上传解压

下载好Kafka的压缩包之后上传到机器上，并解压

2.在Kafka目录下创建log目录，用来存放kafka的日志文件

3.修改配置文件
`cd config vi server.properties`

修改如下：

broker.id=0 log.dirs=上边你创建的的log目录地址 zookeeper.connect=cdh0:2181,cdh1:2181,cdh2:2181

注意：

broker.id不能重复

这里下载的Kafka是2.0版本的，所以有些配置已经不需要修改了，在老版本中还需要修改hostname这个配置

4.分发到其他集群机器上
scp -r kafka的目录地址 cdh1:要分发到其他机器节点的目录位置

5.修改机器节点配置

修改其他机器节点上配置文件server.properties中的broker.id为1和2

6.启动Kafka

分别在集群机器上启动Kafka
`bin/kafka-server-start.sh config/server.properties`

7.关闭Kafaka

`bin/kafka-server-stop.sh stop`

拓展：

这里如果没有集群，可以在单节点机器上部署分布式，只需要将配置文件server.properties复制几份，然后在启动的时候指定好配置文件的位置即可