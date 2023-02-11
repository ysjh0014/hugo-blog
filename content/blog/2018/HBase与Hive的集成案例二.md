---
title: "HBase与Hive的集成案例二"
date: 2018-11-05 17:44:13
draft: false
---
**1.案例需求**

在 HBase 中已经存储了某一张表hbase_hive，然后在Hive中创建一个外部表来关联HBase中的hbase_hive这张表，使之可以借助 Hive来分析 HBase 这张表中的数据，案例二是紧接着案例一进行的，所以在做案例二之前应该先进行案例一

**2.在Hive中创建外部表并关联到HBase中的表**
CREATE EXTERNAL TABLE hbase_emp( empno int, ename string, job string, mgr int, hiredate string, sal double, comm double, deptno int) STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler' WITH SERDEPROPERTIES ("hbase.columns.mapping" = ":key,info:ename,info:job,info:mgr,info:hiredate,info:sal,info:comm,info:deptno") TBLPROPERTIES ("hbase.table.name" = "hbase_hbase");

**3.查看是否关联成功**

select /*from hbase_emp;

关联成功后就可以使用Hive来进行一些分析操作了