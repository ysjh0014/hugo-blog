---
title: "使用SpringBoot+Swagger编写API接口"
date: 2019-05-16 10:13:49
draft: false
---
开发环境：

IDEA

SpringBoot

Swagger

Maven

首先我们要使用IDEA创建一个SpringBoot的项目，如果IDEA中没有安装SpringBoot插件的话，先在Setting---》Plugins中搜索Spring Assistant并下载安装

然后就可以创建SpringBoot项目了

![](https://img-blog.csdnimg.cn/20190516111656369.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==,size_16,color_FFFFFF,t_70)

创建SpringBoot项目比较简单，接口编写也比较简单，这里先放一张编写好的接口项目目录结构

![](https://img-blog.csdnimg.cn/20190516112227301.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==,size_16,color_FFFFFF,t_70)

可以看出，跟ssm框架编写的差不多，基本就是Servie，Controller，Mapper这几个模块，并且使用了SpringBoot后，配置文件减少了很多，但是接口的编写因为没有页面进行测试，之前都是使用postman进行测试，但是使用Swagger更加方便和简洁，再放一张Swagger的效果图

![](https://img-blog.csdnimg.cn/20190516112945556.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==,size_16,color_FFFFFF,t_70)

想要加入Swagger，首先在pom文件中加入Swagger的jar包，然后再进行Swagger的引入即可，具体代码我放在github上了，大家可以clone下来然后直接在IDEA中import该项目即可

github代码地址：

[https://github.com/ysjh0014/warehouse/tree/master/springboot-demo](https://github.com/ysjh0014/warehouse/tree/master/springboot-demo)