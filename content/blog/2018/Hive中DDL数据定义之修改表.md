---
title: "Hive中DDL数据定义之修改表"
date: 2018-09-07 18:45:28
draft: false
---
**1.重命名表**
alter table 原表名 rename to 新表名;

**2.增 删分区表**

见 [Hive中DDL数据定义之分区表](https://blog.csdn.net/ys_230014/article/details/82466189)

**3.修改表中列信息**

查询表结构
desc 表名；

添加列

alter table 表名 add columns(deptdesc string);

更新列

alter table 表名 change column deptdesc desc int;

替换列

alter table 表名 replace columns(deptno string, dname string, loc string);