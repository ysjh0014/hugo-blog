---
title: "Hive中DDL数据定义之数据库操作"
date: 2018-09-06 11:14:13
draft: false
---
**1.创建数据库**

create database 数据库名;

避免要创建的数据库已经存在错误,增加if not exists判断(标准写法)

create database if not exists 数据库名;

创建一个数据库，指定数据库在HDFS上存储的位置:

create database 数据库名 location '/你想要存放HDFS中的目录'; /*这里会自动生成该目录

**2.修改数据库**

用户可以使用alter database命令为某个数据库的dbproperties设置键-值对属性值，来描述这个数据库的属性信息，数据库的其他元数据信息都是不可更改的，包括数据库名和数据库所在的目录位置

alter database ys set dbproperties ('createtime'='20180906');

然后查看修改结果：

desc database extended ys;
![](https://img-blog.csdn.net/20180906110525742?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

**3.查询数据库**

1)

显示数据库

show databases;

过滤显示查询的数据库

show databases like 'ys/*';

2)

显示数据库信息

desc database 数据库名;

显示数据库详细信息

desc database extended 数据库名;

使用数据库

use 数据库名;

**4.删除数据库**

1)删除空数据库

drop database 空数据库名;

2)如果删除的数据库不存在，最好采用 if exists判断数据库是否存在

drop database if exists 数据库名;

3)如果数据库不为空，可以采用cascade命令，强制删除

drop database db_hive cascade;