---
title: "Zookeeper的分布式安装部署"
date: 2018-07-16 17:35:01
draft: false
---
Zookeeper对集群时间同步要求非常严格，所以配置分布式zookeeper集群时，必须要求集群时间同步

分布式安装部署的步骤:

1.修改zookeeper目录下的conf目录中的zoo_sample.cfg

mv zoo_sample.cfg zoo.cfg

vi zoo.cfg

![](https://img-blog.csdn.net/20180716172417287?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

如图所示

在zookeeper目录下创建一个/data/zkData文件夹，然后将dataDir修改为你的目录

在zoo.cfg里面添加:

server.1=192.168.83.110:2888:3888
server.2=192.168.83.111:2888:3888
server.3=192.168.83.112:2888:3888

这里的ip换成你自己的ip地址或者主机名

2.在zookeeper目录下的/data/zkData/目录下创建一个myid，里面写1

2.使用scp将zookeeper同步到其他节点机器上

3.修改其他两个节点机器上的zookeeper目录下的/data/zkData/myid/分别为2和3

4.用 bin/zkServer.sh 启动zookeeper，这里zookeeper没有同时启动的脚本，只能一个一个机器的启动，除非自己写脚本

![](https://img-blog.csdn.net/20180716173257407?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

![](https://img-blog.csdn.net/20180716173336329?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

![](https://img-blog.csdn.net/20180716173359958?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

这里可以看出zookeeper集群的状态，一个leader,两个follower