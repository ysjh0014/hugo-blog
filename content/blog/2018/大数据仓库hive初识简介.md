---
title: "大数据仓库hive初识简介"
date: 2018-06-17 18:05:03
draft: false
---
hive是FaceBook实现并开源的用于解决海量结构化日志的数据统计，是为了解决Mapreduce编程的不便性以及成本高的问题，可以简化操作

什么是hive:

处理的数据储存在HDFS上

底层分析数据的实现是Mapreduce

执行程序运行在yarn上

hive是基于hadoop的一个数据仓库工具，可以将结构化的数据文件映射成一张表，并提供类SQL查询功能(HQL),本质是将HQL转化为Mapreduce程序

hive处理的数据存储在hdfs上，执行是在yarn上边，不参与数据的存储和处理，所以hive是大数据的仓库而不是数据库

hive将结构化的数据文件，比如日志文件映射成一张表，这张表的名称，字段，字段类型等，以及映射的数据文件在hdfs的位置，这些信息成为表的元数据，元数据一般存储在关系型数据库(mysql)中

hive如何将HSQL转化为Mapreduce:

通过驱动器: Driver

包含解析器 优化器 编译器 执行器

解析器: 将HSQL语句进行解析,比如表是否存在，字段是否存在，HSQL语句是否有误

优化器: 因为实现相同功能的HSQL语句的效率是不一样的，所以会进行基本的优化

编译器: 生成一个逻辑的执行计划

执行器: 将逻辑计划转换成可以运行的物理计划

hive的优点:

/*操作接口采用类SQL语法，简化开发

/*避免写Mapreduce，减少学习成本

/*统一的元数据管理，可与impala/spark等共享元数据

/*易扩展(支持自定义函数，可以扩展集群规模)

hive的使用场景:

/*数据的离线处理: 比如日志分析，海量结构化数据离线分析

/*hive的执行延迟比较高，因此常用于数据分析的对实时性要求不高的场合

/*hive优势在于处理大数据，对于小数据没有优势