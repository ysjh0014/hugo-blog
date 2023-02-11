---
title: "Sqoop的安装及基本使用"
date: 2018-07-13 16:58:22
draft: false
---
Sqoop的安装:

Sqoop的安装非常简单，首先Sqoop的底层是mapreduce，所以必须依赖于hadoop

将Sqoop的压缩包上传解压后，然后修改配置文件即可

![](https://img-blog.csdn.net/20180713163436152?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

先将conf目录下的 sqoop-env-template.sh修改为 sqoop-env.sh，然后再如上图所示修改，

最后将mysql的驱动包拷贝到sqoop的lib目录下

Sqoop的基本使用:

sqoop的主要作用是将数据在传统的关系型数据库(RDBMS)与hadoop之间进行传递，即可以将传统的关系型数据库中的数据导入到hadoop的hdfs上，也可以将hdfs上的数据导入到传统的关系型数据库中
bin/sqoop help

![](https://img-blog.csdn.net/20180713165245718?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

上边就是sqoop的主要命令，例如version查看sqoop版本，export导出数据，import导入数据，list-databases查看关系型数据库里面的数据库

可以使用 bin/sqoop help+命令 的方式查看具体的命令使用方式

例如 list-databases

在sqoop目录下
bin/sqoop list-databases \ --connect jdbc:mysql://mysql数据库所在主机ip:3306 \ --username root \ --password root

![](https://img-blog.csdn.net/20180713165750990?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

可以查看到mysql的数据库情况