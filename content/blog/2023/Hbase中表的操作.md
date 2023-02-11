---
title: "Hbase中表的操作"
date: 2018-09-23 10:21:06
draft: false
---
官方文档： [http://hbase.apache.org/book.html/#quickstart](http://hbase.apache.org/book.html#quickstart)

**1.创建表**

必须指定表名和ColumnFamily（列族名）名称，例如下面的'cf'就是列族名
hbase(main)> create 'test','cf'

**2.查看表**

hbase（main）>list 'test'

**3.查看表的详细信息**

hbase（main）>decribe 'test'

**4.插入数据到表**

hbase(main) > put 'test','1','cf:name','Thomas' hbase(main) > put 'test','1','cf:sex','male' hbase(main) > put 'test','2','cf:age','18' hbase(main) > put 'test','2','cf:name','Janna' hbase(main) > put 'test','3','cf:sex','female' hbase(main) > put 'test','3','cf:age','20'

这里的数字1，2，3表示行名，每一行都有一个自己的名称，后边的数据必须要加上列族名

**5.扫描表中的所有数据**
hbase(main) >scan 'test' hbase(main) >scan 'test',{STARTROW=>'2'} hbase(main) >scan 'test',{STARTROW=>'1',STOPROW=>'2'}

注意： 这里的STARTROW和STOPROW必须大写

**6.获取单行数据**
hbase(main) >get 'test','1','cf:name'

**7.禁用表格**

如果要删除表或更改其设置，以及在某些其他情况下，您需要先使用该

disable
命令禁用该表。您可以使用该

enable
命令重新启用它
hbase(main) >disable 'test'

**8.更新指定字段的数据**

hbase(main) >put 'test','1','cf:name','ys'

**9.删除数据**

hbase(main) >delete 'test','1',''cf:name'

**10.清空表数据**

先禁用表，再清空表数据
hbase(main) >truncate 'test'

**11.删除表**

先禁用表，再删除表
hbase(main) >drop 'test'

**12.统计表数据行数**

hbase(main) >count 'test'