---
title: "Storm的容错性(可靠性)"
date: 2018-11-21 19:35:40
draft: false
---
**1.Worker挂掉**

当worker挂掉，supervisor将会重新启动它，如果supervisor启动连续失败并且无法对Nimbus进行心跳，Nimbus将会在其他机器上重新安排worker

**2.节点挂掉**

如果节点机器挂掉，分配给该机器的任务将超时，Nimbus会将这些任务重新分配给其他机器

3.Nimbus或者Supervisor守护进程挂掉

Nimbus和Supervisor守护进程设计为快速失败(遇到任何意外情况时进程自毁)和无状态(所有状态都保存在Zookeeper或磁盘上)，因此，如果Nimbus或Supervisor守护进程死亡，它们会重新启动，就像没有发生任何事情一样

Nimbus或者Supervisor守护进程的死亡不会影响worker

**4.Nimbus是单点故障吗？**

Nimbus挂掉，worker将仍然会运行，worker挂掉，Nimbus会重新启动worker，是可以做HA高可用的

Nimbus是有备用的，一个处于活跃状态，一个处于等待状态