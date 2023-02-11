---
title: "Hive中DDL数据定义之建表语法"
date: 2018-09-06 11:36:14
draft: false
---
**1.创建表**

建表语法
create [external] table [if not exists] table_name [(col_name data_type [comment col_comment], ...)] [comment table_comment] [partitioned by (col_name data_type [comment col_comment], ...)] [clustered by (col_name, col_name, ...) [sorted by (col_name [asc|desc], ...)] into num_buckets buckets] [row format row_format] [stored AS file_format] [location hdfs_path]

字段解释说明

1)create table 创建一个指定名字的表，如果相同名字的表已经存在，则抛出异常；用户可以用 IF NOT EXISTS 选项来忽略这个异常

2)external关键字可以让用户创建一个外部表，在建表的同时指定一个指向实际数据的路径(location)，Hive创建内部表时，会将数据移动到数据仓库指向的路径；若创建外部表，仅记录数据所在的路径，不对数据的位置做任何改变。在删除表的时候，内部表的元数据和数据会被一起删除，而外部表只删除元数据，不删除数据

3)comment：为表和列添加注释

4)partitioned by：创建分区表

5)clustered by：创建分桶表

6)sorted by：不常用

7)row format

delimited [fields terminated by char] [COLLECTION ITEMS TERMINATED BY char]

[MAP KEYS TERMINATED BY char] [LINES TERMINATED BY char]

| SERDE serde_name [WITH SERDEPROPERTIES (property_name=property_value, property_name=property_value, ...)]

用户在建表的时候可以自定义SerDe或者使用自带的SerDe，如果没有指定ROW FORMAT 或者ROW FORMAT DELIMITED，将会使用自带的SerDe，在建表的时候，用户还需要为表指定列，用户在指定表的列的同时也会指定自定义的SerDe，Hive通过SerDe确定表的具体的列的数据

8)stored as： 指定存储文件类型

常用的存储文件类型：SEQUENCEFILE（二进制序列文件）、TEXTFILE（文本）、RCFILE（列式存储格式文件）

如果文件数据是纯文本，可以使用STORED AS TEXTFILE。如果数据需要压缩，使用 STORED AS SEQUENCEFILE

9)location：指定表在HDFS上的存储位置

10)like：允许用户复制现有的表结构，但是不复制数据