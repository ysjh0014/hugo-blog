---
title: "分布式协作服务框架Zookeeper功能简介"
date: 2018-06-28 20:19:16
draft: false
---
Zookeeper是什么:

/*一个开源的分布式的，为分布式应用提供协调服务的Apache项目

/*提供一个简单的原语集合，以便于分布式应用可以在它之上构建更高层次的同步服务

/*设计非常易于编程，它使用的是类似于文件系统那样的树形数据结构

/*目的就是将分布式服务不再需要由于协作冲突而另外实现协作服务

简单来说就是为分布式服务提供协作服务的，Zookeeper的服务器节点一般是奇数个，下图是Zookeeper的服务架构图:

![](https://img-blog.csdn.net/20180628172359473?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

Zookeeper从设计模式角度来看，是一个基于观察者模式设计的分布式服务管理框架，它负责存储和管理大家都关心的数据，然后接收观察者的注册，一旦这些数据的状态发生变化，Zookeeper就将负责通知已经在Zookeeper上注册的那些观察者做出相应的反应，从而实现集群中类似Master/Slave 管理模式

应用场景:

/*统一命名服务

/*配置管理

/*集群管理

/*共享锁/同步锁

Zookeeper的安装可以看我之前的博文[Zookeeper的单机模式安装](https://blog.csdn.net/ys_230014/article/details/80098070)，里面有很详细的讲解