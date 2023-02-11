---
title: "SpringCloud之注册中心Eureka"
date: 2019-05-17 10:18:18
draft: false
---
**1.创建服务注册中心(Eureka Server)**

首先在IDEA中创建一个SpringBoot项目

![](https://img-blog.csdnimg.cn/20190517095303219.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==,size_16,color_FFFFFF,t_70)

一直Next之后，进行到下面这张图时按照图片上的进行选择

![](https://img-blog.csdnimg.cn/20190517095500544.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==,size_16,color_FFFFFF,t_70)

这里选择Eureka Server可以直接在项目的pom文件中加入Eureka的jar包

然后想要启动一个注册中心只要在application.properties中进行端口等的配置
server.port=8761 eureka: instance: hostname: localhost client: registerWithEureka: false fetchRegistry: false serviceUrl: defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/ spring.application.name=eurka-server

在SpringBoot的启动类中添加一个@EnableEurekaServer注解

![](https://img-blog.csdnimg.cn/20190517100137183.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==,size_16,color_FFFFFF,t_70)

启动之后，可以在浏览器进行访问：localhost:8761

![](https://img-blog.csdnimg.cn/201905171006208.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==,size_16,color_FFFFFF,t_70)

可以看到此时注册中心中还没有client

**2.创建一个服务提供者(Eureka Client)**

创建一个Client向Server进行注册

同样的，跟创建Server时的步骤一样，首先创建一个SpringBoot项目，然后进行application.properties配置文件的配置
server.port=8762 eureka: client: serviceUrl: /#服务器地址 defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/ /#服务名 spring.application.name=service-client

在启动类中加入注解@EnableEurekaClient

![](https://img-blog.csdnimg.cn/20190517101124805.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==,size_16,color_FFFFFF,t_70)

启动Client之后，可以将Server的浏览器页面进行刷新，查看Client是否注册成功

![](https://img-blog.csdnimg.cn/20190517101626524.png)

你会看到一个Client已经注册在Server中了，服务名是SERVER-CLIENT，端口号是8762