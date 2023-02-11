---
title: "Linux中的shell编程初识"
date: 2018-11-06 20:11:03
draft: false
---
**1.shell简介**

Shell是Linux的一个外壳，它包在Linux内核的外面，为用户和内核之间的交换提供一个接口

![](https://img-blog.csdnimg.cn/20181106200324843.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==,size_16,color_FFFFFF,t_70)

**2.Shell程序结构**

![](https://img-blog.csdnimg.cn/20181106200934550.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==,size_16,color_FFFFFF,t_70)

**3.Shell编程——Hello World**

最简单的Shell程序就是不包含一条语句，但这是无意义的

Hello World程序只包含一条代码：echo "Hello World"

Shell程序就是包含一系列的Linux命令和控制语句而已

计算机语言分为编程语言和脚本语言

编程语言：

需要编译和链接——>速度快，语法复杂——>c/c++/java/c/#

脚本语言：

不用编译，解析执行——>速度慢，语法简单——>bash/php/python/javascript/asp