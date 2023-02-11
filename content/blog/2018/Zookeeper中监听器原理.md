---
title: "Zookeeper中监听器原理"
date: 2018-08-24 14:32:31
draft: false
---
![](https://img-blog.csdn.net/20180824083908836?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

1.监听原理

1)首先有一个main()线程

2)在main线程中创建Zookeeper客户端，会创建两个线程，connect负责网络连接通信，listener负责监听

3)通过connect线程将注册的监听事件发送给Zookeeper

4)在Zookeeper的注册监听器列表中将注册的监听事件添加到列表中

5)Zookeeper监听到有数据或路径变化，就会将这个消息发送给listener线程

6)listener线程内部调用了process（）方法

2.常见的监听

1)监听节点数据的变化：

get path [watch]

2)监听子节点增减的变化:

ls path [watch]