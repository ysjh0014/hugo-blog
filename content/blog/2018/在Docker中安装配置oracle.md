---
title: "在Docker中安装配置oracle"
date: 2018-09-03 20:55:46
draft: false
---
当然前提是已经安装好了Docker，如果没有安装，可以参考我之前的文章: [Docker的不同安装方式](https://blog.csdn.net/ys_230014/article/details/80630389)

1.将镜像pull到本地

docker pull wnameless

/oracle-xe-11g

2.创建docker容器，开放22端口和1521端口，分别对应ssh和oracle数据库的端口

docker run -d -p 22 -p 1521 wnameless/oracle-xe-11g

这时候已经可以连接oracle数据库了，如下图所示

![](https://img-blog.csdn.net/20180903205423348?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

主机名或ip地址是docker容器的ip,用户名是system,密码是oracle