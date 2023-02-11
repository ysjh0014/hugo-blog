---
title: "Storm单机版部署及讲解"
date: 2018-11-08 21:17:37
draft: false
---
**1.部署前环境**

jdk7+

python2.6.6+

zookeeper(这里的单机版使用Storm自带的zookeeper)

**2.下载Storm压缩包，上传解压**

**3.修改conf目录下的storm-env.sh**
export JAVA_HOME=你自己的jdk所在位置的目录

**4.启动Storm**

1)在Storm目录下执行bin/storm就可以看到很多详细的命令

2)启动Storm自带的zookeeper
在前台启动
bin/storm dev-zookeeper

在后台启动 进入bin目录

nohup sh storm dev-zookeeper

3)启动主节点nimbus

nohup sh storm nimbus &

4)启动从节点supervisor

nohup sh storm supervisor &

5)启动UI界面 ip+8080

nohup sh storm ui &

6)启动日志查看服务logviewer

nohup sh storm logviewer &

**5.使用jps查看**

![](https://img-blog.csdnimg.cn/20181108211701641.png)

![](https://img-blog.csdnimg.cn/20181108211714986.png)