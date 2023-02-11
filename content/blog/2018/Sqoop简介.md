---
title: "Sqoop简介"
date: 2018-09-12 20:57:04
draft: false
---
Apache Sqoop(TM)是一种旨在有效地在 Apache Hadoop 和诸如关系数据库等结构化数据存储之间传输大量数据的工具。
Sqoop 于 2012 年 3 月孵化出来,现在是一个顶级的 Apache 项目

最新的稳定版本是1.4.7，1.99.7与1.4.7不兼容且功能不完整，不适用于生产部署

Sqoop官方网站： [http://sqoop.apache.org/](http://sqoop.apache.org/)

Sqoop：(SQL TO HADOOP)

这里的意思并不是像hive一样使用sql来操作hadoop,而是将传统的RDBMS数据库中的数据导入到hadoop中

RDBMS-------》Hadoop 数据导入过程 import

Hadoop-------》RDBMS 数据导出过程 export

export只有在将数据结果导出到传统数据库的时候会用到，其他时候用的不多

Sqoop是将导入或导出命令翻译成 mapreduce 程序来实现。
在翻译出的 mapreduce 中主要是对 inputformat 和 outputformat 进行定制