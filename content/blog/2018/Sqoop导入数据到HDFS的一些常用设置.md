---
title: "Sqoop导入数据到HDFS的一些常用设置"
date: 2018-07-16 14:03:37
draft: false
---
/*只导入表中数据的某些列
bin/sqoop import \ --connect jdbc:mysql://192.168.83.112:3306/test \ --username root \ --password root \ --table student \ --target-dir /user/root/sqoop/import/student \ --num-mappers 1 \ --columns id,username

/*对要导入的mysql中的数据加上where条件

bin/sqoop import \ --connect jdbc:mysql://192.168.83.112:3306/test \ --username root \ --password root \ --table student \ --target-dir /user/root/sqoop/import/student \ --where "passwd='admin'" \ --num-mappers 1

/*使用query进行数据过滤

bin/sqoop import \ --connect jdbc:mysql://192.168.83.112:3306/test \ --username root \ --password root \ --query 'select id,passwd from student where $CONDITIONS' \ --target-dir /user/root/sqoop/import/student \ --num-mappers 1

/*文件的存储格式(parquetfile avrodatafile textfile)

默认的是textfile
bin/sqoop import \ --connect jdbc:mysql://192.168.83.112:3306/test \ --username root \ --password root \ --table student \ --target-dir /user/root/sqoop/import/student \ --where "passwd='admin'" \ --num-mappers 1 \ --as-parquetfile

/*压缩格式

bin/sqoop import \ --connect jdbc:mysql://192.168.83.112:3306/test \ --username root \ --password root \ --query 'select id,passwd from student where $CONDITIONS' \ --target-dir /user/root/sqoop/import/student \ --num-mappers 1