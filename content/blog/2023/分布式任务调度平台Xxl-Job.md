---
title: "分布式任务调度平台Xxl-Job"
date: 2023-06-20 10:24:00
draft: false
featured_image: "https://hugo-ys.oss-cn-hangzhou.aliyuncs.com/static/img/xxl-job.jpg"
tags:
- xxl-iob
categories:
- java
---
#### XXL-JOB是什么
官方是这样介绍的：
> XXL-JOB是一个分布式任务调度平台，其核心设计目标是开发迅速、学习简单、轻量级、易扩展
> 官方文档地址：https://www.xuxueli.com/xxl-job/
> Xxl-Job有两个核心，一个调度中心，一个执行器，需要同时部署才行
#### XXL-JOB怎么用
一、首先部署调度中心
先克隆代码，地址：https://github.com/xuxueli/xxl-job
然后将项目代码db目录下的sql文件在数据库执行，并将项目中application.properties文件中的数据库信息地址为你自己的数据库地址
最后启动xxl-job项目，启动后在浏览器中访问：http://localhost:8080/xxl-job-admin
用户名：admin    密码：123456
![xxl-job](https://hugo-ys.oss-cn-hangzhou.aliyuncs.com/static/img/xxl-job.jpg)
至此调度中心部署完毕
二、部署执行器


