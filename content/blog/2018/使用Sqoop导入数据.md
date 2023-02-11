---
title: "使用Sqoop导入数据"
date: 2018-07-16 11:40:20
draft: false
---
**1.RDBMS到HDFS**

Sqoop把关系型数据库(这里以mysql为例)的数据导入到HDFS中，主要分为两步

/*得到元数据(mysql数据库中的数据)

/*提交map

![](https://img-blog.csdn.net/20180716100113666?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

这样流程就很清晰了，首先连接到mysql数据库得到表中的数据，然后进行map任务即可

1)先在mysql中创建一张表
create table ys.test(id int(4) primary key not null auto_increment, name varchar(255), sex varchar(255))；

2)全部导入

bin/sqoop import \ --connect jdbc:mysql://172.17.0.5:3306/ys \ --username root \ --password root \ --table test \ --target-dir /user/company \ --delete-target-dir \ --num-mappers 1 \ --fields-terminated-by "\t"

参数说明：

--target-dir ：指定导入数据到hdfs的位置

--delete-target-dir ： 如果导入数据到hdfs的位置目录已存在，则删除

--num-mappers ：指定mapper任务的个数，默认是有4个

--fields-terminated-by "\t" ： 指定导入数据到hdfs时每一列数据的分隔符，mysql数据导入到hdfs中默认的列分隔符是“，”，默认的行分隔符是"\n"

Sqoop导入数据到HDFS具体执行流程:

/*将命令自动生成相应的java类

/*将java代码编译成jar包

/*执行jar包

/*执行mapreduce(对import来说，只运行map任务)

3)查询导入
bin/sqoop import \ --connect jdbc:mysql://172.17.0.5:3306/ys \ --username root \ --password root \ --target-dir /user/test \ --delete-target-dir \ --num-mappers 1 \ --fields-terminated-by "\t" \ --query 'select name,sex from test where id <=1 and $CONDITIONS;'

注意： 局部导入数据使用的是--query参数，不能与--table参数一起使用，where语句后面必须跟$CONDITIONS，--query后边如果是双引号，$CONDITIONS必须加\，避免shell识别为自己的变量，如果是多个mapper任务的话，还需要加入--split-by参数来指定分割的字段名称，$CONDITIONS就是标记当前mapper从哪个数据段开始读

4)导入指定列
bin/sqoop import \ --connect jdbc:mysql://172.17.0.5:3306/ys \ --username root \ --password root \ --target-dir /user/test \ --delete-target-dir \ --num-mappers 1 \ --fields-terminated-by "\t" \ --columns id,sex \ --table test

5)使用Sqoop关键字筛选查询导入数据

bin/sqoop import \ --connect jdbc:mysql://172.17.0.5:3306/ys \ --username root \ --password root \ --target-dir /user/test \ --delete-target-dir \ --num-mappers 1 \ --fields-terminated-by "\t" \ --table test \ --where "id=1"

注意： 在 Sqoop 中可以使用 sqoop import -D property.name=property.value 这样的方式加入执行任务的参数,多个参数用空格隔开

**2.RDBMS到Hive**
bin/sqoop import \ --connect jdbc:mysql://172.17.0.6:3306/ys \ --username root \ --password root \ --table test \ --num-mappers 1 \ --hive-import \ --fields-terminated-by "\t" \ --hive-overwrite \ --hive-table test_hive

注意： 该过程分为两步,第一步将数据导入到 HDFS,第二步将导入到 HDFS 的数据迁移到 Hive 仓库
第一步默认的临时目录是/user/root/表名