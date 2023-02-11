---
title: "Hbase的基本特性及工作机制"
date: 2018-07-20 13:50:00
draft: false
---
### 基本特性:

Hbase是一种nosql数据库，是一种分布式数据库系统,可以提供数据的实时随机读写

数据的最终持久化存储是基于hdfs的，特点是可以随时实现在线扩容

数据的增删改查模块是基于分布式系统的

Hbase数据库与关系型数据库不一样:

关系型数据库的表结构是字段名，下面存储的是字段值，而Hbase数据库没有这些，Hbase的表结构是rowkey(行键)，行键下面是key-value值，Hbase数据库允许将众多的key-value值分为几个大类，叫做列族，因此Hbase数据库的表结构一般分为: rowkey(行键)，列族，key-value值

Hbase数据库中的key-value中的value值允许有多个版本，在传统的关系型数据库中会直接覆盖掉原来的值，但是在Hbase数据库中会保存原来的值，只是会给他们一个时间戳，新加入的时间戳更新，在查询时如果不特别指出，会默认查询出时间戳最新的那个value值，但是也可以查询以前的value值

### 工作机制:

Hbase的数据都是存储在hdsf上的，是可以无限存储的，只要添加集群容量就可以，由于是分布式数据库，Hbase服务器肯定也是很多台，那么就需要分任务给不同的服务器，按照数据库行键的范围(region)来分，比如一号服务器负责0000-9999的一个region,然后不同的服务器负责不同的查找范围，因此在hdfs中的目录存放结构也不是在一个目录下，而是一个一张表在一个目录下，但是里面又分为不同的region目录，这样可以避免datanode频繁的io操作

由于是分布式就存在机器节点挂掉的情况，如果某一台region server挂掉了，那么就需要一个master的角色，来监控着region server，如果有region server挂掉了，那么master就会将他的任务分配到别的机器节点上，Hbase是用zookeeper来做协作服务的， region server实时的向zookeeper注册信息，为了防止master挂掉，也可以配置多个master

整个流程如下图所示:

![](https://img-blog.csdn.net/20180720134912194?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)