---
title: "Spark概述"
date: 2018-10-08 19:39:39
draft: false
---
**1.什么是Spark**

Spark官方网站： [http://spark.apache.org/](http://spark.apache.org/)

Spark是一种快速、通用、可扩展的大数据分析引擎，2009年诞生于加州大学伯克利分校AMPLab，2010年开源，2013年6月成为Apache孵化项目，2014年2月成为Apache顶级项目。目前，Spark生态系统已经发展成为一个包含多个子项目的集合，其中包含SparkSQL、Spark Streaming、GraphX、MLlib等子项目，Spark是基于内存计算的大数据并行计算框架。Spark基于内存计算，提高了在大数据环境下数据处理的实时性，同时保证了高容错性和高可伸缩性，允许用户将Spark部署在大量廉价硬件之上，形成集群

**2.为什么要学Spark**

Spark是Mapreduce的替代方案，而且兼容HDFS，Hive，可以融合到Hadoop的生态系统中去，以弥补Mapreduce的不足

Hadoop： 两步计算，磁盘存储 Spark： 多步计算，内存存储

中间结果输出： 基于MapReduce的计算引擎通常会将中间结果输出到磁盘上，进行存储和容错。出于任务管道承接的考虑，当一些查询翻译到MapReduce任务时，往往会产生多个Stage，而这些串联的Stage又依赖于底层文件系统（如HDFS）来存储每一个Stage的输出结果