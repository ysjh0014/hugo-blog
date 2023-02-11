---
title: "Ajax插件mockjax模拟json数据"
date: 2019-05-30 10:32:12
draft: false
---
大家开发前端页面的时候，特别是前后端分离之后，经常会遇见在进行前端开发时，后端接口还没有开发好，这时候要求我们进行测试，没有接口就请求不到数据，如何测试呢？

这时候就可以用到模拟数据的Ajax插件mockjax，下面就通过代码说一下mockjax

首先我们看一段Ajax的代码：
$.ajax({ url: "http://localhost:8764/bpm", type: "get", dataType: "json", success: function (data) { console.log(data); } })

可以看到这里的url是没有对应的后端接口给我们的，此时就可以愉快的用到mockjax了

首先我们要下载并引入mockjax的插件：

mockjax下载地址：[https://github.com/jakerella/jquery-mockjax](https://github.com/jakerella/jquery-mockjax)

记得引入：
<script type="text/javascript" src="js/jquery.mockjax.js"></script>

将下边的mockjax代码写在Ajax的前边：

$.mockjax({ url: 'http://localhost:8764/bpm', // 返回的HTTP状态码 status: 200, // 响应时间 responseTime: 750, // 响应的HTTP内容类型 contentType: 'application/json', // 返回的内容 responseText: { "user": [{ "id": 1, "name": "david", "birthday": "2001/01/26" }] } });

此时我们在浏览器运行前端页面在F12开发者工具中就可以看到数据了

但是mockjax只能用于少量数据，如果我们需要大量数据来测试的话应该怎么办呢，总不能使用mockjax一个一个手写吧

这时候就要用到mockJSON插件了

mockJSON下载地址：[https://github.com/mennovanslooten/mockJSON](https://github.com/mennovanslooten/mockJSON)

记得引入：
<script type="text/javascript" src="js/jquery.mockjson.js"></script>

下面就放代码看mockJSON如何使用：

$.mockJSON.data.USER_NAME = ['张三', '李四', '王五', '赵六', '孙七', '周八', '吴九', '郑十']; $.mockjax({ url: 'http://localhost:8764/bpm', // 返回的HTTP状态码 status: 200, // 响应时间 responseTime: 750, // 响应的HTTP内容类型 contentType: 'application/json', // 返回的内容 responseText: $.mockJSON.generateFromTemplate({ "user|3-6": [{ "id|+1": 1, "name": "@USER_NAME" }] }) });

接释一下responseText里面的字段：

user|3-6：产生3到6组数据

id|+1：id每次加1

"name": "@USER_NAME"：从USER_NAM中随机取数据

每次刷新页面的时候都可以看到数据不一样