---
title: "Docker中的push和pull操作"
date: 2018-09-27 18:40:39
draft: false
---
Docker可以像GitHub一样进行Push和Pull操作并且十分简单

1.在Docker Hub上注册一个账号，然后创建一个远程仓库

Docker Hub地址： [https://hub.docker.com/](https://hub.docker.com/)

2.首先将本地容器打包成本地镜像
docker commit 容器名 镜像仓库:镜像标签

然后只要使用docker images可以查看到你打包到本地镜像就可以了

3.将本地容器打包到远程仓库
docker tag 本地镜像仓库：本地镜像标签 远程仓库名：远程镜像标签

4.push到远程仓库

docker login
 
docker push 远程仓库名：远程镜像标签

注意 ： 这里的远程镜像标签是自己定义的名称，即在Docker Hub上看到的标签名

push之前要先登录

5.从远程pull到本地
docker login
 
docker pull 远程仓库名：远程镜像标签

最后等着下载就好了