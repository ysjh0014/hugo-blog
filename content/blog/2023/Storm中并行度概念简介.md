---
title: "Storm中并行度概念简介"
date: 2018-11-14 14:10:46
draft: false
---
![](https://img-blog.csdnimg.cn/20181114135520721.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==,size_16,color_FFFFFF,t_70)

当我们处理的数据量越来越大的时候，很少的节点处理起来就会变得困难，我们能想到的办法就是增加节点数，但是增加服务器节点有许多的限制，并不是首选方法，首选发放是增加Storm程序的并行度，进行代码的优化

而并行度是要配置多个地方的，比如Work Executor Task，这三个之间又是互相影响的

一个运行的Topology就是由集群中多台物理机上的多个worker进程组成

一个worker进程执行的是一个Topology的子集

一个worker进程会启动1到n个线程来执行一个Topology中的component(Spout或者Bolt)

executor是一个被worker进程启动的单独线程，每个executor只会运行一个topology的一个component

task是最终运行spout或者bolt的最小执行单元

默认的：

一个supervisor节点最多启动4个worker进程

每一个topology默认占用一个worker进程

每个worker进程会启动一个executor

每个executor启动一个task

Storm还可以更改正在运行的Topology的并行度，详细的官方文档有绍：

[http://storm.apache.org/releases/1.2.2/Understanding-the-parallelism-of-a-Storm-topology.html](http://storm.apache.org/releases/1.2.2/Understanding-the-parallelism-of-a-Storm-topology.html)