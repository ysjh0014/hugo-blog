---
title: "Zookeeper的单机模式安装"
date: 2018-04-26 19:18:38
draft: false
---
本文以centos7系统为例,不过无论是虚拟机还是远程云服务器都是一样的操作

1.首先下载zookeeper的压缩包，然后把压缩包传到服务器上，使用命令解压:

![](https://img-blog.csdn.net/20180426185854342)

2.然后进入到zookeeper目录下，创建文件夹data

![](https://img-blog.csdn.net/20180426190053178)

3.进入zookeeper的conf目录下，修改zoo_sample.cfg为zoo.cfg

![](https://img-blog.csdn.net/20180426190426358)

4.编辑zoo.cfg

![](https://img-blog.csdn.net/20180426190612288)

输入上边命令后再输入i开始编辑，修改其中的![](https://img-blog.csdn.net/20180426190743797)，然后按Esc并输入:wq退出并保存

5.启动zookeeper

进入bin目录

![](https://img-blog.csdn.net/20180426191608500)

再使用如下命令查看zookeeper的状态

![](https://img-blog.csdn.net/20180426191712201)

如上图所示则启动成功

最后提示一点，需要关闭防火墙，否则zookeeper不能被访问，端口号是2181

关闭防火墙的命令:

service iptablesstop(相当于临时关闭防火墙，下次开机时会再次启动)

chkconfig iptables off(永久关闭防火墙)

如果还没有启动成功，则可以把data目录下的文件删除

6.zookeeper的客户端模式

使用命令 ./zkCli.sh 就可以进入zookeeper的客户端

zookeeper客户端也有一些命令，如下:

![](https://img-blog.csdn.net/20180628185235933?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

查看目录结构:

![](https://img-blog.csdn.net/20180628185447862?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

创建一个目录:

![](https://img-blog.csdn.net/2018062818533152?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

查看目录内容:

![](https://img-blog.csdn.net/2018062818541685?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

删除目录:

![](https://img-blog.csdn.net/20180628185518197?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

输入quit即可退出客户端模式