---
title: "并行计算框架Mapreduce简介"
date: 2018-05-31 13:18:15
draft: false
---
hadoop的三个组件，先有mapreduce(分布式计算模型)，后有hdfs,知道hadoop才有了yarn，因此掌握mapreduce很有必要，虽然现在都是使用流式处理框架，如storm,spark等，但是这几种框架的思想及原理都来源于mapreduce

Mapreduce:

思想:分而治之：map(映射)--->对每一部分的数据进行处理,可以高度并行(最核心的部分)

reduce(化简)--->合并

过程: input---map----reduce----output

在这个过程中数据的传输形式是<key,value>对在流通，map和reduce中都是<key,value>对的形式

输入的时候会默认去解析输入数据成<key,value>,例如下面的一组输入内容:

hadoop hadoop ---> <0,hadoop hadoop>
hadoop hdfs ---> <13,hadoop hdfs>
hdfs ys

hadoop yarn

hdfs mapresuce

mapreduce框架默认的会进行split分割操作，如上边所示，每一行就是一个<key,value>对，其中key是偏移量，value是该数据项,然后会交给map进行处理，例如第一行的数据，map后会变成<hadoop,1> <yarn,1>这两个<key,value>对

map--->shuffle--->reduce,map到reduce中间有一个很复杂的过程shuffle(洗牌)，它将map中传来的<key,value>对进行分组，将相同key的value合并在一起，放在一个集合中，然后再交给reduce处理，如下图

![](https://img-blog.csdn.net/20180530200500766)