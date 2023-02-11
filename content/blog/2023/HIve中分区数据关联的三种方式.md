---
title: "HIve中分区数据关联的三种方式"
date: 2018-09-07 18:29:18
draft: false
---
之前的分区表都是先创建表然后加载数据到分区表中，然后就会在HDFS自动创建相关的目录存储数据，但是这里反过来做，先在HDFS中创建相应的目录，然后把数据直接上传到这个目录下，具体如下所示

先在HDFS中创建存放数据的目录
dfs -mkdir -p /user/hive/warehouse/ys.db/test/month=201809/day=02;

然后直接上传数据到该目录下

dfs -put /opt/package/hive/txt/dept.txt /user/hive/warehouse/ys.db/test/month=201809/day=02;

这时候查询数据会发现没有数据，只有相应的字段

![](https://img-blog.csdn.net/20180907182457591?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

这是因为分区表与数据还没有关联起来，下面有3中方法进行关联

1.执行修复命令
msck repair table test;

2.执行添加分区

alter table test add partition(month='201809', day='02');

3.load数据到分区

load data local inpath '/opt/package/hive/txt/dept.txt' into table test partition(month='201809',day='02');

这时候再查询数据就会查到了