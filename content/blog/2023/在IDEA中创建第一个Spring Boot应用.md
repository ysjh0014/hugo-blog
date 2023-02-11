---
title: "在IDEA中创建第一个Spring Boot应用"
date: 2018-11-23 19:43:03
draft: false
---
**1.创建springboot项目**

![](https://img-blog.csdnimg.cn/20181123193648891.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==,size_16,color_FFFFFF,t_70)

Next后边的步骤都是平时用的，这里就不再放图出来了

**2.编写一个简单代码进行测试**

在SpringbootApplication.java中：
package cn.ysjh.springboot2; import org.springframework.boot.SpringApplication; import org.springframework.boot.autoconfigure.SpringBootApplication; import org.springframework.context.annotation.Configuration; import org.springframework.stereotype.Controller; import org.springframework.web.bind.annotation.RequestMapping; import org.springframework.web.bind.annotation.ResponseBody; @SpringBootApplication @Controller @Configuration public class SpringbootApplication { @RequestMapping("hello") @ResponseBody public String hello(){ return "hello ys！"; } public static void main(String[] args) { SpringApplication.run(Springboot2Application.class, args); } }

注解说明：

@SpringBootApplication：Spring Boot项目的核心注解，主要目的是开启自动配置

@Configuration：这是一个配置Spring的配置类

@Controller：标明这是一个SpringMVC的Controller控制器

**3.运行**

直接运行SpringbootApplication.java中的main方法

![](https://img-blog.csdnimg.cn/20181123194201436.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==,size_16,color_FFFFFF,t_70)

**4.在浏览器中查看**

输入 localhost:8080/hello 就可以看到效果