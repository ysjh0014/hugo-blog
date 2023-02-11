---
title: "远程连接redis数据库"
date: 2018-11-15 16:11:40
draft: false
---
我这里的连接环境是redis数据库安装在腾讯云服务器上，使用Redis Desktop Manager软件来连接

首先安装redis数据库就不在这里说明了，前面有详细的文章介绍

Redis Desktop Manager官网下载是要收费的，可以在我的百度网盘下载：

链接: [https://pan.baidu.com/s/19yl0O3Mizbp4v5eSGdDRXA](https://pan.baidu.com/s/19yl0O3Mizbp4v5eSGdDRXA)

提取码: 93na

软件下载打开后输入主机ip，端口号默认6379

这时候你会发现怎么也连接不上，报连接拒绝的错，这是因为你的redis没有进行远程连接设置

进入redis.conf

将bind 127.0.0.1改为bind 0.0.0.0

这要所有的机器就都能连接redis数据库了

图形化连接软件还是很好用的，可以进行增删改查一系列操作

![](https://img-blog.csdnimg.cn/20181115161318883.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==,size_16,color_FFFFFF,t_70)