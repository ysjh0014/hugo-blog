---
title: "Docker基本命令"
date: 2018-06-06 17:46:36
draft: false
---
**1.启动容器**

/*启动运行单次命令的容器:

docker run ubuntu echo 'hello world'

/*启动交互式容器:

docker run -i -t ubuntu /bin/bash

上边这两种方式都是运行完或退出后容器就会停止，

**2.查看容器**

docker ps :查看正在运行的容器，不包括已经创建但停止运行的容器

docker ps -a:查看所有容器，包括停止运行的容器

docker inspect+容器id或者name:查看容器的具体配置信息

**3.自定义容器**

docker run --name=容器名 -i -t ubuntu /bin/bash:自定义容器名

**4.重启容器**

docker start 容器名:重启并后台运行容器

docker start -i 容器名:以交互式方式重启容器

**5.删除容器**

docker rm 容器名:只能用来删除已经停止的容器

**6.守护式容器**

docker run --name=容器名 -i -t ubuntu /bin/bash ctrl+p ctrl+q :启动一个守护式容器，即退出后容器仍在后台运行

docker attach+容器名:重新以交互式进入到容器中

上边的是先启动一个交互式容器，再让它在后台运行，下面的事直接启动一个守护式容器:

docker run --name=容器名 -d ubuntu /bin/sh -c "while true;do echo hello world;sleep 2;done"

一个守护式容器一旦执行完任务也会关闭，所以上边使用了shell编程，循环输出，这时候要想看容器的执行情况可以查看容 器的日志

docker logs+容器名:返回所有的日志

docker logs -t+容器名:返回所有的日志包括时间

docker logs -tf+容器名:动态返回所有的日志

docker logs -tf --tail+数字+容器名:可以指定返回最新的几条

**7.容器的进程**

docker top 容器名:查看容器内的进程

docker exec [-d] [-i] [-t] 容器名 /bin/bash:在运行的容器中启动新的进程

**8.停止容器**

docker stop 容器名:发送一个信号等待容器的停止

docker kill 容器名:直接停止容器

**9.进入容器**
docker exec -it mysql bash

**10.添加hosts**

--add-host cdh0：172.17.0.2

**11.重命名容器名**

docker rename 原容器名 新容器名

**12.查看容器ip**

docker inspect 容器ID | grep IPAddress