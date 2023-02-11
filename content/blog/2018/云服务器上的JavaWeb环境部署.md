---
title: "云服务器上的JavaWeb环境部署"
date: 2018-07-04 17:43:05
draft: false
---
之前有一篇博文是关于部署javaweb环境的，但是当时因为是初学，所以写的比较乱，并且很麻烦，今天从新总结一下

首先介绍下我的版本配置:

腾讯云 系统是centos

jdk版本是1.8.0_161

tomcat版本是8.0.52

1.解压

解压jdk和tomcat的压缩包，将他们放在同一个目录下

2.配置环境变量

配置全局环境变量

vi ~/.bashrc

在最后输入下面的内容

export JAVA_HOME=jdk所在目录
export PATH=${JAVA_HOME}/bin:$PATH

export CATALINA_HOME=tomcat所在目录

3.启动tomcat

进入到tomcat所在目录

启动tomcat: bin/startup.sh

关闭tomcat： bin/shutdown.sh

这时候在浏览器中输入服务器的公有ip+8080,出现tomcat的首页表示配置成功

浏览器默认是使用80端口的，所以要在tomcat的/conf/server.xm文件中修改8080端口为80端口

如果有域名，也是在/conf/server.xm文件中修改，将localhost全部修改为你的域名，就可以将公有ip替换成域名使用了