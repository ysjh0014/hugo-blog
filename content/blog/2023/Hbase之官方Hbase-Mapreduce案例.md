---
title: "Hbase之官方Hbase-Mapreduce案例"
date: 2018-11-03 10:00:41
draft: false
---
Hbase是一个非关系型的数据库，可以分布式部署，擅长存储数据，但是不能分析数据，所以通过 HBase 的相关 JavaAPI，我们可以实现伴随HBase 操作的 MapReduce 过程，比如使用MapReduce 将数据从本地文件系统导入到 HBase 的表中，比如我们从 HBase 中读取一些原始数据后使用 MapReduce 做数据分析

**Hbase-Mapreduce官方案例(统计有多少行数据)**

**1.导入环境变量**

因为要引用HBase的API，所以要在Hadoop中引入HBase的jar包
export HBASE_HOME=/opt/package/hadoop export HBASE_HOME=/opt/package/hbase export HADOOP_CLASSPATH=`${HBASE_HOME}/bin/hbase mapredcp`

将其中HADOOP_HOME和HBASE_HOME的目录换成你自己的

**2.运行官方的Hbase-Mapreduce案例**
bin/yarn jar /opt/package/hbase/lib/hbase-server-0.98.6-cdh5.3.6.jar rowcounter HBase中的表名

运行结果：

![](https://img-blog.csdnimg.cn/20181103093217385.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==,size_16,color_FFFFFF,t_70)

**使用Mapreduce将本地数据导入到HBase**

**1.导入环境变量**

如果你刚导入过上边案例中的环境变量，并且当时的窗口没有关闭的话，这里就不用再从新导入环境变量了

**2.在本地创建一个tsv格式的文件：test.tsv**
1001 zs blue 1002 ls yello 1003 ww red

中间使用tab键隔开

**3.在HBase中创建表**
create 'ys','info'

**4.在HDFS中创建文件夹并上传test.tsv文件**

bin/hdfs dfs -mkdir /usr/ys/test bin/hdfs dfs -put test.tsv /usr/ys/test

**5.执行Mapreduce将数据导入到HBase的表中**

bin/yarn jar /opt/package/lib/hbase-server-0.98.6-cdh5.3.6.jar importtsv \ -Dimporttsv.columns=HBASE_ROW_KEY,info:name,info:color ys \ hdfs://cdh0:8020/usr/ys/test

运行结果：

![](https://img-blog.csdnimg.cn/20181103095856907.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==,size_16,color_FFFFFF,t_70)

在HBase的shell中查看ys表中是否有数据

![](https://img-blog.csdnimg.cn/20181103100005965.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==,size_16,color_FFFFFF,t_70)