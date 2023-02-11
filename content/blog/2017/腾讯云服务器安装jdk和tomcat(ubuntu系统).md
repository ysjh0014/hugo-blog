---
title: "腾讯云服务器安装jdk和tomcat(ubuntu系统)"
date: 2017-11-27 19:41:40
draft: false
---
腾讯云服务器安装javaweb环境:

这里以ubuntu系统为例

一.安装jdk

首先通过xshell连接到服务器

然后首先输入命令:sudo su来获取权限,因为ubuntu系统对权限的要求比较高,不能以root用户登录

1.默认jdk安装

sudo apt-get update
sudo apt-get install default-jre

2.Oracle JDK 安装（推荐）

设置 PPA
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update

JDK6 ：
sudo apt-get install oracle-java6-installer
JDK 7：
sudo apt-get install oracle-java7-installer
JDK 8
sudo apt-get install oracle-java8-installer

安装完毕输入java-version,出现了jdk版本说明已经安装好了

输入：java -verbose可以查看java安装路径

3.设置jdk环境变量

使用vi打开配置文件：sudo vi ~/.bashrc 然后按i开始添加

在末尾添加：

export JAVA_HOME=/usr/lib/jvm/java-8-oracle
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
export PATH=${JAVA_HOME}/bin:$PATH

再按Esc退出,然后输入:wq进行保存，最后使用source ~/.bashrc使刚刚的配置生效

这里注意前后不要有空格,还有如果jdk安装路径不一样,可以用java -verbose查看路径后自己修改

二.安装tomcat

这里采用上传压缩包的方式

首先在 官网下载linux版的tomcat

然后使用工具xftp上传到到目录:home/ubuntu下,

解压:sudo tar -zxvf /*/*/*/*.tar.gz

1.配置tomcat:

复制解压后的文件到 /opt 目录
sudo cp -r apache-tomcat-8.0.12 /opt
进入 /opt/apache-tomcat-8.0.12 目录
cd /opt/apache-tomcat-8.0.12

打开启动的脚本文件 : sudo vi ./bin/startup.sh

按照配置jdk时的方法在文本末尾加入:

JAVA_HOME=/usr/lib/jvm/java-8-oracle
JRE_HOME=${JAVA_HOME}/jre
PATH=$JAVA_HOME/bin:$PATH
CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
TOMCAT_HOME=/opt/tomcat8

然后在tomcat/bin目录下启动tomcat: ./startup.sh

这时候在浏览器输入ip:8080应该会出现tomcat的猫

如果需要更改端口号为80端口，可以在tomcat/conf/server.xml文件中修改端口号为80

如果有域名的话需要在tomcat/conf/server.xml文件中把localhost全部修改为你的域名

最后如果要上传项目到tomcat下,需要把tomcat的root权限修改，不然通过xftp上传不了,输入一下命令:

把Tomcat包含子目录，属主和组全部设置ubuntu
/# chown ubuntu -R /usr/local/tomcat
/# chgrp ubuntu -R /usr/local/tomcat