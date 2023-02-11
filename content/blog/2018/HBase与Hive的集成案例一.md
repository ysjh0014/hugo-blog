---
title: "HBase与Hive的集成案例一"
date: 2018-11-05 17:36:33
draft: false
---
**1.Hive与HBase的对比**

**Hive**

1)数据仓库
Hive 的本质其实就相当于将 HDFS 中已经存储的文件在 Mysql 中做了一个双射关系，以方便使用 HQL 去管理查询

2)用于数据分析、清洗
Hive 适用于离线的数据分析和清洗，延迟较高

3)基于 HDFS、MapReduce
Hive 存储的数据依旧在 DataNode 上，编写的 HQL 语句终将是转换为 MapReduce 代码执行

**HBase**

1)数据库
是一种面向列存储的非关系型数据库

2)用于存储结构化和非结构话的数据
适用于单表非关系型数据的存储，不适合做关联查询，类似 JOIN 等操作

3)基于 HDFS
数据持久化存储的体现形式是 Hfile，存放于 DataNode 中，被 ResionServer 以 region 的形式进行管理

4)延迟较低，接入在线业务使用
面对大量的企业数据，HBase 可以直线单表大量数据的存储，同时提供了高效的数据访问速度

**2.案例一**

**1)案例需求**

建立一张Hive 表，关联到HBase表，插入数据到 Hive 表的同时能够同步数据到HBase表

**2)创建Hive表的同时关联到HBase**
CREATE TABLE hive_hbase( empno int, ename string, job string, mgr int, hiredate string, sal double, comm double, deptno int) stored by 'org.apache.hadoop.hive.hbase.HBaseStorageHandler' with serdeproperties ("hbase.columns.mapping" = ":key,info:ename,info:job,info:mgr,info:hiredate,info:sal,info:comm,info:deptno") tblproperties ("hbase.table.name" = "hbase_hive");

上边步骤之后进入Hive和HBase中查看表hive_hbase和hbase_hive是否创建成功

**3)在Hive中创建临时表，用于load数据到表中**

不能直接load数据到关联了HBase的那张表中，因为load是不执行Mapreduce的，所以必须通过一张临时表来实现
CREATE TABLE emp( empno int, ename string, job string, mgr int, hiredate string, sal double, comm double, deptno int) row format delimited fields terminated by '\t';

**4)向Hive的临时表中load数据**

load data local inpath '/opt/package/hive/txt/emp.txt' into table emp;

emp.txt文件中的数据：

7369 SMITH CLERK 7902 1980-12-17 800.00 20 7499 ALLEN SALESMAN 7698 1981-2-20 1600.00 300.00 30 7521 WARD SALESMAN 7698 1981-2-22 1250.00 500.00 30 7566 JONES MANAGER 7839 1981-4-2 2975.00 20 7654 MARTIN SALESMAN 7698 1981-9-28 1250.00 1400.00 30 7698 BLAKE MANAGER 7839 1981-5-1 2850.00 30 7782 CLARK MANAGER 7839 1981-6-9 2450.00 10 7788 SCOTT ANALYST 7566 1987-4-19 3000.00 20 7839 KING PRESIDENT 1981-11-17 5000.00 10 7844 TURNER SALESMAN 7698 1981-9-8 1500.00 0.00 30 7876 ADAMS CLERK 7788 1987-5-23 1100.00 20 7900 JAMES CLERK 7698 1981-12-3 950.00 30 7902 FORD ANALYST 7566 1981-12-3 3000.00 20 7934 MILLER CLERK 7782 1982-1-23 1300.00 10

**5)将临时表中的数据通过insert命令导入到与HBase关联的那张表中去(hive_hbase表)**

insert into table hive_hbase select /* from emp;

这时候你会发现会执行Mapreduce

**6)查看Hive和HBase中的表是否已经同步了数据**
select /*from hive_hbase; scan 'hbase_hive'