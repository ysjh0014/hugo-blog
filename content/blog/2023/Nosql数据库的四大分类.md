---
title: "Nosql数据库的四大分类"
date: 2018-08-01 15:39:58
draft: false
---
Nosql(not only sql)数据库基本可以分为四大类:

1.KV键值

BerkeleyDB redis tair memcache.......

2.文档型数据库(bson格式比较多)

CouchDB

MongoDB: 是一个基于分布式文件存储的数据库，由c++语言编写，旨在为WEB应用提供可扩展的高性能数据存储解决方案

介于关系数据库与非关系数据库之间，是非关系型数据库中功能最丰富，最像关系型数据库的

3.列存储数据库

Cassandra Hbase(大数据Hadoop中应用的分布式数据库系统，我的博客也有相关的文章)

4.图关系数据库

Neo4J InfoGrid

图关系数据库不是放图形的，放的是关系，比如: 朋友圈社交网络，广告推荐系统，社交网络，推荐系统等，专注于构建关系图谱