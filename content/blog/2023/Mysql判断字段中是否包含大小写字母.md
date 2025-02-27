---
title: "Mysql判断字段中是否包含大小写字母"
date: 2023-06-20 10:24:00
draft: false
featured_image: "https://hugo-ys.oss-cn-hangzhou.aliyuncs.com/static/img/xxl-job.jpg"
tags:
- Mysql
categories:
- Mysql
---
#### 判断字段是否包含字母，不区分大小写
`select * from 表名 where 字段名 REGEXP '[A-Z]';`
#### 判断字段是否包含字母，区分大小写
//只查包含大写字母的字段
`select * from 表名 where 字段名 REGEXP BINARY '[A-Z]';`
//只查包含小写字母
`select * from 表名 where 字段名 REGEXP BINARY '[a-z]';`


