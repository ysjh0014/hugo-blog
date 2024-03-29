---
title: "Mysql数据库中的存储引擎"
date: 2020-03-25 15:02:41
draft: false
featured_image: "https://hugo-ys.oss-cn-hangzhou.aliyuncs.com/static/img/java.png"
tags:
- Java
categories: 
- Java
---
**1.什么是存储引擎**

Mysql中的数据用各种不同的技术存储在文件或者内存中，每一种技术都使用不同的存储机制、索引技巧、锁定水平，并且最终提供广泛的不同的功能和能力，这些不同的技术以及配套的相关功能在Mysql中被称为存储引擎，我们可以根据对数据处理的需求，选择不同的存储引擎

**2.常用的存储引擎**

**MyISAM存储引擎：**

MyISAM存储引擎基于ISAM存储引擎，并对其进行了扩展，它是在web、数据仓储和其它应用环境下最常使用的存储引擎之一，MyISAM拥有较高的插入、查询速度，但是不支持事务

MyISAM在磁盘上存储成3个文件，文件名就是表名，但是扩展名不同

frm(存储表定义)

MYD(MYData，存储数据)

MYI(MYIndex，存储索引)

MyISAM主要特性：

被大文件的文件系统和操作系统支持

当把删除和更新以及插入操作混合使用的时候，动态尺寸的行产生更少的碎片。这要通过合并相邻被删除的块，若下一个块被删除，就扩展到下一块自动完成

每个MyISAM表最大索引数是64，这可以通过重新编译来改变，每个索引最大的列数是16

NULL被允许在索引的列中，这个值占每个键的0~1个字节

可以把数据文件和索引文件放在不同的目录(InnoDB是放在一个目录里面的)

MyISAM存储引擎的索引结构为B+Tree，MyISAM中data域保存数据记录的地址，MyISAM中索引检索的算法为首先按照B+Tree搜索算法搜索索引，如果指定的key存在，则取出其data域的值，然后以data域的值为地址，读取相应数据记录，MyISAM的索引方式也叫做”非聚集的“。MyISAM索引文件和数据是分离的，索引文件仅仅保存数据记录的地址
**InnoDB存储引擎：**

InnoDB存储引擎是事务型数据库的首选引擎，支持事务安全表(ACID)，支持行锁定和外键，InnoDB是Mysql默认的存储引擎

使用InnoDB存储引擎Mysql将在数据目录下创建一个名为ibdata的10M大小的自动扩展数据文件，以及两个名为ib_logfile0和ib_logfile1的5M大小的日志文件

InnoDB主要特性：

1).InnoDB给Mysql提供了具有提交、回滚、崩溃恢复能力的事务安全(ACID兼容)的存储引擎，InnoDB锁定在行级并且也在select语句中提供一个类似Oracle的非锁定读，这些功能增加了多用户部署和性能。在SQL查询中，可以自由的将InnoDB类型的表和其它Mysql的表类型混合起来，甚至在同一个查询中也可以混合

2).InnoDB存储引擎为在主内存中缓存数据和索引而维持它自己的缓冲池，InnoDB将它的表和索引在一个逻辑表空间中，表空间可以包含数个文件(或原始磁盘文件)，这与MyISAM表不同，比如在MyISAM表中每个表被存放在分离的文件中，InnoDB表可以是任何尺寸，即使在文件尺寸被限制为2GB的操作系统上

3).InnoDB支持外键完整性约束，存储表中的数据时，每张表的存储都按照主键顺序存放，如果没有显示在表定义时指定主键，InnoDB会为每一行生成一个6字节的ROWID，并以此作为主键

InnoDB存储引擎也使用B+Tree作为索引结构，但是实现方式和MyISAM截然相反

第一个区别就是InnoDB的数据文件本身就是索引文件，MyISAM索引文件和数据文件是分离的，索引文件仅保存数据记录的地址。在InnoDB中，表数据文件本身就是按照B+Tree组织的一个索引结构，这棵树的叶节点data域保存了完整的数据记录

第二个不同是InnoDB的辅助索引data域存储相应记录主键的值而不是地址，也就是说，InnoDB的所有辅助索引都引用主键作为data域

**Memory存储引擎：**

Memory存储引擎将表的数据存储到内存中，未查询和引用其它表数据提供快速访问

Memory主要特性：

Memory表的每个表可以有多大32个索引，每个索引16列，以及500字节的最大键长度

Memory存储引擎执行hash和btree索引

可以在一个Memory表中有非唯一键值

Memory使用一个固定的记录长度格式

Memory不支持blog和text列

当不再需要Memory表的内容时，要释放被Memory表使用的内存，应该执行delete from或者truncate table，或者删除整个表(使用drop table)

**MyISAM和InnoDB存储引擎的区别：**

InnoDB支持事务，MyISAM不支持

InnoDB支持行级锁，MyISAM支持表级锁

InnoDB支持MVCC，MyISAM不支持

InnoDB支持外键，MyISAM不支持

InnoDB不支持全文索引，MyISAM支持

**使用场景：**

InnoDB：如果要提供提交、回滚、崩溃恢复能力的事务安全能力，并要求实现并发控制，InnoDB是一个很好的选择

MyISAM：如果数据表主要用来插入和查询操作，MyISAM引擎能提供较高效率

Memory：如果只是临时存放数据，数据量不大，并且不需要较高的数据安全性，可以选择将数据保存在内存中的Memory引擎，Mysql中使用该引擎作为临时表，存放查询结果的中间结果，数据的处理速度很快但是安全性不高