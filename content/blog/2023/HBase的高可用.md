---
title: "HBase的高可用"
date: 2018-11-08 16:26:21
draft: false
---
在HBase中HMaster负责监控RegionServer的生命周期，均衡 RegionServer 的负载，那么如果HMaster 挂掉了，那么整个 HBase 集群将不能正常工作，所以HBase中需要对HMaster进行高可用配置

具体步骤如下：

1.关闭HBase集群(如果已关闭则跳过这步)
bin/stop-hbase.sh

2.在HBase中conf目录下创建backup-masters文件

touch backup-masters

3.在 backup-masters 文件中配置高可用 HMaster 节点

vi backup-masters

然后在里面添加另一个HMaster的主机名或ip

4.将backup-masters文件同步到集群其他机器上
scp -r backup-masters cdh1:/opt/package/hbase/conf

5.重新启动HBase集群

6.到WEBUI进行界面查看
0.98 版本之前：http://ip:60010 0.98 版本之后：http://ip:16010