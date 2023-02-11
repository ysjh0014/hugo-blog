---
title: "Spark Streaming中的基本抽象DStream"
date: 2018-10-31 11:05:23
draft: false
---
DStream是Spark Streaming提供的基本抽象，它表示连续的数据流，可以是从源接收的输入数据流，也可以是通过转换输入流生成的已处理数据流。在内部，DStream由一系列连续的RDD表示，这是Spark对不可变分布式数据集的抽象，DStream中的每个RDD都包含来自特定时间间隔的数据，对DStream进行操作就是对RDD进行操作，如下图所示

![](https://img-blog.csdnimg.cn/20181031104142772.png)

举个例子来说明一下，就拿前面一直在用的经典案例wordcount吧，在Spark Streaming中进行wordcount操作时，是通过DStream进行的，每个一定时间产生一个小的批次，这个批次结束了之后DStream并不会结束，而是一直存在的，直到下一个批次产生，一直如此，一个DStream中对应的是一连串的RDD，例如在t1时间内切分压平，与1生成元组，聚合等DStream，都会产生一个RDD，这一连串的RDD，实际上就是一个小的DAG(有向无环图，小DAG是因为DAG中对应的数据少)，小的DAG触发了Action，提交到集群上，t1，t2.....时间点内都是这样运作的，只是RDD中的数据不一样而已，所以一个DStream中就有了多个RDD，每隔一个时间就会生成一个RDD，DStream和DStream之间存在依赖关系，在一个时间点内，存在依赖关系的DStream对应的RDD之间也存在依赖关系

![](https://img-blog.csdnimg.cn/2018103111051037.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==,size_16,color_FFFFFF,t_70)