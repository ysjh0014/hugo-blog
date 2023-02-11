---
title: "Hive项目实现五"
date: 2018-09-12 20:32:16
draft: false
---
**5.每个类别中的视频热度 Top10，以Music为例**

需求分析： 先将user_orc表中的category(视频类别) 展开，可以创建一张表用于存放视频类别，然后向表中插入数据，最后统计对应类别(Music)中的视频热度

创建表
create table test( videoId string, uploader string, age int, categoryId string, length int, views int, rate float, ratings int, comments int, relatedId array<string>) row format delimited fields terminated by "\t" collection items terminated by "&" stored as orc;

插入数据

insert into table test select videoId, uploader, age, categoryId, length, views, rate, ratings, comments, relatedId from user_orc lateral view explode(category) catetory as categoryId;

统计Music类别中的视频热度Top10

select videoId, views from test where categoryId = "Music" order by views desc limit 10;

**6. 每个类别中视频流量 Top10，以Music为例**

需求分析： 直接在5中创建的表中按照ratings(流量)排序
select videoId, views, ratings from test where categoryId = "Music" order by ratings desc limit 10;

**7.上传视频最多的用户 Top10 以及他们上传的视频**

需求分析： 先找到上传视频最多的 10 个用户的用户信息，通过 uploader 字段与 youtube_orc 表进行 join,得到的信息按照 views 观看次数进行排序即可
select t2.videoId, t2.views, t2.ratings, t1.videos, t1.friends from ( select /* from video_orc order by videos desc limit 10) t1 join user_orc t2 on t1.uploader = t2.uploader order by views desc limit 20;

**8.每个类别视频观看数 Top10**

需求分析： 先得到 categoryId 展开的表数据，子查询按照 categoryId 进行分区,然后分区内排序,并生成递增数字,该递增数字这一列起名为 rank 列，通过子查询产生的临时表,查询 rank 值小于等于 10 的数据行即可
select t1./* from ( select videoId, categoryId, views, row_number() over(partition by categoryId order by views desc) rank from test) t1 where rank <= 10;

9.可能出现的问题

JVM堆内存溢出

解决办法： 在 yarn-site.xml 中加入如下代码
<property> <name>yarn.scheduler.maximum-allocation-mb</name> <value>2048</value> </property> <property> <name>yarn.scheduler.minimum-allocation-mb</name> <value>2048</value> </property> <property> <name>yarn.nodemanager.vmem-pmem-ratio</name> <value>2.1</value> </property> <property> <name>mapred.child.java.opts</name> <value>-Xmx1024m</value> </property>