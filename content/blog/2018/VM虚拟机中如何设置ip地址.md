---
title: "VM虚拟机中如何设置ip地址"
date: 2018-05-14 20:26:11
draft: false
---
当我们在windows环境下，在cmd命令行中输入ipconfig可以看到我们的主机ip地址，但是我们创建了一台虚拟机，并且装好系统时，输入ifconfig(这里和windows下命令不一样，不要搞混了)时，会发现得不到ip地址，下面就说一下如何设置虚拟机的ip

首先我们找到VM软件的顶部:虚拟机---》设置，然后在设置里面修改网络适配器为NET模式

![](https://img-blog.csdn.net/20180514200430165)

这个时候应该就可以连上网络并得到ip地址了，这个ip是主机电脑分配给虚拟机的，所以是可以连接上互联网的，你可以在linux中输入ping www.baidu.com可以ping的通，但是动态ip用起来不方便，我们一般设置为静态ip，即不变的

我们右键网络图标，然后点击Edit Connections

![](https://img-blog.csdn.net/20180514202026280)

然后点击Edit-----》IPV4 Settings

![](https://img-blog.csdn.net/20180514202319673)

将Method改为手动，然后在Addresses中随便添加一个ip地址，如上图所示，设置好之后返回命令行，先关闭网络再重启网络后输入ifconfig就可以看到ip地址就是你刚刚设置的了，不会变化的

注意:上边说的随便添加一个ip地址，在虚拟机中是可以ifconfig出来的，但是在windows主机下是不能ping通的，所以如果想能够远程连接到该虚拟机，应该把固定ip设为和windows主机一个ip字段的，即ip前三段一致，这样才能够连接到