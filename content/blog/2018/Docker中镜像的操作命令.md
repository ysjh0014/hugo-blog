---
title: "Docker中镜像的操作命令"
date: 2018-06-09 19:11:34
draft: false
---
**1.docker的镜像存储位置**

目录:/var/lib/docker

docker info:查看docker的镜像存储位置

**2.列出镜像**

docker images:查看容器中的所有镜像,这个命令查看的IMAGE ID是被截断的

docker images --no-trunc:查看IMAGE ID没有被截断的镜像

docker images -a:查看所有的镜像，包括中间层镜像

docker images+仓库名:查看该仓库名的镜像

docker images -q:只返回镜像的IMAGE ID

**3.镜像标签和仓库**

**![](https://img-blog.csdn.net/20180609164243160)**

REPOSITORY:仓库

TAG:标签,在后创建容器的时候可以指定，如果不指定则默认的是latest

REPOSITORY+TAG构成镜像的名字,例如ubuntu:14.04

同一个TAG标签可以对应相同的IMAGE ID，并且这里的IMAGE ID是被截断的

**4.查看镜像的具体信息**

docker inspect+仓库名:标签名

**5.删除镜像**

docker rmi 仓库名:标签名/截断的或者完全的IMAGE ID

如果想要删除多个镜像，可以在docker rmi 后面添加多个仓库名:标签名/截断的或者完全的IMAGE ID

如果要删除所有的镜像，则使用docker rmi $(docker images -q)命令

首删除容器中的镜像前必须要停止容器的运行，然后才能删除容器中的镜像

**6.查找镜像**

第一种是在Docker Hub官方网站上查找:[https://registry.hub.docker.com](https://registry.hub.docker.com),因为要注册账户，这里就不多介绍

第二种是使用docker命令来查找,此方式查找命令行中最多显示25个

docker search ubuntu:返回的结果中会包含评分，星级等信息

docker search -s 4 ubuntu:只返回星级在4以上的

**7.拉取镜像**

docker pull 仓库名:标签名

这里如果没有配置加速器的话下载速度可能会很慢，配置加速器的方法在我之前的[Docker不同的安装方式](https://blog.csdn.net/ys_230014/article/details/80630389)文章中有