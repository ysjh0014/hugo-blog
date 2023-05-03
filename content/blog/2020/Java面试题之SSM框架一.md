---
title: "Java面试题之SSM框架一"
date: 2020-03-03 16:51:11
draft: false
featured_image: "https://hugo-ys.oss-cn-hangzhou.aliyuncs.com/static/img/java.png"
tags:
- Java
categories: 
- Java
---
**1.不同版本的Spring有哪些主要功能？**
版本 特征 Spring2.5 发布于2007年，这是第一个支持注解的版本 Spring3.0 发布于2009年，完全利用了Java5中的改进，并为JEE6提供了改进 Spring4.0 发布于2013年，这是第一个完全支持JAVA8的版本

**2.什么是Spring？**

Spring是一个Java企业级应用的开源开发框架，旨在降低应用程序开发的复杂度，它是轻量级的，松散耦合的，它具有分层体系结构，允许用户选择组件，还可以集成其他框架，所以又被称为框架的框架

**3.列举Spring的优点？**

轻量：Spring是轻量的，基本的版本大约2M

控制反转：Spring通过控制反转实现了松散耦合，对象们给出它们的依赖，而不是创建或者查找依赖的对象们

面向切面的编程：Spring支持面向切面的编程，并且把应用业务逻辑和系统服务分开

容器：Spring包含并管理应用中对象的生命周期和配置

MVC框架：Spring的WEB框架是个精心设计的框架，是WEB框架的一个很好的替代品

事务管理：Spring提供一个持续的事务管理接口，可以扩展到上至本地事务下至全局事务

异常处理：Spring提供方便的API把具体技术相关的异常转化为一致的unchecked异常

**4.Spring IOC/DI是什么？什么是Spring 容器？IOC的优缺点？**

参考文章：

[Spring IOC/DI介绍](https://blog.csdn.net/ys_230014/article/details/88084482)

**5.Spring IOC的实现机制**

Spring IOC的实现机制就是工厂模式加反射机制

代码示例：
interface Fruit { public abstract void eat(); } class Apple implements Fruit { public void eat(){ System.out.println("Apple"); } } class Orange implements Fruit { public void eat(){ System.out.println("Orange"); } } class Factory { public static Fruit getInstance(String ClassName) { Fruit f=null; try { f=(Fruit)Class.forName(ClassName).newInstance(); } catch (Exception e) { e.printStackTrace(); } return f; } } class Client { public static void main(String[] a) { Fruit f=Factory.getInstance("io.github.dunwu.spring.Apple"); if(f!=null){ f.eat(); } } }

**6.Spring中依赖注入的方式**

在Spring中最常见的依赖注入方式有三种：

构造函数注入

Setter注入

接口注入

参考文章：

[Spring常用的三种依赖注入方式](https://blog.csdn.net/qq594913801/article/details/80512451)

**7.区分构造函数注入和Setter注入**
构造函数注入 Setter注入 没有部分注入 有部分注入 不会覆盖Setter属性 会覆盖Setter属性 任意修改都会创建一个新实例 任意修改不会创建一个新实例 适用于设置很多属性 适用于设置很少属性

**8.区分BeanFactory和ApplicationContext**

参考文章：

[Spring系列之BeanFactory和ApplicationContext](https://www.cnblogs.com/xiaoxi/p/5846416.html)