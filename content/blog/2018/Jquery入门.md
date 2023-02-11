---
title: "Jquery入门"
date: 2018-04-05 12:02:27
draft: false
---
作为一款轻量级的Javascript库，Jquery有许多优点，是对JS的封装，但是不是全部封装，毕竟有些JS是封装不了的

而Jquery就是依赖JS的，学习Jquery首先要导入Jquery库，然后要会看Jquery的文档，并且Jquery2.0版本以后不再支持IE6.0,7.0,8.0, .js是源码版，.min.js是压缩版

1.传统JS与Jquery的比较

![](https://img-blog.csdn.net/20180405105233155)

可以很清楚的看出，使用Jquery代码会很简洁

2.JS和JQ对象的相互转换

![](https://img-blog.csdn.net/20180405105738637)

虽然JQ是对JS的封装，但是JS对象只能调用JS的API，JQ也只能调用JQ的API,但是一般都是JS对象转换成JQ对象，

JQ本质是一个数组，数组中是一个个的JS对象

3.JS对象与JQ对象的比较

JS的三种基本定位方式：

id属性：document.getElementById() name属性： document.getElementsByName() 标签名：document.getElementsByTagName()

JQ的三种基本定位方式：

"/#id"定位 "标签名"定位 "样式名"定位

![](https://img-blog.csdn.net/20180405120102591)

JS对象和JQ对象对错误的处理方式：

![](https://img-blog.csdn.net/20180405120205331)