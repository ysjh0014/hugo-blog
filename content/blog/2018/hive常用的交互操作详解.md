---
title: "hive常用的交互操作详解"
date: 2018-07-12 16:47:33
draft: false
---
bin/hive -help

![](https://img-blog.csdn.net/20180712160412513?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

1. bin/hive -e "要执行的sql语句";

这样可以不用进入交互式命令行去执行sql语句，直接就能显示出结果

2. bin/hive -f sql语句脚本目录

可以执行sql脚本文件

bin/hive -f sql语句脚本目录 > 执行结果要写到的目录

这样可以将执行结果写到指定的位置目录

3. bin/hive -i <filename>

与用户udf相互使用

4.在hive交互式命令行中对hdfs文件进行操作

dfs -ls /;

dfs -mkdir -p /usr/ysjh/mapreduce/;

![](https://img-blog.csdn.net/20180712162815352?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

5.在hive交互式命令行中对本地文件进行操作

!ls /opt

![](https://img-blog.csdn.net/20180712162904370?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

6.查看操作历史命令

cd root

cat .hivehistory