---
title: "MapReduce框架的数据类型"
date: 2018-06-01 14:13:29
draft: false
---
在之前的文章[WordCount编程及执行流程](https://blog.csdn.net/ys_230014/article/details/80531180)的源码中可以看出，wordcount程序中并没有使用java原生或者封装的数据类型，而是使用Text,LongWritable,IntWritable之类的数据类型，下面就介绍一下MapReduce框架的数据类型

/*该数据类型都实现Writable接口，以便用这些类型定义的数据可以被序列化进行网络传输和文件存储

/*基本的数据类型:

IntWritable: 整数型

DoubleWritable: 双字节数值

FloatWritable: 浮点数

LongWritable: 长整形数

Text: 使用utf-8格式存储的文本，相当于String

BooleanWritable: 布尔型数值

ByteWritable: 单字节数值

NullWritable: 当<key,value>中的key或value为空时使用

Writable接口中有两个方法，write()和readFields()方法，write()把每个对象序列化到输入流，readFields()方法把输入流字节反序列化