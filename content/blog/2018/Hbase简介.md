---
title: "Hbase简介"
date: 2018-09-22 20:30:30
draft: false
---
官方网站： [http://hbase.apache.org]()

**1.Hbase的简介**

HBase 的原型是 Google 的 BigTable 论文,受到了该论文思想的启发,目前作为 Hadoop 的子项目来开发维护,用于支持结构化的数据存储
-- 2006 年 Google 发表 BigTable 白皮书
-- 2006 年开始开发 HBase
-- 2008 年北京成功开奥运会,程序员默默地将 HBase 弄成了 Hadoop 的子项目
-- 2010 年 HBase 成为 Apache 顶级项目
-- 现在很多公司二次开发出了很多发行版本,你也开始使用了

**2.Habse的起源**

HBase 一种是作为存储的分布式文件系统,另一种是作为数据处理模型的 MR 框架，因为日常开发人员比较熟练的是结构化的数据进行处理,但是在 HDFS 直接存储的文件往往不具有结构化,所以催生出了 HBase 在 HDFS 上的操作，如果需要查询数据,只需要通过键值便可以成功访问

**3.Hbase中的角色**

1).HMaster

监控 RegionServer

处理 RegionServer 故障转移

处理元数据的变更

处理 region 的分配或移除

在空闲时间进行数据的负载均衡

通过 Zookeeper 发布自己的位置给客户端

2).Regionserver

负责存储 HBase 的实际数据

处理分配给它的 Region

刷新缓存到 HDFS

维护 HLog

执行压缩

负责处理 Region 分片

**4.Hbase的架构**

HBase 内 置 有 Zookeeper ， 但 一 般 我 们 会 有 其 他的 Zookeeper 集 群 来 监 管 master 和regionserver,，Zookeeper 通过选举,保证任何时候，集群中只有一个活跃的 HMaster， HMaster与 HRegionServer 启动时会向 ZooKeeper 注册，存储所有 HRegion 的寻址入口，实时监控HRegionserver 的上线和下线信息。并实时通知给 HMaster,存储 HBase 的 schema 和 table元数据,默认情况下，HBase 管理 ZooKeeper 实例,Zookeeper 的引入使得 HMaster 不再是单点故障，一般情况下会启动两个 HMaster，非 Active 的 HMaster 会定期的和 Active HMaster通信以获取其最新状态,从而保证它是实时更新的，因而如果启动了多个 HMaster 反而增加了 Active HMaster 的负担。一个 RegionServer 可以包含多个 HRegion,每个 RegionServer 维护一个 HLog，和多个 HFiles以及其对应的 MemStore。 RegionServer 运行于 DataNode 上，数量可以与 DatNode 数量一致