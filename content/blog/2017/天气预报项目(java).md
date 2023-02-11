---
title: "天气预报项目(java)"
date: 2017-11-11 20:57:34
draft: false
---
### **这是一个天气预报的爬虫，是用java代码写得，首先向大家展示一下效果**

### **比如查询南阳本地的天气：**

### **![](https://img-blog.csdn.net/20171111210043461?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveXNfMjMwMDE0/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)**

### **我们先来理一下思路，实现这个功能总共分三步：
1.利用http请求对中国天气气象网进行请求
2.在中国天气气象网的api中你会发现每个城市是有对应的编码，所以我们要把城市转换成对应的编码
3.对中国天气气象网返回的数据进行解析，**

## **一 解析URL**

### **首先要对URL进行解析了，我们是对**中国天气气象局的网站进行爬取**，那么我们首先对URL进行分析：**

### **http://weather.123.duba.net/static/weather_info/101010100.html
**我们通过观察就会发现这个请求里面只有1个参数：要查询的城市编码****

### ****![](https://img-blog.csdn.net/20171112102031157?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveXNfMjMwMDE0/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)**
**二将城市名称转换为相对应的城市编码**
**我们可以用一个Map集合来实现，把城市名设为key,城市编码设为value****
**然后就可以根据key值找到相对应的value值**
**首先存放在Map集合中**

**![](https://img-blog.csdn.net/20171111214029665?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveXNfMjMwMDE0/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)**

### ****然后进行根据key值****找value值![](https://img-blog.csdn.net/20171111214530400?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveXNfMjMwMDE0/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

## 三 解析天气数据

### **我们将上面的URL复制到浏览器中会得到下面的：**

![](https://img-blog.csdn.net/20171111213038318?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveXNfMjMwMDE0/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

### **这是网页返回给我们的数据，我们需要从中解析出我们想要的部分,这里因为返回的不是标准的json格式的数据，所以用正则表达式来解析：**

**![](https://img-blog.csdn.net/20171112102315710?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveXNfMjMwMDE0/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)**

### **对于正则表达式我不是很了解，大家可以去百度学习一下哦**

**这个小玩意是用java写的，属于后台实现，没有与前端页面进行交互，算是初级版吧**