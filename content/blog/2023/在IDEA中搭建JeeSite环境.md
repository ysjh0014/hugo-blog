---
title: "在IDEA中搭建JeeSite环境"
date: 2018-11-21 19:20:57
draft: false
---
JeeSite 是一个 Java EE 企业级快速开发平台，基于经典技术组合（Spring Boot、Spring MVC、Apache Shiro、MyBatis、Beetl、Bootstrap、AdminLTE），在线代码生成功能，包括核心模块如：组织机构、角色用户、菜单及按钮授权、数据权限、系统参数、内容管理、工作流等。采用松耦合设计；界面无刷新，一键换肤；众多账号安全设置，密码策略；在线定时任务配置；支持集群，支持SAAS；支持多数据源

开发环境：

Java SDK 1.8

IDEA

Apache Maven 3.3.0+

MySql 5.7.11+

1.导入到IDEA

1)检出JeeSite4源代码：
git clone https://gitee.com/thinkgem/jeesite4.git

2)拷贝

web
文件夹，到你的工作目录（不包含中文和空格的目录）下，重命名为你的工程名，如：

jeesite-demo

3)打开

pom.xml
文件，修改第13行，artifactId为你的工程名，如：

<artifactId>jeesite-demo</artifactId>

4)导入到IDEA，导入的时候直接按Maven项目导入即可，一直按Next按钮即可

5)这时，IDEA会自动加载Maven依赖包，初次加载会比较慢（根据自身网络情况而定），若工程上有小叉号，请打开Problems窗口，查看具体错误内容，直到无错误为止

注意：

这里初次导入可能pom文件会报错，然后并不会下载jar包，这是因为你的pom中没有项目的标识，可以加入如下代码：
<groupId>cn.ysjh</groupId> <version>1.0-SNAPSHOT</version>

这里可以自定义groupId

2.在mysql数据库中创建用户和授权
set global read_only=0; set global optimizer_switch='derived_merge=off'; create user 'jeesite'@'%' identified by 'jeesite'; create database jeesite DEFAULT CHARSET utf8 COLLATE utf8_general_ci; grant all privileges on jeesite./* to 'jeesite'@'%'; flush privileges;

然后就可以导入数据库的表和数据了，执行项目文件夹的bin目录中的init-data.bat

注意：

这里的init-data.bat是要在maven中执行的，所以必须在windows下配置好maven，在cmd中使用mvn进行测试

这里的init-data.bat脚本执行可能需要一点时间

3.修改项目中的JDBC连接

在/src/main/resources/config/application.yml中进行修改

4.启动项目

当前是 Spring Boot 工程，内部已经集成 Web 容器，你无需另外再下载 Tomcat 进行部署，只需按照以下方式进行即可

打开

/src/main/resources/config/application.yml
文件，配置你的服务端口

port
、部署路径

context-path
，例如：
server: port: 8980 servlet: context-path: /jeesite-demo tomcat: uri-encoding: UTF-8

当然也可以在Tomcat中运行

最后使用浏览器访问：

localhost:8980/jeesite-demo

默认最高管理员账号：system

密码：admin