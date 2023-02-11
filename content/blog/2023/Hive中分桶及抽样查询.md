---
title: "Hive中分桶及抽样查询"
date: 2018-09-08 19:22:15
draft: false
---
**1.分桶**

分桶表数据存储

分区针对的是数据的存储路径;分桶针对的是数据文件
分区提供一个隔离数据和优化查询的便利方式。不过,并非所有的数据集都可形成合理的分区,特别是之前所提到过的要确定合适的划分大小这个疑虑。
分桶是将数据集分解成更容易管理的若干部分的另一个技术

数据准备

student.txt
1001 ss1 1002 ss2 1003 ss3 1004 ss4 1005 ss5 1006 ss6 1007 ss7 1008 ss8 1009 ss9 1010 ss10 1011 ss11 1012 ss12 1013 ss13 1014 ss14 1015 ss15 1016 ss16

1)先创建分桶表,通过直接导入数据文件的方式

create table stu_buck(id int, name string) clustered by(id) into 4 buckets row format delimited fields terminated by '\t';

2)查看表结构

hive (default)> desc formatted stu_buck;

Num Buckets: 4

3)导入数据到分桶表中
hive (default)> load data local inpath '/opt/module/datas/student.txt' into table stu_buck;

这时候在HDFS中发现并没有发成四个桶

4)创建分桶表时,数据通过子查询的方式导入

1)先建一个普通的 stu 表
create table stu(id int, name string) row format delimited fields terminated by '\t';

2)向普通的 stu 表中导入数据

load data local inpath '/opt/module/datas/student.txt' into table stu;

3)清空 stu_buck 表中数据

truncate table stu_buck; select /* from stu_buck;

4)导入数据到分桶表,通过子查询的方式

insert into table stu_buck select id, name from stu cluster by(id);

发现还是只有一个分桶

5)需要设置一个属性
hive (default)>set hive.enforce.bucketing=true; hive (default)> set mapreduce.job.reduces=-1; hive (default)>insert into table stu_buck select id, name from stu cluster by(id);

这时候就是分成四个桶了

6)查询分桶的数据
hive (default)> select /* from stu_buck; OK stu_buck.id stu_buck.name 1001 ss1 1005 ss5 1009 ss9 1012 ss12 1016 ss16 1002 ss2 1006 ss6 1013 ss13 1003 ss3 1007 ss7 1010 ss10 1014 ss14 1004 ss4 1008 ss8 1011 ss11 1015 ss15

**2.分桶抽样查询**

对于非常大的数据集,有时用户需要使用的是一个具有代表性的查询结果而不是全部结

果。Hive 可以通过对表进行抽样来满足这个需求。
查询表 stu_buck 中的数据
hive (default)> select /* from stu_buck tablesample(bucket 1 out of 4 on id);

注:tablesample 是抽样语句,语法:TABLESAMPLE(BUCKET x OUT OF y)
y 必须是 table 总 bucket 数的倍数或者因子。hive 根据 y 的大小,决定抽样的比例。例如, table 总共分了 4 份,当 y=2 时,抽(4/2=)2个 bucket 的数据,当 y=8 时,抽取(4/8=)1/2个 bucket 的数据。x 表示从哪个 bucket 开始抽取。例如, table 总 bucket 数为 4,tablesample(bucket 4 out of4),表示总共抽取(4/4=)1 个 bucket 的数据,抽取第 4 个 bucket 的数据。
注意:x 的值必须小于等于 y 的值,否则
FAILED: SemanticException [Error 10061]: Numerator should not be bigger than
denominator in sample clause for table stu_buck

**3.数据块抽样**

Hive 提供了另外一种按照百分比进行抽样的方式,这种是基于行数的,按照输入路径下的数据块百分比进行的抽样
hive (default)> select /* from stu tablesample(0.1 percent) ;

提示:这种抽样方式不一定适用于所有的文件格式。另外,这种抽样的最小抽样单元是一个 HDFS 数据块。因此,如果表的数据大小小于普通的块大小 128M 的话,那么将会返回所有行