---
title: "Hive中DML数据操作"
date: 2018-09-07 21:33:55
draft: false
---
**1.数据导入**

1)向表中装载数据（load）

语法
load data [local] inpath '/opt/module/datas/student.txt' [overwrite] into table student [partition (partcol1=val1,…)];

load data:表示加载数据
local:表示从本地加载数据到hive表；否则从HDFS加载数据到hive表
inpath:表示加载数据的路径
into table:表示加载到哪张表
student:表示具体的表
overwrite:表示覆盖表中已有数据，否则表示追加
partition:表示上传到指定分区

2)通过查询语句向表中插入数据（Insert）

基本插入数据
insert into table student partition(month='201709') values('1004','wangwu');

基本模式插入（根据单张表查询结果）

insert overwrite table student partition(month='201708') select id, name from student where month='201709';

多插入模式（根据多张表查询结果）

from student insert overwrite table student partition(month='201707') select id, name where month='201709' insert overwrite table student partition(month='201706') select id, name where month='201709';

3)查询语句中创建表并加载数据（As Select）

create table if not exists student1 as select id,name from student;

4)创建表时通过Location指定加载数据路径

创建表，并指定在hdfs上的位置
create table if not exists student1( id int, name string ) row format delimited fields terminated by '\t' location '/user/hive/warehouse/student1';

上传数据到hdfs上

dfs -put /opt/module/datas/student.txt /user/hive/warehouse/student1;

5)import数据到指定Hive表中

先用export导出后，再将数据导入
import table student2 partition(month='201709') from '/user/hive/warehouse/export/student';

**2.数据导出**

1)insert导出

将查询的结果导出到本地
insert overwrite local directory '/opt/module/datas/export/student' select /* from student;

将查询的结果格式化导出到本地

insert overwrite local directory '/opt/module/datas/export/student1' row format delimited fields terminated by '\t' collection items terminated by '\n' select /* from student;

将查询的结果导出到HDFS上(没有local)

insert overwrite directory '/user/atguigu/hive/warehouse/student2' row format delimited fields terminated by '\t' collection items terminated by'\n' select /* from student;

2)Hadoop命令导出到本地

dfs -get /user/hive/warehouse/student/month=201709/000000_0 /opt/module/datas/export/student1.txt;

3)Hive Shell 命令导出

bin/hive -e 'select /* from default.student;' > /opt/module/datas/export/student1.txt;

4)export导出到HDFS上

export table default.student to '/user/hive/warehouse/export/student';

5)Sqoop导出

**3.清除表中数据（Truncate）**
truncate table student;

这里Truncate只能删除管理表，不能删除外部表中数据