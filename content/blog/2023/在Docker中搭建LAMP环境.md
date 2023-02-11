---
title: "在Docker中搭建LAMP环境"
date: 2018-10-20 11:02:35
draft: false
---
如果按照传统思想，搭建LAMP环境肯定是一个一个安装，但是在Docker中不一样，可以进行一键安装，不需要复杂的操作

1.下载LAMP镜像
docker pull tutum/lamp

2.生成容器

docker run --name=ys --hostname=ys -i -t -p 80 -p 3306 tutum/lamp /bin/bash

这时候就安装结束了，在ys这个容器中已经有apache php mysql这几个环境了，mysql使用mysql -uroot -p登录，密码是空