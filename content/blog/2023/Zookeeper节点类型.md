---
title: "Zookeeper节点类型"
date: 2018-08-21 16:17:05
draft: false
---
节点类型

1.Znode有两种类型

短暂(ephemeral): 客户端和服务器端断开连接后，创建的节点自己删除

长久(persistent): 客户端和服务器端断开连接后，创建的节点不删除

2.Znode有四种形式的目录节点(默认是persistent)

1).持久化目录节点（PERSISTENT）

客户端与zookeeper断开连接后，该节点依旧存在

2).持久化顺序编号目录节点（PERSISTENT_SEQUENTIAL）

客户端与zookeeper断开连接后，该节点依旧存在，只是Zookeeper给该节点名称进行顺序编号

3).临时目录节点（EPHEMERAL）

客户端与zookeeper断开连接后，该节点被删除

4).临时顺序编号目录节点（EPHEMERAL_SEQUENTIAL）

客户端与zookeeper断开连接后，该节点被删除，只是Zookeeper给该节点名称进行顺序编号

3.创建znode时设置顺序标识，znode名称后会附加一个值，顺序号是一个单调递增的计数器，由父节点维护

4.在分布式系统中，顺序号可以被用于为所有的事件进行全局排序，这样客户端可以通过顺序号推断事件的顺序