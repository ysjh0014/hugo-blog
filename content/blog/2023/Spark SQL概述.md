---
title: "Spark SQL概述"
date: 2018-10-17 11:10:53
draft: false
---
**1.什么是Spark SQL**

Spark SQL是Spark用来处理结构化数据的一个模块，它提供了一个编程抽象叫做DataFrame并且作为分布式SQL查询引擎的作用，将SQL解析成特殊的RDD(DataFrame)，然后在Spark集群上运行

**2.为什么要学习Spark SQL**

我们已经学习了Hive，它是将Hive SQL转换成MapReduce然后提交到集群上执行，大大简化了编写MapReduce的程序的复杂性，由于MapReduce这种计算模型执行效率比较慢。所以有Spark SQL的应运而生，它是将Spark SQL转换成RDD，然后提交到集群执行，执行效率非常快

**3.Spark SQL的特点**

1)易整合，可以使用SQL或者DataFrame API

2)统一的数据访问方式，以相同方式连接到任何数据源(Hive，Avro，Parquet，ORC，JSON和JDBC)

3)Hive集成，Spark SQL支持HiveQL语法以及Hive SerDes和UDF，允许访问现有的Hive仓库

4)提供标准的连接(JDBC和ODBC)

**4.DataFrame**

1)什么是DataFrame

与RDD类似，DataFrame也是一个分布式数据容器。然而DataFrame更像传统数据库的二维表格，除了数据以外，还记录数据的结构信息，即schema。同时，与Hive类似，DataFrame也支持嵌套数据类型（struct、array和map）。从API易用性的角度上 看，DataFrame API提供的是一套高层的关系操作，比函数式的RDD API要更加友好，门槛更低。由于与R和Pandas的DataFrame类似，Spark DataFrame很好地继承了传统单机数据分析的开发体验

2)RDD与DataFrame的区别

DataFrame里面存放的结构化数据的描述信息，DataFrame要有表头（表的描述信息），描述了有多少列，每一列数叫什么字、什么类型、能不能为空

DataFrame是特殊的RDD（普通的RDD+Schema信息就变成了DataFrame）