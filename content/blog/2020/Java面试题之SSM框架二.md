---
title: "Java面试题之SSM框架二"
date: 2020-04-16 13:12:00
draft: false
featured_image: "https://hugo-ys.oss-cn-hangzhou.aliyuncs.com/static/img/java.png"
tags:
- Java
categories: 
- Java
---
**1.SpringMVC的工作流程**

SpringMVC是一种基于Spring实现了Web MVC设计模式的请求驱动类型的轻量级Web框架，使用了MVC架构模式的思想

工作流程：

1.用户将请求发送给前端控制器

2.前端控制器收到请求调用处理器映射器

3.处理器映射器根据请求url找到具体的处理器，生成处理器对象和处理器拦截器，将其返回给前端控制器

4.前端控制器通过处理器适配器调用处理器

5.执行处理器

6.处理器执行完后返回ModelAndView

7.处理器适配器将ModelAndView返回给前端控制器

8.前端控制器将ModelAndView传给视图解析器

9.视图解析器解析后返回具体的View给前端控制器

10.前端控制器对View进行渲染(将模型数据填充到视图中)

11.前端控制器对用户进行响应

**2.Mybatis一级缓存、二级缓存**

一级缓存基于SqlSession，默认开启，在操作数据库的时候需要构造SqlSession对象，在对象中有一个HashMap用于存储缓存数据，当在同一个SqlSession中执行两次相同的SQL语句时，第一次执行后会将数据库中查询到的数据写入到缓存中，第二次查询就会从缓存中直接获取数据，提高查询效率

如果SqlSession执行了增删改操作，并且提交到了数据库，就会清空SqlSession中的一级缓存，这样可以保证缓存中存储的是最新的数据，避免出现脏读现象

二级缓存的作用域是mapper中的namespace，不同的SqlSession在同一个namespace下执行两次相同的SQL语句，得到的数据会存储在二级缓存中，同样是使用HashMap进行数据存储的，相比于一级缓存，二级缓存的范围更大，多个SqlSession可以共用二级缓存，二级缓存是跨SqlSession的

一级缓存是默认开启的，二级缓存需要手动开启