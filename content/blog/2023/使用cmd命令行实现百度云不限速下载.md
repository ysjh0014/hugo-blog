---
title: "使用cmd命令行实现百度云不限速下载"
date: 2018-05-04 15:52:31
draft: false
---
需要准备工具：Windows10，CMD命令行，BaiduPCS-GO插件。下载完毕后可以存放到任何位置，建议存放到无中文目录内。然后打开我的电脑-属性-高级系统设置-环境变量-系统变量-Path。点击编辑，新建，输入你的BaiduPCS-Go存放目录，看清楚是存放目录

▼如图所示：（我的存放目录是D:\BaiduPCS-Go）

![](https://img.ithome.com/newsuploadfiles/2018/4/20180417_143946_200.jpg "IT之家学院：使用CMD命令行满速下载百度云")

这样CMD才可以正确识别到程序

做好所有准备后，Win+R键运行CMD，输入BaiduPCS-Go，回车键运行

![](https://img.ithome.com/newsuploadfiles/2018/4/20180417_143952_581.jpg "IT之家学院：使用CMD命令行满速下载百度云")

然后会如下显示：

![](https://img.ithome.com/newsuploadfiles/2018/4/20180417_144000_238.jpg "IT之家学院：使用CMD命令行满速下载百度云")

接下来要登陆百度账号。需要使用到以下命令：

利用浏览器的Cookie记录一键登陆，各个浏览器方法不一样，这里我以Chrome浏览器为准。（首先你需要在百度网页上登陆你的账号，否则无效）打开Cookie，找到BDUSS，复制内容（一串字符）

![](https://img.ithome.com/newsuploadfiles/2018/4/20180417_144006_167.jpg "IT之家学院：使用CMD命令行满速下载百度云")

然后在CMD里面键入：login -bduss=（这里也就是你复制的那串字符），千万不输错命令，否则无效！

然后你的账号就完成登陆了：

![](https://img.ithome.com/newsuploadfiles/2018/4/20180417_144011_406.jpg "IT之家学院：使用CMD命令行满速下载百度云")

接下来你就可以去下载文件了，当然是无法直接下载别人分享的文件。你需要将别人分享的文件保存到你自己的网盘内，然后复制文件名（也包括后缀。比如：av249.avi），在CMD里面键入：d+空格+文件名

![](https://img.ithome.com/newsuploadfiles/2018/4/20180417_144016_564.jpg "IT之家学院：使用CMD命令行满速下载百度云")

回车后即可开始下载，下载地址在你保存BaiduPSC-Go的目录里面。需要注意下，下载的文件名字里面不能有空格，下载速度会慢慢变快，需要等待一会儿。

![](https://img.ithome.com/newsuploadfiles/2018/4/20180417_144022_549.jpg "IT之家学院：使用CMD命令行满速下载百度云")

这里提供BaiduPCS-Go下载：[网盘](https://pan.baidu.com/s/1sJ4fiXZLTJ-ewDKKjlilKg) 密码：vx0r