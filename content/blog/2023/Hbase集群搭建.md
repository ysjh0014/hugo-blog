---
title: "Hbase集群搭建"
date: 2018-08-10 14:19:40
draft: false
---
前面已经对Hbase进行过介绍，Hbase是存储在HDFS上的，并且由zookeeper进行管理的，

因此安装准备如下:

一个hadoop集群

一个zookeeper集群,这里重点是讲Hbase集群的搭建，所以默认你已经有了hadoop集群和zookeeper集群，并且已经全部运行了

我这里用的是CDH5.3.6,所以不用考虑兼容性问题，

角色分配如下：
机器一： namenode datanode regionserver hmaster zookeeper 机器二： datanode regionserver zookeeper 机器三： datanode regionserver zookeeper

首先当然是上传解压，，然后修改配置文件

**hbase-env.sh**
export JAVA_HOME=/opt/jdk1.8.0_161 /*这里换成你自己的jdk路径 export HBASE_MANAGES_ZK=false /*这里改为false是禁用hbase自带的zookeeper，使用外部的zookeeper, 因为zookeeper不仅要监控hbase,还要监控其他的

**hbase-site.xml**

<property> <name>hbase.zookeeper.property.dataDir</name> <value>/opt/zookeeper-3.4.5-cdh5.3.6/data/zkData</value> /*这个是你的zookeeper集群配置文件里面dataDir的路径 </property> <property> <name>hbase.rootdir</name> <value>hdfs://192.168.83.110:8020/hbase</value> </property> <property> <name>hbase.cluster.distributed</name> <value>true</value> </property> <property> <name>hbase.zookeeper.quorum</name> <value>cdh0,cdh1,cdh2</value> </property>

**regionserver**

cdh0 cdh1 cdh2

配置好之后就可以启动hbase集群了，当然前提是你的hadoop和zookeeper已经准备好了

bin/start-hbase.sh

![](https://img-blog.csdn.net/20180810141221125?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

Hbase的WEB UI界面： ip：60010

使用hbase的命令行客户端查看hbase是否能够正常运行

bin/hbase shell

![](https://img-blog.csdn.net/20180810141331635?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

list: 查看表

![](https://img-blog.csdn.net/20180810141502563?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

status: 查看集群状态

![](https://img-blog.csdn.net/20180810141520278?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

version: 查看集群版本

如果在命令行客户端中出现错误，就进入zookeeper把hbase删除，然后重新启动hbase，具体步骤如下

在zookeeper目录下

bin/zkCli.sh

![](https://img-blog.csdn.net/20180810141800285?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

ls /

rmr hbase

![](https://img-blog.csdn.net/20180810141909237?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

然后重新启动Hbase集群