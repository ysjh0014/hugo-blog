---
title: "yarn对集群资源的管理和调度"
date: 2018-05-29 14:42:08
draft: false
---
/*资源调度和资源隔离是yarn作为一个资源管理系统，最重要和最基础的两个功能，资源调度是由ResourceManager完成的，资源隔离是由各个NodeManager实现的

/*ResourceManager将某个NodeManager上资源分配给任务(这就是所谓的"资源调度")后，NodeManager需按照要求为任务提供相应的资源，甚至保证这些资源应具有独占性，为任务运行提供基础的保证，这就是所谓的资源隔离

/*当谈到资源时，我们通常指内存，cpu和IO三种资源，Hadoop Yarn同时支持内存和cpu两种资源的调度

/*内存资源的多少会决定任务的生死，如果内存不够，任务可能会运行失败，相比之下，cpu资源则不同，它只会决定任务运行的快慢，不会对生死产生影响

/*yarn允许用户配置每个节点上可用的物理内存资源，yarn配置的只是自己可以使用的，主要配置参数如下:

<property>
<name>yarn.nodemanager.resource.memory-mb</name>
<value>4096</value>
</property>
<property>
<name>yarn.nodemanager.resource.cpu-vcores</name>
<value>4</value>

</property>

第一个是yarn可使用的物理内存总量，默认是8192，第二个是yarn可使用的虚拟cpu个数，这两个都要根据具体的设备来手动分配，其他的配置如每个任务可申请的最少物理内存是1024，最小的虚拟cpu个数是1，这些都保证默认值即可

yarn是非常强大的，是hadoop的操作系统，以yarn为核心的生态系统：

![](https://img-blog.csdn.net/20180529144048274)