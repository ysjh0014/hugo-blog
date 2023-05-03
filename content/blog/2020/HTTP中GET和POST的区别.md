---
title: "HTTP中GET和POST的区别"
date: 2020-03-25 20:40:21
draft: false
featured_image: "https://hugo-ys.oss-cn-hangzhou.aliyuncs.com/static/img/http.jpg"
---
当你使用Java进行Web开发时，肯定写过很多个GET和POST请求

**它们的区别：**

GET在浏览器回退时是无害的，而POST会再次提交请求

GET的所有参数全部包装在URL中，明文显示，并且服务器的访问日志会记录，非常不安全，POST的URL中只有资源路径，不包含参数，参数封装在二进制的数据体中，服务器不会记录参数，相对安全

GET请求会被浏览器主动缓存，POST不会，除非手动设置

GET请求只能进行URL编码，POST请求支持多种编码

GET请求参数会被完整地保留在浏览器历史记录中，POST不会

GET请求在URL中传送的参数是有长度限制的，POST没有

对参数的数据类型，GET只接受ASCII字符，POST没有限制

GET参数通过URL传递，POST放在Request body中

**但是GET和POST本质上没有区别**

GET和POST是HTTP协议中的两种发送请求的方法

HTTP是基于TCP/IP的关于数据如何在万维网中进行通信的协议

HTTP的底层是TCP/IP，所以GET和POST的底层也是TCP/IP，也就是说，GET和POST都是TCP连接，如果要给GET加上Request body，给POST带上URL参数，技术上是完全行得通的

但是上边的GET和POST的区别是怎么回事？

这是由于HTTP的规定和浏览器/服务器的限制导致它们在应用过程中有一些不同

本质上GET和POST都是TCP连接

**GET和POST有一个重大区别：**

GET产生一个TCP数据包，POST产生两个TCP数据包

对于GET请求，浏览器会把HTTP header和data一并发送出去，服务器相应200

对于POST请求，浏览器先发送header，服务器相应100，浏览器再发送data，服务器相应200

也就是说，GET只需要跑一趟，而POST需要跑两趟，先去打个招呼，然后再把货送过去

但是并不是所有浏览器都会在POST请求中发送两次包，火狐就只发送一次