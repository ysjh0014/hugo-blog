---
title: "Hive项目实战四"
date: 2018-09-12 14:06:16
draft: false
---
### **最终业务实现**

**1.视频观看数 Top10**

使用order by做一个全局排序即可
select videoId,uploader,views from user_orc order by views desc limit 20;

**2. 视频类别热度 Top10**

需求分析： 统计出每个类别有多少个视频，然后显示出视频最多的前10个，我们需要使用group by对视频类别进行聚合，然后使用count()进行统计出每个类别视频个数，最后将视频个数进行排序输出前10个，因为一个视频可能对应多个类别，要想使用group by，需要先将类别进行列转行(展开)
select category_name as category, count(t.videoId) as hot from ( select videoId, category_name from user_orc lateral view explode(category) t_catetory as category_name) t group by t.category_name order by hot desc limit 10;

explode是将category列展开，例如category列如果是一个集合，集合中是key-value对，展开后就是key一行，value一行，如果表中有多个字段，就要加上lateral view，t_catetory是虚拟表的名，必须有

**3.视频观看数 Top20 所属类别包含这 Top20 视频的个数**

需求分析： 先找到观看数最高的 20 个视频所属条目的所有信息,降序排列，把这 20 条信息中的 category 分裂出来(列转行)，最后查询视频分类名称和该分类下有多少个 Top20 的视频
select category_name as category, count(t2.videoId) as hot_with_views from ( select videoId, category_name from ( select /* from user_orc order by views desc limit 20) t1 lateral view explode(category) t_catetory as category_name) t2 group by category_name order by hot_with_views desc;

**4.视频观看数 Top50 所关联视频的所属类别的热度排名**

需求分析： 查询出观看数最多的前 50 个视频的所有信息(包含了每个视频对应的关联视频),记为临时表 t1，将找到的 50 条视频信息的相关视频的id列转行,记为临时表 t2，将相关视频的 id 和user_orc 表进行 inner join 操作，按照视频类别进行分组,统计每组视频个数,然后排序
select category_name as category, count(t5.videoId) as hot from ( select videoId, category_name from ( select distinct(t2.videoId), t3.category from ( select explode(relatedId) as videoId from ( select /* from user_orc order by views desc limit 50) t1) t2 inner join user_orc t3 on t2.videoId = t3.videoId) t4 lateral view explode(category) t_catetory as category_name) t5 group by category_name order by hot desc;