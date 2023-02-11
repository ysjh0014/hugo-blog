---
title: "HDFS启动时的safemode模式"
date: 2018-05-28 15:50:16
draft: false
---
安全模式safemode整个过程是从启动datanode到启动完毕

safemode的作用：

/*等待datanode向namenode发送块的报告

/*namenode会将块的个数和fsimage和edits中的作比较，当达到99.999%的阈值时安全模式safemode会自行在30秒后 退出，这30秒的缓冲时间是为了让hdfs文件系统有一个稳定时间再去使用和操作

安全模式下不能进行改变文件系统的命名空间，即元数据

在namenode的web界面(ip+50070)中可以详细的看到安全模式的整个过程

如何手动进入安全模式safemode：

在hdfs下有一个命令:

bin/hdfs dfsadmin

![](https://img-blog.csdn.net/20180528154505287)

启动:

bin/hdfs dfsadmin -safemode enter

查看safemode状态:

bin/hdfs dfsadmin -safemode get

离开:

bin/hdfs dfsadmin -safemode leave