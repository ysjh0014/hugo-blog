---
title: "实时计算Spark Streaming初识"
date: 2018-10-28 09:20:30
draft: false
---
Spark Streaming是核心Spark API的扩展，可实现实时数据流的可扩展，高吞吐量，容错流处理。数据可以从许多来源(如Kafka，Flume，Kinesis或TCP套接字)中获取，并且可以使用以高级函数表示的复杂算法进行处理map，例如reduce，join和window。最后，处理后的数据可以推送到文件系统，数据库和实时仪表板。实际上，也可以在数据流上应用Spark的机器学习和图形处理算法

![](https://img-blog.csdnimg.cn/20181028091002337.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==,size_27,color_FFFFFF,t_70)

Spark Streaming接收实时输入数据流并将数据分成批处理，然后由Spark引擎处理，以批量生成最终结果流

可以看出，Spark Streaming是微批次处理架构，跟Storm的完全流式处理是不同的，Storm是完全流式处理，延迟低，对于实时性要求特别高的场景有适合，而Spark的优势是吞吐量大，并且有一个完整的生态，响应时间也可以接受，并且在编程以及后续维护上要比Storm容易得多，只能说各有各的优势吧

实时计算就是一直会执行，除非发生故障或者人为把它停掉，而Spark Streaming实时计算是分成很小的批次进行处理的，在Driver端不停的提交任务，一定的时间间隔读取一个小的批次，然后将这个小的数据提到集群，只要不停的产生批次，不停的提交就是一个实时计算程序了，之前在Spark中提交的离线任务是一个大的RDD(RDD中对应的数据比较多)，现在产生的是很多小的RDD(每隔一个固定时间产生一个小的RDD)

Spark Streaming的底层其实也是基于我们之前讲解的Spark Core的，基本的计算模型还是基于内存的大数据实时计算模型，而且，它的底层的组件或者叫做概念，其实还是最核心的RDD，只不过针对实时计算的特点，在RDD之上，进行了一层封装，叫做DStream。其实，学过了Spark SQL之后，你理解这种封装就容易了。之前学习Spark SQL是不是也是发现，它针对数据查询这种应用，提供了一种基于RDD之上的全新概念，DataFrame，但是，其底层还是基于RDD的。所以，RDD是整个Spark技术生态中的核心。要学好Spark在交互式查询、实时计算上的应用技术和框架，首先必须学好Spark核心编程，也就是Spark Core