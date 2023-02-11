---
title: "Spark高可用集群安装"
date: 2018-10-10 19:07:15
draft: false
---
在之前的文章[Spark集群安装](https://blog.csdn.net/ys_230014/article/details/82973339)中，已经详细的介绍了Spark分布式集群的安装方法

Spark集群启动后执行jps命令，主节点上有Master进程，其他子节点上有Work进行，但是有一个很大的问题，那就是Master节点存在单点故障，要解决此问题，就要借助zookeeper，并且启动至少两个Master节点来实现高可靠

具体实现步骤如下：

**1.Spark集群规划**

这里有三台主机： cdh0 cdh1 cdh2

cdh0和cdh1是Master，cdh0 cdh1 cdh2是Worker

**2.停止Spark集群，修改配置文件**

1)修改spark-env.sh

在其中添加：
export SPARK_DAEMON_JAVA_OPTS="-Dspark.deploy.recoveryMode=ZOOKEEPER -Dspark.deploy.zookeeper.url=cdh0,cdh1cdh2 -Dspark.deploy.zookeeper.dir=/spark"

-Dspark.deploy.zookeeper.url=zk1,zk2,zk3是zookeeper所在机器地址，默认端口号是2181可以不用写，如果端口号修改过就需要加上端口号，-Dspark.deploy.zookeeper.dir=/spark是在zookeeper中创建spark目录用来存储信息，保存活跃Master的信息，所有Worker的资源信息和资源使用情况，为了能够进行故障切换

2)修改slaves

在其中加上你自己的Worker节点机器主机名或者ip地址

**3.启动Zookeeper集群**

**4.在一台机器上使用**
sbin/start-all.sh

来启动Spark集群，这时候只有一个Master，然后在另外任意一台Worker节点机器上使用

sbin/start-master.sh

再启动一个Master

最后你可以通过WEBUI进行观察，之前有一个Master的时候，WEBUI上的status是ALIVE(活跃)状态，现在有两个Master，你切换到上边单独启动的那个Master的WEBUI上，可以看到是 STANDBY(备用)状态，只有当活跃状态的Master挂掉了才能使用备用状态的Master