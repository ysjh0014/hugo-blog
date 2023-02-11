---
title: "在Docker中部署静态网站"
date: 2018-06-09 12:08:11
draft: false
---
创建一个带端口映射的交互式容器

docker run -p 80 --name=web -i-t ubuntu /bin/bash /*这里的web是容器名,可以自定义

在容器中下载nginx和vim

apt-get update

apt-get install nginx

apt-get install vim

创建一个简单的html页面

mkdir /opt/html

cd /opt/html

vi index.html /*这里在index.html中编写一个简单的html页面

修改nginx的配置文件

cd /etc/nginx/sites-enabled/

vi default

将default中的root后面的目录改为刚存放html页面的目录，保存并退出

启动nginx

nginx

ps -ef /*查看nginx是否已经启动

![](https://img-blog.csdn.net/20180609120804955)

按ctrl+p,ctrl+q，让容器在后台一直运行

查看端口映射

在root账户下使用docker port+容器名来查看端口映射

![](https://img-blog.csdn.net/20180609120546985)

最后检查是否能访问此html页面

curl http://127.0.0.1:32768/index.html

如果返回了你自己写的html代码，即代表已经成功部署了