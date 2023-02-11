---
title: "Storm中的DRPC简单概述"
date: 2018-11-14 20:21:56
draft: false
---
前边我们基Hadoop实现了RPC，下面将一下Storm中的DRPC

DRPC：分布式RPC，Storm中的DRPC是使用Storm实时并行计算真正强大的函数，Storm拓扑作为输入接收函数参数流，并为每个函数调用发出结果的输出流

DRPC不是Storm的一个特征，因为它是Storm的streams spouts bolts和topologies表示的模式，DRPC本可以打包成Storm独立的库，但它与Storm捆绑在一起非常有用

分布式RPC由“DRPC服务器”协调，DRPC服务器协调接收RPC请求，将请求发送到Storm拓扑，从Storm拓扑接收结果，并将结果发送回等待的客户端，从客户端的角度来看，分布式RPC调用看起来就像常规的RPC调用

如下图所示：

![Tasks in a topology](http://storm.apache.org/releases/1.2.2/images/drpc-workflow.png)

客户端向DRPC服务器发送要执行的函数的名称以及该函数的参数，实现该功能的拓扑使用

DRPCSpout
从DRPC服务器接收功能调用流，每个函数调用都由DRPC服务器标记为唯一ID，然后拓扑计算结果，并在拓扑结束时调用一个bolt

ReturnResults
连接到DRPC服务器并为其提供函数调用id的结果，然后，DRPC服务器使用id来匹配客户端正在等待的结果，取消阻塞等待的客户端，并将结果发送给它