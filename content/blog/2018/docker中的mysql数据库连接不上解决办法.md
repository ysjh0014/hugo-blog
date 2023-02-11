---
title: "docker中的mysql数据库连接不上解决办法"
date: 2018-11-05 17:05:15
draft: false
---
**1.在docker内部连接不上mysql数据库**

即在本地模式下不能连接

这时候应该是docker容器重启过，mysql数据库没有启动的原因，可以使用
service mysql restart

来启动mysql数据库

**2.在宿主机上不能远程连接到docker容器中的mysql数据库**

这时候应该是mysql数据库经过重启之后，没有对root用户进行授权，所以不能远程连接

可以在启动mysql数据库之后进入到该数据库中，然后对root用户进行授权
mysql -uroot -proot GRANT ALL PRIVILEGES ON /*./* TO 'root'@'%' IDENTIFIED BY 'root' WITH GRANT OPTION;

最后在宿主机上使用

mysql -h+ip地址 -uroot -proot

来连接到docker中的mysql数据库