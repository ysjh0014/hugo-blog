---
title: "hadoop完全分布式搭建"
date: 2018-06-08 18:17:18
draft: false
---
之前的文章都是伪分布式的hadoop集群搭建，而完全分布式集群搭建可以在之前伪分布式的基础上进行修改，但是下面介绍的是完全从零开始搭建的

首先要搭建完全分布式集群，至少要三台机器，这里使用的是三台虚拟机，然后进行规划，例如namenode,resourcemanager要放在哪台机器上，下面是我的三台机器的规划

机器一: 静态ip:192.168.157.110：namenode datanode nodemanager

机器二: 静态ip:192.168.157.111: datanode resourcemanager nodemanager

机器三: 静态ip:192.168.157.112: datanode nodemanager secondarynamenode historyserver

从上边可以看出namenode是在机器一上，resourcemanager是在机器二上，而historyserver是在机器三上，下边就在配置文件中进行相应配置

下面是我的配置文件，这里除了主从节点的配置外还进行了其他的配置，例如日志回收的时间，内存的分配等

core-site.xml:

<property>
<name>fs.defaultFS</name>
<value>hdfs://192.168.157.110:8020</value>
</property>
<property>
<name>hadoop.tmp.dir</name>
<value>/opt/app/hadoop-2.5.0/data/tmp</value>
</property>
<property>
<name>fs.trash.interval</name>
<value>10080</value>

</property>

hdfs-site.xml:

<property>
<name>dfs.namenode.secondary.http-address</name>
<value>192.168.157.112:50090</value>

</property>

slave:

192.168.157.110

192.168.157.111

192.168.157.112

mapred-site.xml:

<property>
<name>mapreduce.framework.name</name>
<value>yarn</value>

</property>

yarn-site.xml:

<property>
<name>yarn.nodemanager.aux-services</name>
<value>mapreduce_shuffle</value>
</property>
<property>
<name> yarn.resourcemanager.hostname</name>
<value>192.168.157.111</value>
</property>
<property>
<name>yarn.log-aggregation-enable</name>
<value>true</value>
</property>
<property>
<name>yarn.log-aggregation.retain-seconds</name>
<value>640800</value>
</property>
<property>
<name>yarn.nodemanager.resource.memory-mb</name>
<value>4096</value>
</property>
<property>
<name>yarn.nodemanager.resource.cpu-vcores</name>
<value>4</value>

</property>

上边的配置好后首先将三台机器进行ssh无密码登录设置，主要就是

ssh-keygen -t rsa

ssh-copy-id ip

这两个命令，主节点为namenode和resourcemanager，所以有主节点的机器要能够无密码登录其他所有机器，然后就可以直接在主节点上启动从节点，注意启动前要记得初始化hdfs