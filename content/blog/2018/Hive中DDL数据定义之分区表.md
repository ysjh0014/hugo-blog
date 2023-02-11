---
title: "Hive中DDL数据定义之分区表"
date: 2018-09-07 18:05:21
draft: false
---
分区表实际上就是对应一个HDFS文件系统上的独立的文件夹，该文件夹下是该分区所有的数据文件。Hive中的分区就是分目录，把一个大的数据集根据业务需要分割成小的数据集。在查询时通过WHERE子句中的表达式选择查询所需要的指定的分区，这样的查询效率会提高很多

**1.单级分区表基本操作**

1)创建分区表语法
create table test( deptno int, dname string, loc string ) partitioned by (month string) row format delimited fields terminated by '\t';

2)加载数据到分区表中

load data local inpath '/opt/package/hive/txt/dept.txt' into table test partition(month='20180907'); load data local inpath '/opt/package/hive/txt/dept.txt' into table test partition(month='20180908'); load data local inpath '/opt/package/hive/txt/dept.txt' into table test partition(month='20180909');

3)查询分区表中数据

单分区查询
select /*from test where month='201709';

多分区联合查询

select /* from test where month='20180907' union all select /* from test where month='20180908' union all select /* from test where month='20180909';

注意:

Hive 1.2.0之前的版本仅支持union all，其中重复的行不会被删除 Hive 1.2.0和更高版本中，union的默认行为是从结果中删除重复的行

4)增加分区

增加单个分区
alter table test add partition(month='201809011')；

增加多个分区

alter table test add partition(month='201809011') partition(month='201809012') partition(month='201809013')；

5)删除分区

删除单个分区
alter table test drop partition(month='20180913');

删除多个分区

alter table test drop partition(month='20180911') partition(month='20180912') partition(month='20180913')；

6)查看分区表有多少分区

show partitions test;

7)查看分区表结构

desc formatted test;

除了能查看出是管理表之外，还能查看分区的信息

![](https://img-blog.csdn.net/20180907174637118?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

**2.多级分区表**

二级分区表

创建二级分区表
create table test( deptno int, dname string, loc string ) partitioned by (month string,day string) row format delimited fields terminated by '\t';

加载数据到二级分区表中

load data local inpath '/opt/package/hive/txt/dept.txt' into table test partition(month='201809',day='01');

查询二级分区表数据

select /*from test where month='201709' and day='01';