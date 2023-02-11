---
title: "Hbase读写数据流程"
date: 2018-09-23 10:37:28
draft: false
---
**1.读数据流程**

1)HRegionServer 保存着 meta 表以及表数据,要访问表数据,首先 Client 先去访问zookeeper,从 zookeeper 里面获取 meta 表所在的位置信息,即找到这个 meta 表在哪个HRegionServer 上保存着

2)接着 Client 通过刚才获取到的 HRegionServer 的 IP 来访问 Meta 表所在的HRegionServer,从而读取到 Meta,进而获取到 Meta 表中存放的元数据

3)Client 通过元数据中存储的信息,访问对应的 HRegionServer,然后扫描所在HRegionServer 的 Memstore 和 Storefile 来查询数据

4)最后 HRegionServer 把查询到的数据响应给 Client

**2.写数据流程**

1)Client 也是先访问 zookeeper,找到 Meta 表,并获取 Meta 表信息

2)确定当前将要写入的数据所对应的 RegionServer 服务器和 Region

3)Client 向该 RegionServer 服务器发起写入数据请求,然后 RegionServer 收到请求并响应

4)Client 先把数据写入到 HLog,以防止数据丢失

5)然后将数据写入到 Memstore

6)如果 Hlog 和 Memstore 均写入成功,则这条数据写入成功。在此过程中,如果 Memstore达到阈值,会把 Memstore 中的数据 flush 到 StoreFile 中

7)当 Storefile 越来越多,会触发 Compact 合并操作,把过多的 Storefile 合并成一个大的Storefile，当 Storefile 越来越大,Region 也会越来越大,达到阈值后,会触发 Split 操作,将 Region 一分为二

注意： 因为内存空间是有限的,所以说溢写过程必定伴随着大量的小文件产生