---
title: "Hive项目实战三"
date: 2018-09-12 12:50:22
draft: false
---
### **创建表**

这里总共需要创建4张表，明明只有两个数据文件，为什么要创建4张表呢？因为这里创建的表要使用orc的压缩方式，而不使用默认的textfile的方式，orc的压缩方式要想向表中导入数据需要使用子查询的方式导入，即把从另一张表中查询到的数据插入orc压缩格式的表汇中，所以这里需要四张表，两张textfile类型的表user和video，两张orc类型的表user_orc和video_orc

**1.先创建textfile类型的表**
create table user( videoId string, uploader string, age int, category array<string>, length int, views int, rate float, ratings int, comments int, relatedId array<string>) row format delimited fields terminated by "\t" collection items terminated by "&" stored as textfile;
 
create table video( uploader string, videos int, friends int) row format delimited fields terminated by "\t" stored as textfile;

向两张表中导入数据，从hdfs中导入

load data inpath '数据文件在hdfs中的位置' into table user;

**2.创建两张orc类型的表**

create table user_orc( videoId string, uploader string, age int, category array<string>, length int, views int, rate float, ratings int, comments int, relatedId array<string>) clustered by (uploader) into 8 buckets row format delimited fields terminated by "\t" collection items terminated by "&" stored as orc;
 
create table video_orc( uploader string, videos int, friends int) clustered by (uploader) into 24 buckets row format delimited fields terminated by "\t" stored as orc;

向两张表中导入数据

insert into table user_orc select /*from user; insert into table video_orc select /*from video;

这时候数据就加载到两张表中了，可以进行简单的查看

select /*from user_orc limit 10; select /*from video_orc limit 10;