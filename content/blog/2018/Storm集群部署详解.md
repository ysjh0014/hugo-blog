---
title: "Storm集群部署详解"
date: 2018-11-09 20:01:50
draft: false
---
**1.集群规划**

cdh0 storm zookeeper

cdh1 storm zookeeper

cdh2 storm zookeeper

**2.基础环境搭建**

jdk7+

python2.6.6+

**3.配置文件的修改**

storm-env.sh
export JAVA_HOME=你自己的jdk的目录

storm.yaml

storm.zookeeper.servers: - "cdh0" - "cdh1" - "cdh2" storm.local.dir: "/opt/package/storm/app/storm" supervisor.slots.ports: - 6700 - 6701 - 6702 - 6703

第一个storm.zookeeper.servers是zookeeper的主机列表，需要修改为你自己的

第二个storm.local.dir是Nimbus和Supervisor守护进程需要本地磁盘上的目录来存储少量状态，所以需要创建该目录

第三个supervisor.slots.ports是slots的个数即端口

具体配置请查看官网： [http://storm.apache.org/releases/2.0.0-SNAPSHOT/Setting-up-a-Storm-cluster.html](http://storm.apache.org/releases/2.0.0-SNAPSHOT/Setting-up-a-Storm-cluster.html)

注意：

这里的storm.yaml配置文件使用的是yaml语法，需严格按照语法来写，空格等不要少写，yaml语法可以自行百度

**4.分发storm**
scp -r xxx cdh0:xxx

**5.启动storm各个进程**

在我的文章 [Storm单机版部署及讲解](https://blog.csdn.net/ys_230014/article/details/83868373) 中又详细的说明

**6.使用jps查看**