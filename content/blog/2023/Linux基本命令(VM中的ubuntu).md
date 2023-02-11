---
title: "Linux基本命令(VM中的ubuntu)"
date: 2018-05-20 18:56:28
draft: false
---
1.创建用户，用户间切换

/#表示是root用户 $表示是普通用户

创建用户: useradd 用户名

创建密码:passwd 用户名

切换用户: su 用户名

2.主机名与ip

1)修改主机名：

hostname 主机名(暂时修改)

永久修改:使用命令 vi etc/hostname 将里面的原主机名修改为你想要的主机名，然后重启就会永久修改了

2) 设置固定的静态ip的方法在我的上一篇博客中已经提到：[VM虚拟机中如何设置ip地址](https://blog.csdn.net/ys_230014/article/details/80311121)

3)主机名与ip地址的映射:

使用命令 vi etc/hosts 在hosts中添加ip地址和主机名保存即可

3.创建文件，文件夹等

mkdir +文件夹名称

touch +文件名称

4.查看内存磁盘信息

free -h: 查看内存信息

df -h: 查看磁盘信息

5.解压.rar文件

unrar x ./文件名