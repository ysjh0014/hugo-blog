---
title: "Storm核心概念"
date: 2018-11-12 16:38:34
draft: false
---
简单讲解：

Topology：计算拓扑，由Spouts和Bolts组成，将整个流程串起来

Stream：流，数据流，水流，是一个抽象概念，由没有边界的Tuple组成

Spout：产生数据/水的东西，消息流的源头，Topology的消息生产者

Bolt：处理数据/水的东西 水壶/水桶，消息处理单元，可以做过滤，聚合，查询/写数据库等操作

Tuple：数据/水，传递的基本单元

例如Storm官网首页的这张图：

![](https://img-blog.csdnimg.cn/20181110100138295.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==,size_16,color_FFFFFF,t_70)

水龙头是Spout，产生水的，水滴就是Tuple，水滴会传递到Bolt，用来处理数据，这里会有很多个Bolt，Bolt又会传递到下一个Bolt，这个Bolt之间的传递就是Stream，数据流，而整个流程就是拓扑Topology

官方文档地址：

[http://storm.apache.org/releases/2.0.0-SNAPSHOT/Concepts.html](http://storm.apache.org/releases/2.0.0-SNAPSHOT/Concepts.html)

Topology：

实时应用程序的逻辑被打包到Storm拓扑中，Storm拓扑类似于MapReduce作业，一个关键的区别是MapReduce作业最终完成，而拓扑结构永远运行(除非杀死它)

Stream：

流是Storm中的核心抽象，流是无限的元组序列，以分布式方式并行处理和创建，流定义了一个模式，该模式命名流的元组中的字段。默认情况下，元组可以包含整数，长整数，短整数，字节，字符串，双精度数，浮点数，布尔值和字节数组。还可以定义自己的序列化程序，以便可以在元组中本机使用自定义类型

Spout：

spout是拓扑中的流的来源。通常，spouts将从外部源读取元组并将它们发送到拓扑中

Spouts可以发出多个流

Bolts：

拓扑中的所有处理都是用bolt完成的，Bolts可以执行任何操作，包括过滤，函数，聚合，连接，数据库操作等

bolt可以进行简单的流转换，进行复杂的流转换通常需要多个步骤，因此需要多个bolt