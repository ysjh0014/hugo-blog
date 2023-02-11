---
title: "Spark2.0新特性"
date: 2018-10-23 21:54:55
draft: false
---
**1.更简单**

支持标准的SQL和简化的API

Spark 2.0依然拥有标准的SQL支持和统一的DataFrame/Dataset API，但我们扩展了Spark的SQL 性能，引进了一个新的ANSI SQL解析器并支持子查询，Spark 2.0可以运行所有的99个TPC-DS的查询，这需要很多的SQL：2003个功能

在编程API方面，我们已经简化了API

统一Scala/Java下的DataFrames 和 Datasets

SparkSession

更简单、更高性能的Accumulator API

基于DataFrame的Machine Learning API 将成为主要的ML API

Machine Learning 管道持久性

R中的分布式算法

**2.更快**

Spark 2.0将拥有更快的速度，下图是Spark 2.0和Spark 1.6的速度对比图

![](https://img-blog.csdn.net/20181023215357781?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

**3.更智能**

通过在DataFrames之上构建持久化的应用程序来不断简化数据流，允许我们统一数据流，支持交互和批量查询