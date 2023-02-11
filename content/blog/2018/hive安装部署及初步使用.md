---
title: "hive安装部署及初步使用"
date: 2018-06-20 11:07:40
draft: false
---
hive是将mapreduce这个过程进行了封装，现在只需要写HSQL语句就可以实现mapreduce，所以hive的运行环境就是hadoop

下面的hive安装部署是在hadoop集群已经安装好并且能够运行的前提下，如果没有可以参考我之前的博文，hadoop的伪分布式和分布式都有详细的步骤介绍

hive安装部署:

我这里的hive是1.2.1版本的，2.0以上的版本和以下的版本底层不一样，2.0.以下的是对mapreduce进行封装，2.0以上的版本是对spark进行的封装，所以2.0以上的版本会更快，用户对于不同的版本不会感到有什么不同，但是这里还没有学到spark，所以我就安装2.0一下的版本了

1.向宿柱机上传hive的压缩包，解压

2.进入到hive目录下的conf目录

vi hive-env.sh

![](https://img-blog.csdn.net/2018062010572973?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

将上面图片中的两个修改为你自己的安装目录

3.在hdfs中创建tmp和/user/hive/warehouse(如果有就不用创建了),并赋予他们权限

进入到hadoop目录下

bin/hdfs dfs -mkdir -p /user/hive/warehouse

bin/hdfs dfs -chmod g+w /tmp

bin/hdfs dfs -chmod g+w /user/hive/warehouse

4.启动hive

进入到hive安装目录下

bin/hive

这里第一次启动会有点慢

启动时可能会报jline这个jar包冲突，这是因为hadoop中也有这个jar包，但是版本太老，解决办法就是进入到hadoop-2.5.0/share/hadoop/yarn/lib下，删除两个jline的jar包，然后重启hive就不会报错了

![](https://img-blog.csdn.net/20180620110648197?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

这时候就可以使用HSQL语句了，非常简单

hive初步使用:

1.首先要创建一张表

create table student(id int,name string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',';

上边的语句是创建表并对数据进行格式化和指明字段的分隔符

2.加载数据到这张表里面

在/opt/hive-1.2.2/下创建data目录用来存放数据，在里面创建一个student.txt,并录入几条数据

load data local inpath '/opt/hive-1.2.2/data/student.txt' into table student;

3.测试

select /*from student;

![](https://img-blog.csdn.net/20180624201018931?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

select id from student;

![](https://img-blog.csdn.net/20180624201029985?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

这里可以看出执行语句的时候并没有走mapreduce程序，而是直接就输出结果了，说明hive不是完全走mapreduce的，这样可以提高执行效率，毕竟mapreduce需要执行的时间比较久