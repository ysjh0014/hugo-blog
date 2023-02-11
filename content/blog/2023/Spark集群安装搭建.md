---
title: "Spark集群安装搭建"
date: 2018-10-08 20:44:16
draft: false
---
**1.下载Spark**

Spark是一个独立的内存计算框架，如果不考虑存储的话，可以完全独立运行，因此这里就只安装Spark集群

Spark下载地址： [http://spark.apache.org/downloads.html](http://spark.apache.org/downloads.html)

![](https://img-blog.csdn.net/20181008203401965?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

选择好Spark和Hadoop的版本之后就可以下载了，从2.0版本开始，Spark默认使用Scala2.11

**2.上传解压**

将Spark的压缩包上传到集群的某一台机器上，然后解压缩

**3.进行Spark的配置文件的配置**

进入到Spark的目录下
cd conf mv spark-env.sh.template spark-env.sh vi spark-env.sh

在该配置文件中添加如下配置

export JAVA_HOME=你的jdk所在目录

配置slaves文件

mv slaves.template slaves vi slaves

在slaves中添加你的Spark集群子节点机器的主机名或者ip

**4.将配置好的Spark传输到集群的其他机器上**

使用scp命令，如果集群机器特别多的话，可以使用shell编程来循环自动传输，这里不在详细说明

**5.启动Spark**

进入到Spark的主目录下
sbin/start-all.sh

使用jps命令可以看出，该Spark集群有一个Master，三个Work

Spark集群的WEBUI界面： Master所在的主机ip：8080