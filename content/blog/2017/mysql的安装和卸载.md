---
title: "mysql的安装和卸载"
date: 2017-09-17 11:28:47
draft: false
---
### mysql的卸载：

### 1.启动cmd->输入services.msc->找到mySQL->停止SQL服务

**2.删除安装目录中残留的mysql文件:在安装目录中找到mysql，删除残留的mysql文件**

**3.删除隐藏的mysql文件:找到安装盘下的programdata文件夹，可能会隐藏，需要在工具中开启隐藏文件夹，然后删除其中的mysql文件。**

### mysql的安装:

### 1.安装时除了安装路径可以自己选择以外，其他都按默认，直接点击next;

### 2.安装成功后需要进行mysql的配置:①选择配置方式，“Detailed Configuration（手动精确配置）”、“Standard Configuration（标准配置）”，我们选择“Detailed Configuration”，方便熟悉配置过程。
②选择服务器类型，“Developer Machine（开发测试类，mysql占用很少资源）”、“Server Machine（服务器类型，mysql占用较多资源）”、“Dedicated MySQL Server Machine（专门的数据库服务器，mysql占用所有可用资源）”
③选择mysql数据库的大致用途，“Multifunctional Database（通用多功能型，好）”、“Transactional Database Only（服务器类型，专注于事务处理，一般）”、“Non-Transactional Database Only（非事务处理型，较简单，主要做一些监控、记数用，对MyISAM数据类型的支持仅限于non-transactional），按“Next”继续。
④选择网站并发连接数，同时连接的数目，“Decision Support(DSS)/OLAP（20个左右）”、“Online Transaction Processing(OLTP)（500个左右）”、“Manual Setting（手动设置，自己输一个数）”。
⑤是否启用TCP/IP连接，设定端口，如果不启用，就只能在自己的机器上访问mysql数据库了，在这个页面上，您还可以选择“启用标准模式”（Enable Strict Mode），这样MySQL就不会允许细小的语法错误。如果是新手，建议您取消标准模式以减少麻烦。但熟悉MySQL以后，尽量使用标准模式，因为它可以降低有害数据进入数据库的可能性。按“Next”继续
⑥对mysql默认数据库语言编码进行设置（重要），一般选UTF-8，按“Next”继续

⑦询问是否要修改默认root用户(超级管理)的密码,“Enable root access from remote machines(是否允许root用户在其它机器上登录,如果要安全,就不要勾上,如果要方便,就勾上它)”,最后“Create An Anonymous Account(新建一个匿名用户,匿名用户可以连接数据库,不能操作数据,包括查询)”,一般就不用勾上了,设置完毕,按“Next”继续