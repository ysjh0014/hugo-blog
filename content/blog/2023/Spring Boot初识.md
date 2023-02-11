---
title: "Spring Boot初识"
date: 2018-11-23 14:55:12
draft: false
---
**1.Spring Boot的产生**

随着动态语言的流行(Ruby，Scala，Node.js)，Java的开发显得越来越笨重，繁多的配置，低下的开发效率，复杂的部署流程以及第三方技术集成难度大

因此，Spring Boot应运而生，它使用"习惯优于配置"的理念让你的项目快速运行起来，使用Spring Boot很容易创建一个独立运行，准生产级别的基于Spring框架的项目，使用Spring Boot你可以不用或者只需要很少的Spring配置

**1.Spring Boot简介**

Sping Boot的目的在于创建和启动新的基于Spring框架的项目，Spring Boot会选择最适合的Spring子项目和第三方开源库进行整合，大部分Spring Boot应用只需要非常少的配置就可以快速运行起来

Spring Boot是伴随着Spring4.0诞生的

Spring Boot提供了一种快速使用Spring的方式

**2.Spring Boot的优点**

1)为基于Spring的开发提供更快的入门体验

2)创建可以独立运行的Spring应用

3)直接嵌入Tomcat或者Jetty服务器，不需要打包成war文件

4)提供推荐的基础pom文件来简化Apache Maven配置

5)尽可能地根据项目依赖来自动配置Spring框架

6)提供可以直接在生产环境中使用的功能，如性能指标，应用信息

7)开箱即用，没有代码生成，也无需XML配置，同时也可以修改默认值来满足特定的需求

8)其他大量的项目都是基于Spring Boot之上的，如Spring Cloud

Spring Boot使编码，配置，部署，监控变得简单