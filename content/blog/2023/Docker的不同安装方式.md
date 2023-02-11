---
title: "Docker的不同安装方式"
date: 2018-06-09 10:40:52
draft: false
---
Docker是一种虚拟化技术，而且是操作系统级别的虚拟化，所以学习它是非常必要的，下面介绍它的安装方式

1.在linux中安装Docker(以ubuntu为例)

Docker必须依赖linux内核，一般14.0以上的ubuntu系统就可以满足要求，在ubuntu下安装有两种方法

1).安装linux维护的版本，即使用apt-get install docker直接安装，这种方法安装的docker版本比较低

2).安装docker官方维护的版本，这种方法安装的是最新版的docker，这里也有两种方法进行安装的

**/*使用脚本安装**

********先附上脚本代码 docker.sh

1. /#!/bin/sh
1.
1. /#install curl
1. apt-get update
1. apt-get install -y curl
1.
1. /#install-docker
1. curl -fsSL get.docker.com -o get-docker.sh
1. sh get-docker.sh --mirror Aliyun
1.
1. /#install speeder
1. curl -sSL https://get.daocloud.io/daotools/set_mirror.sh | sh -s http://573b0ee5.m.daocloud.io
1. systemctl restart docker.service
1.
1. /#test docker
1. docker run hello-world

首先将文件放到一个目录下，如opt/docker中

然后进入该目录,cd /opt/docker

最后执行，./docker.sh

**/*详细安装**

首先安装docker

apt-get update

apt-get install curl

curl -fsSL get.docker.com -o get-docker.sh

sh get-docker.sh --mirror Aliyun

配置加速器

curl -sSL https://get.daocloud.io/daotools/set_mirror.sh | sh -s [http://573b0ee5.m.daocloud.io](http://573b0ee5.m.daocloud.io/)

systemctl restart docker.service

最后进行测试docker,输入以下命令:

docker --version

可以判断出docker是否安装成功以及docker的版本

### **2.在windows下安装**

在docker官网下载docker for windows,这里提供下载地址:

[https://store.docker.com/editions/community/docker-ce-desktop-windows?tab=description](https://store.docker.com/editions/community/docker-ce-desktop-windows?tab=description)

下载到本地之后双击运行就可以安装docker了，这里需要注意的是需要把360安全管家之类的先关闭，不然后安装不成功，另外安装docker后，就不能和虚拟机一起打开了，就是说在windows下虚拟机和docker只能运行其中一个