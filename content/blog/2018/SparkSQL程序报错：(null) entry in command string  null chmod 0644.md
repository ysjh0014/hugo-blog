---
title: "SparkSQL程序报错：(null) entry in command string:null chmod 0644"
date: 2018-10-24 21:17:02
draft: false
---
之前在写将mysql数据库中的数据取出然后以Json格式写入文件中的时候报 (null) entry in command string: null chmod 0644 错误，找了好久终于在网上找到解决办法

我的程序是在windows下边跑的，没有在集群上

解决方法：

在 https://github.com/SweetInk/hadoop-common-2.7.1-bin 中下载hadoop.dll，并拷贝到c:\windows\system32目录中

然后重新运行代码程序即可

注意：

程序中的路径都要写windows文件系统中的路径了，不再是hdfs中的路径