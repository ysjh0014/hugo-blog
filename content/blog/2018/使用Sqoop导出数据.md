---
title: "使用Sqoop导出数据"
date: 2018-09-13 16:41:35
draft: false
---
导出： 从大数据集群(Hive，Hdsf，Hbase)导出数据到RDBMS中

从Hive/Hdfs中导出数据到RDBMS(mysql)
bin/sqoop export \ --connect jdbc:mysql://172.17.0.6:3306/ys \ --username root \ --password root \ --table test \ --num-mappers 1 \ --export-dir /user/hive/warehouse/staff_hive \ --input-fields-terminated-by "\t"

注意： mysql 中如果表不存在,不会自动创建