---
title: "sql语言语法"
date: 2017-09-17 16:31:10
draft: false
---
# 以下皆在命令行中执行(sql语言识别到;就会执行)

## 管理数据库:

1.查询所有数据库：show databases;

2.创建数据库：create database 数据库名;

3.删除数据库:drop database 数据库名;

4.指定默认字符集创建数据库:create database 数据库名 ->回车->default character set 字符集类型(如utf8);

5.查看数据库的默认字符集:show create database 数据库名;

6.修改数据库字符集类型:alter database 数据库名 default character set 字符集类型;

## 管理表:

1.创建表: create table 表名(字段); 字段格式: 字段名称 字段类型,字段名称 字段类型,.........

2.查看所有表: use 数据库名; show tables;
3.查看表的结构: desc 表名;

4.删除表: drop table 表名;
5.修改表: ①添加字段 alter table 表名 add 字段名称 字段类型;

②删除字段 alter table 表名 drop 字段名称;
③修改字符类型 alter table 表名 modify 字符名称 修改后的字符类型;

④修改字符名称 alter table 表名 change 旧字符名称 新字符名称 字符类型;
⑤修改表名 alter table 表名 rename 新的表名;

## 管理数据:

1.增加数据:①插入所有字段:insert into 表名 values(1,'张三','男', 20);

②插入部分数据:insert into 表名(id,name) values(1,' 张三');
2.修改数据:①修改所有数据(一般不用):update 表名 set name='李四'';

②带条件的修改:update 表名 set name='李四 'while id=1;//id为1的学生，修改其姓名为李四
③修改多个字段:update 表名 set name='李四 ',age=30,gender='女' while id=1;

3.删除数据:①删除所有数据(一般不用):delete from 表名; 另一种删除所有数据方式方式:truncate table 表名; (两种方法有区别)
②带条件的删除:delete from 表名 while id=2;

4.查询数据(重点):① 查询所有列： SELECT /* FROM student;
② 查询指定列： SELECT id,NAME,gender FROM student;

③ 查询时添加常量列：这里情况比较多，不进行一一说明

##