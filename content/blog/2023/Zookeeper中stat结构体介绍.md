---
title: "Zookeeper中stat结构体介绍"
date: 2018-08-23 21:54:53
draft: false
---
![](https://img-blog.csdn.net/20180823215228966?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

1)czxid- 引起这个znode创建的zxid，创建节点的事务的zxid

2)ctime - znode被创建的毫秒数(从1970年开始)

3)mzxid - znode最后更新的zxid

4)mtime - znode最后修改的毫秒数(从1970年开始)

5)pZxid-znode最后更新的子节点zxid

6)cversion - znode子节点变化号，znode子节点修改次数

7)dataversion - znode数据变化号

8)aclVersion - znode访问控制列表的变化号

9)ephemeralOwner- 如果是临时节点，这个是znode拥有者的session id。如果不是临时节点则是0

10)dataLength- znode的数据长度

11)numChildren - znode子节点数量