---
title: "Storm初识"
date: 2018-11-08 19:53:48
draft: false
---
Storm官方网站： [http://storm.apache.org/](http://storm.apache.org/)

Github地址： [https://github.com/apache/storm](https://github.com/apache/storm)

**1.Storm是什么**

Apache Storm是一个免费的开源分布式实时计算系统，是由Twitter产生的，Storm可以轻松可靠地处理无限数据流，实现Hadoop对批处理所做的实时处理

应用场景： 实时分析，在线机器学习，连续计算，分布式RPC，ETL等

Storm能实现高频数据和大规模数据的实时处理

**2.Storm的优势**

1)简单的编程模型

类似于MapReduce降低了并行批处理复杂性，Storm降低了进行实时处理的复杂性

2)可扩展性

计算是在多个线程、进程和服务器之间并行进行的

3)可靠性

Storm保证每个消息至少能得到一次完整处理。任务失败时，它会负责从消息源重试消息

4)容错性

Storm会管理工作进程和节点的故障

5)多语言

你可以在Storm之上使用各种编程语言。默认支持Clojure、Java、Ruby和Python。要
增加对其他语言的支持，只需实现一个简单的Storm通信协议即可