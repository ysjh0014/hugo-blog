---
title: "IntelliJ IDEA常用配置"
date: 2018-10-25 17:34:14
draft: false
---
IntelliJ IDEA最大的转变就在于工作空间概念的转变，取消了工作空间的概念，并且在IDEA当中，Project和Module是作为两个不同的概念，简单来说，IDEA不需要设置工作空间，因为每一个Project都具备一个工作空间，对于每一个IDEA的项目工程（Project）而言，它的每一个子模块（Module）都可以使用独立的JDK和MAVEN。这对于传统项目迈向新项目的重构添加了极大的便利性，这种多元化的灵活性正是Eclipse所缺失的，因为开始Eclipse在初次使用时已经绑死了工作空间

**1.设置外观和字体大小**

![](https://img-blog.csdn.net/2018102517050760?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

**2.设置编辑器的快捷键 (keymap)**

![](https://img-blog.csdn.net/20181025170623863?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

**3.全局JDK设置**

顶部工具栏 File ->Other Settins -> Default Project Structure -> SDKs -> JDK

![](https://img-blog.csdn.net/20181025170100156?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

**4.全局Maven配置**

顶部工具栏 File ->Other Settings -> Default Settings -> Build & Tools -> Maven

![](https://img-blog.csdn.net/20181025170152710?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

**5.取消大小写敏感**

File->Settings->Editor->General->Code Completion Case->Sensitive Completion = None

![](https://img-blog.csdn.net/20181025170825313?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

**6.代码检测警告提示等级设置**

提示都是对你好，帮助你提高你的代码质量

![](https://img-blog.csdn.net/20181025170958201?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

**7.自动导入包和智能移除(前提是，这个包没有重名的，要是重名了就得自己手动导入)**

顶部工具栏 File ->Other Settings -> Default Settings -> Auto Import

![](https://img-blog.csdn.net/20181025171227976?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

**8.使用Maven Project管理插件**

![](https://img-blog.csdn.net/20181025171745625?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

**9.自动编译**

顶部工具栏 File ->Other Settings -> Default Settings -> Auto Import

开启自动编译之后，结合Ctrl+Shift+F9 会有热更新效果

![](https://img-blog.csdn.net/20181025173221246?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)