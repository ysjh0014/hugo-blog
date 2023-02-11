---
title: "Hadoop集群的ResourceManager HA高可用配置"
date: 2018-11-21 21:27:26
draft: false
---
ResourceManager HA

yarn-site.xml：
<configuration> <!-- Site specific YARN configuration properties --> <property> <name>yarn.nodemanager.aux-services</name> <value>mapreduce_shuffle</value> </property> <property> <name> yarn.resourcemanager.hostname</name> <value>172.17.0.3</value> </property> <property> <name>yarn.log-aggregation-enable</name> <value>true</value> </property> <property> <name>yarn.log-aggregation.retain-seconds</name> <value>640800</value> </property> <property> <name>yarn.nodemanager.resource.memory-mb</name> <value>4096</value> </property> <property> <name>yarn.nodemanager.resource.cpu-vcores</name> <value>4</value> </property> <property> <name>yarn.log.server.url</name> <value>http://cdh0:19888/jobhistory/logs/</value> </property> <property> <name>yarn.resourcemanager.ha.enabled</name> <value>true</value> </property> <!--声明两台 resourcemanager 的地址--> <property> <name>yarn.resourcemanager.cluster-id</name> <value>cluster-yarn1</value> </property> <property> <name>yarn.resourcemanager.ha.rm-ids</name> <value>cdh1,cdh2</value> </property> <property> <name>yarn.resourcemanager.hostname.cdh1</name> <value>cdh1</value> </property> <property> <name>yarn.resourcemanager.hostname.cdh2</name> <value>cdh2</value> </property> <property> <!--指定 zookeeper 集群的地址--> <name>yarn.resourcemanager.zk-address</name> <value>cdh0:2181,cdh1:2181,cdh2:2181</value> </property> <property> <!--启用自动恢复--> <name>yarn.resourcemanager.recovery.enabled</name> <value>true</value> </property> <property> <!--指定 resourcemanager 的状态信息存储在 zookeeper 集群--> <name>yarn.resourcemanager.store.class</name> <value>org.apache.hadoop.yarn.server.resourcemanager.recovery.ZKRMStateStore</value> </property> </configuration>

使用scp命令拷贝给其他两台机器

在cdh1中启动ResourceManager和NodeManager
sbin/start-yarn.sh

在cdh2中启动ResourceManager

sbin/yarn-daemon.sh start resourcemanager

查看cdh2中的服务

bin/yarn rmadmin -getServiceState cdh2

至此ResourceManager HA完成