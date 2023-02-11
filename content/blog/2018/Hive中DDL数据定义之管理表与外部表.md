---
title: "Hive中DDL数据定义之管理表与外部表"
date: 2018-09-06 18:40:53
draft: false
---
**管理表**

1.理论

默认创建的表都是所谓的管理表，有时也被称为内部表。因为这种表，Hive会（或多或少地）控制着数据的生命周期。Hive默认情况下会将这些表的数据存储在由配置项hive.metastore.warehouse.dir(例如，/user/hive/warehouse)所定义的目录的子目录下。 当我们删除一个管理表时，Hive也会删除这个表中数据。管理表不适合和其他工具共享数据。

2.案例

1)普通创建表
create table if not exists student( id int, name string ) row format delimited fields terminated by '\t' stored as textfile location '/user/hive/warehouse/student';

2)根据查询结果创建表（查询的结果会添加到新创建的表中）

create table if not exists student1 as select id, name from student;

3)根据已经存在的表结构创建表

create table if not exists student2 like student;

4)查询表的类型

desc formatted student;

![](https://img-blog.csdn.net/20180906153555120?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

**外部表**

1.理论

因为表是外部表，所有Hive并非认为其完全拥有这份数据。删除该表并不会删除掉这份数据(在HDFS中仍然存在)，不过描述表的元数据信息会被删除掉(在mysql中的元数据)

2.管理表和外部表的使用场景

每天将收集到的网站日志定期流入HDFS文本文件。在外部表（原始日志表）的基础上做大量的统计分析，用到的中间表、结果表使用内部表存储，数据通过SELECT+INSERT进入内部表。

3.案例

分别创建部门和员工外部表，并向表中导入数据

1)原始数据

dept.txt
10 ACCOUNTING 1700 20 RESEARCH 1800 30 SALES 1900 40 OPERATIONS 1700

emp.txt

7369 SMITH CLERK 7902 1980-12-17 800.00 20 7499 ALLEN SALESMAN 7698 1981-2-20 1600.00 300.00 30 7521 WARD SALESMAN 7698 1981-2-22 1250.00 500.00 30 7566 JONES MANAGER 7839 1981-4-2 2975.00 20 7654 MARTIN SALESMAN 7698 1981-9-28 1250.00 1400.00 30 7698 BLAKE MANAGER 7839 1981-5-1 2850.00 30 7782 CLARK MANAGER 7839 1981-6-9 2450.00 10 7788 SCOTT ANALYST 7566 1987-4-19 3000.00 20 7839 KING PRESIDENT 1981-11-17 5000.00 10 7844 TURNER SALESMAN 7698 1981-9-8 1500.00 0.00 30 7876 ADAMS CLERK 7788 1987-5-23 1100.00 20 7900 JAMES CLERK 7698 1981-12-3 950.00 30 7902 FORD ANALYST 7566 1981-12-3 3000.00 20 7934 MILLER CLERK 7782 1982-1-23 1300.00 10

2)建表语句

创建部门表
create external table if not exists default.dept( deptno int, dname string, loc int ) row format delimited fields terminated by '\t';

创建员工表

create external table if not exists default.emp( empno int, ename string, job string, mgr int, hiredate string, sal double, comm double, deptno int) row format delimited fields terminated by '\t';

3)导入数据

load data local inpath '/opt/package/hive/txt/dept.txt' into table dept; load data local inpath '/opt/package/hive/txt/emp.txt' into table emp;

4)查看表格式化数据

![](https://img-blog.csdn.net/20180906184029354?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)