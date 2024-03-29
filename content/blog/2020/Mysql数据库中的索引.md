---
title: "Mysql数据库中的索引"
date: 2020-03-24 15:34:24
draft: false
featured_image: "https://hugo-ys.oss-cn-hangzhou.aliyuncs.com/static/img/java.png"
tags:
- Java
categories: 
- Java
---
**1.什么是索引**

索引是对数据库表中的一列或者多列的值进行排序的一种数据结构，如果把数据库中的表比作一本书，索引就是这本书的目录，通过目录可以快速查找到书中指定内容的位置

索引也是一张表，该表中存储着索引的值和这个值的数据所在行的物理地址，使用索引后可以不用扫描全表来定位某行的数据，而是通过索引表来找到该行数据对应的物理地址

**2.索引的优缺点**

优点：

建立索引的列可以保证行的唯一性，生成唯一的rowId

索引可以有效缩短数据的检索时间，减少I/O次数

索引可以加快表与表之间的连接

为用来排序和分组的字段建立索引可以加快分组和排序

缺点：

创建索引和维护索引需要时间成本，这个成本随着数据量的增大而加大

创建索引和维护索引需要空间成本，每一条索引都需要占据数据库的物理存储空间，数据量越大，占用空间也越大

会降低表的增删改的效率，因为每次增删改，索引需要进行动态维护

**3.Mysql的索引有哪些**

**从逻辑角度**

1).普通索引：普通索引是最基本的索引，它没有任何限制，允许在定义索引的列中插入重复值和空值

直接创建：
create index index_name on table_name (column(length));

修改表结构添加索引：

alter table table_name add index index_name on(column(length));

创建表的时候创建索引：

create table table_name( ..... .... .... index index_name(column(length)) )

删除索引：

drop index index_name on table;

2).唯一索引：索引列的值必须唯一，允许有空值，如果是组合索引，列值的组合必须唯一

直接创建：
create unique index index_name on table_name (column(length));

修改表结构添加索引：

alter table table_name add unique index index_name on(column(length));

创建表的时候创建索引：

create table table_name( ..... .... .... unique index index_name(column(length)) )

3).主键索引：主键索引是一种特殊的唯一索引，一个表只能有一个主键，不允许有空值，一般是在创建表的时候指定主键，主键默认就是主键索引

create table table_name( ........ ...... ..... primary key(column) )

4).组合索引：多个字段上创建的索引，只有在查询条件中使用了创建索引时的第一个字段，索引才会被使用

alter table table_name add index index_name(column,column,column..);

5).全文索引：索引类型为FULLTEXT，允许有重复值和空值，可以在char、varchar、text类型的列上创建，Mysql中MyISAM和InnoDB存储引擎都支持。主要用来查找文本中的关键字，而不是直接与索引中的值比较，它更像是一个搜索引擎，全文索引需要配合match against操作使用，而不是一般的where语句加like

6).空间索引：空间索引是对空间数据类型的字段建立的索引，Mysql中的空间索引类型有4种，GEOMETRY、POINT、LINESTRING、POLYGON，创建空间索引的列，必须将其声明为not null，Mysql中只有MyISAM存储引擎支持创建空间索引

**从物理存储角度**

聚集索引和非聚集索引

聚集索引：聚集索引确定表中数据的物理顺序，一个表中只能包含一个聚集索引，但该索引可以包含多个列，聚集索引对于经常要搜索范围值的列特别有效，使用聚集索引找到包含第一个值的行后，便可以确保包含后续索引值的行在物理相邻

聚集索引中索引的叶节点就是数据节点，而非聚集索引的叶节点仍然是索引节点，只不过有一个指针指向相应的数据块

**从数据结构角度**

1).hash索引：hash索引基于hash表实现，只有查询条件精确匹配hash索引中的所有列才会用到hash索引，存储引擎会为hash索引中的每一列都计算hash码，hash索引中存储的就是hash码，所以每次读取都会进行两次查询

仅仅能满足”=“，”<=>“查询，不能使用范围查询

hash索引无法用于数据的排序操作

hash索引不能利用部分索引键查询

hash索引在任何时候都不能避免表扫描

hash索引遇到大量hash值相等的情况后性能并不一定比BTree索引高

2).B+Tree索引

B+Tree索引在MyISAM和InnoDB存储引擎中的实现不同

InnoDB和MyISAM存储引擎默认使用的是B+Tree索引，Mermory默认使用的是hash索引

MyISAM中data域保存数据记录的地址，MyISAM中索引检索的算法为首先按照B+Tree搜索算法搜索索引，如果指定的key存在，则取出其data域的值，然后以data域的值为地址，读取相应数据记录，MyISAM的索引方式也叫做”非聚集的“。MyISAM索引文件和数据是分离的，索引文件仅仅保存数据记录的地址。在InnoDB中，表数据文件本身就是按B+Tree组织的一个索引结构，这棵树的叶节点data域保存了完整的数据记录，这个索引的key是数据表的关键，这种索引也叫做聚集索引

**4.索引的设计原则**

索引并非越多越好

避免对经常更新的表创建过多的索引，索引中的列要尽可能的小；经常要查询的字段应该创建索引，但是要避免添加不必要的字段

数据量小的表最好不要使用索引

在条件表达式中经常用到的不同值较多的列上建立索引，在不同值少的列上不要建立索引

当唯一性是某种数据本身的特性时，建立唯一索引

在频繁进行排序或者分组的列上建立索引