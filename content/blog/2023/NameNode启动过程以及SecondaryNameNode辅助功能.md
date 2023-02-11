---
title: "NameNode启动过程以及SecondaryNameNode辅助功能"
date: 2018-05-28 09:44:38
draft: false
---
NameNode存储的是整个文件系统的元数据，存放在两个地方:

/*内存

/*本地磁盘: fsimage镜像文件 edits编辑日志文件

第一次启动hdfs时会进行格式化操作，目的就是为了生成fsimage镜像文件，用来存储整个文件系统的元数据

以下是整个流程:

第一次启动:

1)进行hdfs的格式化操作

2)生成fsimage镜像文件

3)启动NameNode: 会读取fsimage镜像文件,获取所有datanode节点元数据信息

4)启动DataNode： 首先datanode会向namenode进行注册,然后会向namenode发送块(元数据)，然后namenode会进行检 查，将datanode的个数与发送块的个数进行比较，如果相等则datanode启动

5)启动后如果进行对文件系统的操作，比如创建文件，上传文件等，这时候会对整个文件系统的命名空间进行改变，而这个改 变会存储在本地磁盘的edits日志文件中，所以说edits日志文件就是对整个文件系统的命名空间和元数据的的改变进行存储

6)所以说NameNode中的元数据存储在两个地方，内存和本地磁盘中，内存中的=本地磁盘中的fsimage+edits

第二次启动:

1)启动NameNode时首先回去读取fsimage镜像文件，然后去读取edits日志文件，最后生成一份新的fsimage镜像文件和edits 日志文件，其中新的fsimage镜像文件把启动时的镜像文件和日志文件整合在了一起，因为读取fsimage镜像文件是非常快的，而 新生成的edits日志文件是空的(null)

2)然后启动DataNode的过程就和第一次启动的一样了，进行注册，发送元数据块，启动后就能够进行对文件系统的操作了，而 这些改变都存储在新生成的edits日志文件中

SecondaryNameNode辅助功能:

辅助NameNode:

上边讲的第二次启动NameNode时生成新的fsimage镜像文件，这个新的镜像文件是将上一个fsimage和edits合并而成的，而这个合并工作就是SecondaryNameNode来完成的，而且是定期就行合并的，因为读取fsimage镜像文件的速度是非常快的，如果edits非常大，不合并的话启动速度会受到很大影响

所以前期自学时，数据量非常小，可以将SecondaryNameNode关闭，不启动这个服务