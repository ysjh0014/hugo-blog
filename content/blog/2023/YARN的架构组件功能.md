---
title: "YARN的架构组件功能"
date: 2018-05-29 14:07:51
draft: false
---
hadoop1.0和hadoop2.0的最大区别就在于hadoop2.0多出了一个yarn,hadoop1.0中Mapreduce即承担集群资源的管理和调 度，又承担数据的处理，而hadoop2.0中将这两个任务分离开，yarn来对集群的资源进行管理和调度，Mapreduce来进行数据的处理，并且Mapreduce是运行在yarn上边的，yarn上不仅能运行Mapreduce这种并行计算框架，还能运行其他的，比如流式计算框架storm,内存计算框架spark,所以yarn被称为hadoop的操作系统

![](https://img-blog.csdn.net/20180528203901970)

yarn服务组件:

/* yarn总体上仍然是Master/slave(主从)结构，在整个资源管理框架中，Re'sourceManager为Master,NodeManager为 Slave

/*ResourceManager负责对各个NodeManager上的资源进行统一管理和调度

/*当用户提交一个应用程序时，需要提供一个用以跟踪和管理这个程序的

/*ApplicationMaster，它负责向ResourceManager申请资源，并要求NodeManager启动可以占用一定资源的任务

/*由于不同的ApplicationMaster被分布到不同的节点上，因此他们之间不会相互影响

ResourceManager:

/*全局的资源管理器，整个集群只有一个，负责集群资源的统一管理和调度分配

/*功能:

处理客户端请求

启动/监控Application Master

监控NodeManager

资源分配与调度

NodeManager:

/*整个集群有多个，负责单节点资源管理和使用

/*功能:

单个节点上的资源管理和任务管理

处理来自ResourceManager的命令

处理来自Application Master的命令

/*NodeManager管理抽象容器，这些容器代表着可供一个特定应用程序使用的针对每个节点的资源

/*定时的向RM汇报本节点上的资源使用情况和各个Container的运行状态

Application Master:

/*管理一个在yarn内运行的应用程序的每个实例

/*功能:

为应用程序申请资源，并进一步分配给内部任务

任务监控与容错

/*负责协调来自ResourceManager的资源，并通过NodeManager监视容器的执行和资源使用(cpu 内存等的资源分配)

Container:

/*yarn中的资源抽象，封装某个节点上的多维度资源，如内存,cpu,磁盘,网络等，当AM向RM申请资源时，RM向AM返回的 资源便是用Container表示的

/*yarn会为每个任务分配一个Container,且该任务只能使用该Container中描述的资源

/*功能:

对任务运行环境的抽象

描述一系列信息

任务运行资源(节点，内存，cpu)

任务启动命令

任务运行环境