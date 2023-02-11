---
title: "Zookeeper中写数据流程"
date: 2018-08-30 16:54:23
draft: false
---
1.Client向Zookeeper的Server1上写数据，发送一个写请求

2.如果Server1不是Leader，那么Server1会把接收到的请求进一步转发给Leader,这个Leader会把写请求广播给各个Leader,各个Server写成功后就会通知Leader

3.当Leader收到大多数Server数据写成功了，那么就说明数据写成功了，比如这里有三个节点，只有两个节点数据写成功了，就认为数据写成功了，写成功以后，Leader会告诉Server数据写成功了

4.Server1会通知Client数据写成功了，这时就认为整个写操作成功