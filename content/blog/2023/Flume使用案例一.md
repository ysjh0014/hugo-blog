---
title: "Flume使用案例一"
date: 2018-09-14 21:13:45
draft: false
---
**1.安装部署Flume**

我这里Flume的版本是CDH5.3.6的，下载地址 [http://archive.cloudera.com/cdh5/cdh/5/flume-ng-1.5.0-cdh5.3.6.tar.gz](http://archive.cloudera.com/cdh5/cdh/5/flume-ng-1.5.0-cdh5.3.6.tar.gz)

上传解压之后修改配置文件
mv flume-env.sh.template flume-env.sh vi flume-env.sh

将export JAVA_HOME修改为自己的目录

**2.案例一： 监控端口数据**

Flume 监控一端 Console,另一端 Console 发送消息,使被监控端实时显示

1)安装telnet工具
apt-get install telnet

2)创建Flume Agent配置文件job_flume.conf（这里的job_flume.conf是随便起的文件名，但后缀必须是.conf）

/# Name the components on this agent a1.sources = r1 a1.sinks = k1 a1.channels = c1 /# Describe/configure the source a1.sources.r1.type = netcat a1.sources.r1.bind = localhost a1.sources.r1.port = 44444 /# Describe the sink a1.sinks.k1.type = logger /# Use a channel which buffers events in memory a1.channels.c1.type = memory a1.channels.c1.capacity = 1000 a1.channels.c1.transactionCapacity = 100 /# Bind the source and sink to the channel a1.sources.r1.channels = c1 a1.sinks.k1.channel = c1

这个配置文件的内容在官方文档中有详细介绍，还是要多看官方文档

3)开启Flume监听端口
bin/flume-ng agent \ --conf conf/ \ --name a1 \ --conf-file conf/job_flume.conf \ -Dflume.root.logger==INFO,console

参数介绍： --conf： Flume的配置文件目录

--name： 这次Job的Agent名称

--conf-file： 这次Job的配置文件目录

-Dflume.root.logger： 日志的输出级别和输出位置

4)使用telnet工具向44444端口发送内容（退出telnet是使用Ctrl+]）

![](https://img-blog.csdn.net/20180914211236394?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

这时候flume就会实时监控到端口接收到的内容，如下图所示：

![](https://img-blog.csdn.net/20180914211218262?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)