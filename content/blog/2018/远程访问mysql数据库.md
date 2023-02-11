---
title: "远程访问mysql数据库"
date: 2018-07-13 14:49:38
draft: false
---
mysql数据库默认是不能被远程访问的，这里以虚拟机中的mysql数据库为例

在虚拟机中的ubuntu系统中，使用 mysql -uroot -p 然后输入密码，就可以连接mysql数据库，但是在windows下使用 mysql -h+ip -uroot -p ，然后输入密码，提示不能连接

这里要做两个设置:

1.修改mysql的配置文件

cd /etc/mysql/mysql.conf.d

vi mysqld.cnf

将其中的 bind-address=127.0.0.1 修改为 bind-address=0.0.0.0 以允许任何ip来访问mysql数据库

然后重启mysql数据库

/etc/init.d/mysql restart

2.在数据库中新增权限用户

GRANT ALL PRIVILEGES ON /*./* TO 'root'@'%' IDENTIFIED BY 'root';

重新加载权限数据

flush privileges;

然后就可以远程访问该虚拟机上的mysql数据库了

例如在windows下使用 mysql -h+ip -uroot -p

输入密码就可以访问成功了