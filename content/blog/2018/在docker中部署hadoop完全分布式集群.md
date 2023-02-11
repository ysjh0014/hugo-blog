---
title: "在docker中部署hadoop完全分布式集群"
date: 2018-06-15 18:53:57
draft: false
---
之前写过一篇关于如何部署hadoop完全分布式集群，但是是在虚拟机中部署的，开三台虚拟机，电脑内存就占用的特别多，毕竟虚拟机比较占内存，而docker是一种操作系统级别的轻量级虚拟化技术，一个docker容器占用少量的内存，下面就详解部署hadoop分布式的步骤(我的是在阿里云服务器上部署的，系统是ubuntu)

1.首先要在ubuntu中安装docker,可以参考之前的博文[Docker的不同安装方式](https://blog.csdn.net/ys_230014/article/details/80630389)，这里不再说明

2.首先做准备工作,创建一个容器将其配置好后作为基础镜像

docker run --name=hadoop -i -t ubuntu /bin/bash

将宿主机中的jdk和hadoop的压缩包传输到容器hadoop中(我这里的jdk是1.8的,hadoop是2.5.0的)

docker cp 宿主机中jdk压缩包的安装目录 hadoop:/opt/

设置环境变量

apt-get update

apt-get install vim

vi ~/.bashrc

在最后添加

export JAVA_HOME=/opt/jdk1.8.0_161

export PATH=${JAVA_HOME}/bin:$PATH

开启ssh服务

apt-get install openssh-server

vi /etc/ssh/sshd_config

在里面加入PermitRootLogin yes

保存并退出

开启ifconfig

apt-get install net-tools

然后按ctrl+p ctrl+q退出容器

最后将这个容器作为基础镜像提交到本地仓库

docker commit hadoop live

3.创建容器并在上面部署hadoop

集群的整体规划:

容器名 容器ip 开启的hadoop服务

hadoop1 172.17.0.3 namenode datanode nodemanager

hadoop2 172.17.0.4 datanode resourcemanager nodemanager

hadoop3 172.17.0.5 datanode nodemanager secondarynamenode historyserver

hadoop1

docker run --name=hadoop1 --hostname=hadoop1 -i -t -p 22 live /bin/bash

hadoop2

docker run --name=hadoop2 --hostname=hadoop2 -i -t -p 22 live /bin/bash

hadoop3

docker run --name=hadoop3 --hostname=hadoop3 -i -t -p 22 live /bin/bash

这里的容器ip要设置为静态的，即不变的

下载pipework:[点击下载](https://github.com/jpetazzo/pipework.git)

把下载的zip包传到宿主机上，解压，修改名字

unzip pipework-master.zip

mv pipework-master pipework

安装bridge-utils

apt-get install bridge-utils

给容器设置固定ip

pipework docker0 hadoop1 172.17.0.3/24

pipework docker0 hadoop2 172.17.0.4/24

pipework docker0 hadoop3 172.17.0.5/24

上边的ip地址要跟宿主机的docker0在同一个网段下,使用ifconfig查看docker0网段的网址

在hadoop1,hadoop2,hadoop3中添加hosts

vi /etc/hosts

172.17.0.3 hadoop1

172.17.0.4 hadoop2

172.17.0.5 hadoop3

在hadoop1中配置hadoop的配置文件

core-site.xml:

<property>
<name>fs.defaultFS</name>
<value>hdfs://172.17.0.3:8020</value>
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
<value>172.17.0.5:50090</value>

</property>

slave:

172.17.0.3

172.17.0.4

172.17.0.5

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
<value>172.17.0.4</value>
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

将hadoop-env.sh mapred-env.sh yarn-env.sh 这三个文件中的jdk的目录修改为/opt/jdk1.8.0_161

4.通过scp将hadoop传输给两外两个容器

配置ssh

在hadoop1中:

ssh-keygen -t rsa

ssh-copy-id hadoop1

ssh-copy-id hadoop2

ssh-copy-id hadoop3

重启ssh服务

/etc/init.d/ssh restart

在hadoop2中重复hadoop1中的工作

并在hadoop3中重启ssh服务

5.启动hadoop集群

进入hadoop1

docker attach hadoop1

cd /opt/hadoop-2.5.0

初始化hdfs

bin/hdfs namenode -format

启动各个服务

hadoop1:

sbin/start-dfs.sh

hadoop2:

sbin/start-yarn.sh

hadoop3:

sbin/mr-jobhistory-daemon.sh start historyserver

最后在hadoop1上进行wordcount测试,看map和reduce是否能正常运行