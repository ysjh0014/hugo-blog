---
title: "HBase与Sqoop的集成"
date: 2018-11-08 15:52:56
draft: false
---
之前学习Sqoop的时候都是Hadoop，Hive和RDBMS之间进行数据的导入与导出，并没有与HBase集成，下面就来讲解HBase与Sqoop的集成

需求：

利用 Sqoop 在 HBase 和 RDBMS 中进行数据的转储，将 RDBMS(Mysql) 中的数据抽取到 HBase 中

1.在Sqoop中配置sqoop-env.sh，添加下边的内容
export HBASE_HOME=你的HBase目录

2.在Mysql中创建一个test数据库，一张表book

CREATE DATABASE test; CREATE TABLE test.book( id int(4) PRIMARY KEY NOT NULL AUTO_INCREMENT, name VARCHAR(255) NOT NULL, price VARCHAR(255) NOT NULL);

向表中插入数据

INSERT INTO test.book (name, price) VALUES('Lie Sporting', '30'); INSERT INTO test.book (name, price) VALUES('Pride & Prejudice', '70'); INSERT INTO test.book (name, price) VALUES('Fall of Giants', '50');

3.执行Sqoop操作导入数据

bin/sqoop import \ --connect jdbc:mysql://cdh0:3306/test \ --username root \ --password root \ --table book \ --columns "id,name,price" \ --column-family "info" \ --hbase-create-table \ --hbase-row-key "id" \ --hbase-table "hbase_book" \ --num-mappers 1 \ --split-by id

注意：

sqoop1.4.6 只支持 HBase1.0.1 之前的版本的自动创建 HBase 表的功能，如果不能自动创建表，还需要手动在HBase中创建
hbase> create 'hbase_book','info'