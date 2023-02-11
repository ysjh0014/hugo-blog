---
title: "框架----springmvc(一)"
date: 2018-04-03 08:26:10
draft: false
---
SpringMVC是一个web层的mvc框架，是spring的部分，类似于struts2框架

要想学习SpringMVC首先要理解web层和mvc设计模式，javaee体系结构分为4层：应用层 web层 业务层 持久层，下面上张图来理解：

![](https://img-blog.csdn.net/20180330151252880?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

mvc设计模式：

这个设计模式想来大家都不陌生，但是为什么会有mvc以及mvc的好处是什么呢？

这就要提到没有mvc设计模式之前了：

![](https://img-blog.csdn.net/20180330151634307?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

可以看出没有mvc之前的执行流程各层之间耦合度太高，也就是各层之间关系太紧密，如果一层需要修改，对其他层的影响太大，所以mvc设计模式应运而生，它的做用就是解耦和，而mvc设计模式的思想就是：任何重定向都能解耦和

![](https://img-blog.csdn.net/20180330152155407?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

图中的模型层就是JavaBean组件：领域模型(即封装好了的属性变量和get,set方法)+业务层+持久层

SpringMVC的执行流程：

![](https://img-blog.csdn.net/20180330152632775?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

看流程图上的12个步骤，需要我们实现的只有Action和View以及在前端控制器中进行接受和转发请求即可，像处理器映射器，处理器适配器，视图解析器只需要在配置文件中进行配置即可，

SpringMVC和MVC的流程图进行对比，你会发现SpringMVC是严格按照mvc的设计模式来设计的，只是将model层细化了

SpringMVC代码实现：

因为SpringMVC是一个web层框架，所以应该创建一个web工程，而不是像mybatis(持久层框架)创建一个java工程

首先导入jar包：

![](https://img-blog.csdn.net/20180403081227343)

然后根据流程图能看出先要设置前端控制器DispatcherServlet和拦截器，在web.xml中配置，web.xml是整个web项目的入口

![](https://img-blog.csdn.net/20180403081716743)![](https://img-blog.csdn.net/20180403081721628)

然后配置springmvc的配置文件，先创建springmvc.xml来配置处理器映射器和处理器适配器

![](https://img-blog.csdn.net/20180403082057331)

创建完之后还需要加载springmvc的配置文件，也是在web.xml中加载的

![](https://img-blog.csdn.net/20180403082558894)

最后创建action方法

![](https://img-blog.csdn.net/20180403082216527)

在WEB-INF目录下创建jsp/index,jsp,如果直接在WEB-INF下创建index.jsp,就算action方法中的路径写对，也会报错