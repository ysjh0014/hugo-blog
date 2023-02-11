---
title: "windows下Scala开发环境的搭建"
date: 2018-09-29 13:18:41
draft: false
---
1.Java JDK的下载

因为Scala语言是运行下JVM上的，所以Java JDK必须有，并且是1.8及其以上版本

2.Scala SDK的下载

Scala SDK下载地址： [https://www.scala-lang.org/download/all.html](https://www.scala-lang.org/download/all.html)

这里在windows下就下载.msi版本的，可以按照提示安装即可，不用手动设置环境变量

如果下载windows下的压缩包版本的，则需要自己手动配置环境变量

3.使用cmd命令行查看Scala是否安装成功

![](https://img-blog.csdn.net/20180929131822872?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

3.使用IDEA开发Scala

IDEA下载的时候就自带Scala插件，如果没有则需要自己安装，可以在线安装也可以离线安装

然后可以创建Scala项目了

![](https://img-blog.csdn.net/20180929131627381?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

这里选择IDEA即可，sbt是一个创建项目的模本，不用管