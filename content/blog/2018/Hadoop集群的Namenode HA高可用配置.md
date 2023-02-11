---
title: "Hadoop集群的Namenode HA高可用配置"
date: 2018-11-21 21:16:48
draft: false
---
HA配置首先要有zookeeper集群，这里就不再说明zookeeper集群的搭建了，可以在我的前面的文章中找到

我这里是在之前Hadoop单点的基础上进行HA配置的

集群HA规划：

cdh0： Namenode Datanode JournalNode NodeManager ZK

cdh1：Namenode Datanode JournalNode ResourceManager NodeManager ZK

cdh2：Datanode ResourceManager NodeManager JournalNode ZK

Namenode HA

hdfs-site.xml：
<configuration> <property> <name>dfs.namenode.secondary.http-address</name> <value>172.17.0.4:50090</value> </property> <property> <!-- 指定数据冗余份数 --> <name>dfs.replication</name> <value>3</value> </property> <property> <!-- 完全分布式集群名称 --> <name>dfs.nameservices</name> <value>mycluster</value> </property> <property> <!-- 集群中 NameNode 节点都有哪些 --> <name>dfs.ha.namenodes.mycluster</name> <value>cdh0,cdh1</value> </property> <property> <!-- cdh0 的 RPC 通信地址 --> <name>dfs.namenode.rpc-address.mycluster.cdh0</name> <value>cdh0:8020</value> </property> <property> <!-- cdh1 的 RPC 通信地址 --> <name>dfs.namenode.rpc-address.mycluster.cdh1</name> <value>cdh1:8020</value> </property> <property> <!-- cdh0 的 http 通信地址 --> <name>dfs.namenode.http-address.mycluster.cdh0</name> <value>cdh0:50070</value> </property> <property> <!-- cdh1 的 http 通信地址 --> <name>dfs.namenode.http-address.mycluster.cdh1</name> <value>cdh1:50070</value> </property> <property> <!-- 指定 NameNode 元数据在 JournalNode 上的存放位置 --> <name>dfs.namenode.shared.edits.dir</name> <value>qjournal://cdh0:8485;cdh1:8485;cdh2:8485/mycluster</value> </property> <property> <!-- 配置隔离机制，即同一时刻只能有一台服务器对外响应 --> <name>dfs.ha.fencing.methods</name> <value>sshfence</value> </property> <property> <!-- 使用隔离机制时需要 ssh 无秘钥登录，value是密钥所在的位置--> <name>dfs.ha.fencing.ssh.private-key-files</name> <value>/root/.ssh/id_rsa</value> </property> <property> <!-- 声明 journalnode 服务器存储目录--> <name>dfs.journalnode.edits.dir</name> <value>/opt/package/hadoop/jl</value> </property> <property> <!-- 关闭权限检查--> <name>dfs.permissions.enable</name> <value>false</value> </property> <property> <!-- 访问代理类：client，mycluster，active 配置失败自动切换实现方式--> <name>dfs.client.failover.proxy.provider.mycluster</name> <value>org.apache.hadoop.hdfs.server.namenode.ha.ConfiguredFailoverProxyProvider</value> </property> </configuration>

core-site.xml：

<configuration> <property> <name>fs.defaultFS</name> <value>hdfs://mycluster</value> </property> <property> <name>hadoop.tmp.dir</name> <value>/opt/package/hadoop/data/</value> </property> <property> <name>fs.trash.interval</name> <value>10080</value> </property> </configuration>

然后将这两个配置文件拷贝到其他两台机器上，使用scp命令

在三台机器上启动Journalnode
sbin/hadoop-daemon.sh start journalnode

在cdh0机器上进行格式化并启动Namenode和Datanode

bin/hdfs namenode -format sbin/start-dfs.sh

在cdh1上同步cdh0的元数据信息，并启动Namenode

bin/hdfs namenode -bootstrapStandby sbin/hadoop-daemon.sh start namenode

在cdh1上查看服务

bin/hdfs haadmin -getServiceState cdh1

这时候cdh0和cdh1上的Namenode都是处于standby状态，需要手动激活一个

bin/hdfs haadmin -transitionToActive cdh0

最后使用cdh0:50070和cdh1:50070都可以进入Namenode的WEBUI界面，cdh0是active状态，cdh1是standby状态