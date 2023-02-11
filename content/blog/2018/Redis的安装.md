---
title: "Redis的安装"
date: 2018-08-01 17:54:02
draft: false
---
Redis简介

Redis(REmote DIctionary Server)远程字典服务器，是完全开源免费的，用c语言编写的，遵守BSD协议，是一个高性能的(key-value)分布式内存数据库，基于内存运行，并支持持久化的Nosql数据库，是当前最热门的Nosql数据库之一，也被称为数据结构服务器

Redis与其他key-value缓存数据库相比有三个特点:

Redis支持数据的持久化，可以将内存中的数据保存在磁盘中，重启的时候可以再次加载进行使用

Redis不仅支持简单的key-value类型的数据，同时还提供list,set,zset,hash等数据结构的存储

Redis支持数据的备份，及master-slave模式的数据备份

Redis安装

Redis的下载地址: [redis.io](http://redis.io)(英文官网)

[redis.cn](http://redis.cn)(中文)

Redis要部署在liunx环境下(这里以虚拟机中的ubuntu14.04.5为运行环境)，首先上传压缩包，解压到相应的目录

然后进入到Redis目录下,依次输入以下命令

make

cd src

make install

为了让redis能够在后台运行，需要修改配置文件

vi redis.conf

![](https://img-blog.csdn.net/20180801174630137?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

如图修改即可

然后进入到redis的src目录下,指定配置文件的位置

redis-server /opt/redis/redis.conf /*这里需要替换为你自己的redis.conf的位置

![](https://img-blog.csdn.net/20180801175126946?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

redis-cli

然后进行简单的测试

![](https://img-blog.csdn.net/20180802093301779?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

redis-benchmark ：可以对Redis性能进行测试

![](https://img-blog.csdn.net/20180802092945983?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

可以看出Redis的读写性能很高，能达到每秒钟13万次

Redis杂项基础知识:

1.Redis是单进程的，单进程模式来处理客户端的请求，对读写等事件的响应

是通过epoll函数的包装来做到的，Redis的实际处理速度完全依赖主进程的执行效率

Epoll是Linux内核为处理大批量文件描述符而做了改进的epoll，是linux下多路复用IO接口select/poll的增强版本

能显著提高程序在大量并发连接中只有少量活跃的情况下的系统CPU利用率

2.Redis默认有16个数据库，类似数组下标从0开始，初始默认使用0号库

3.使用select命令切换数据库，dbsize可以查看当前数据库的key的个数，keys /*可以查看所有的key,keys也支持占位符查找 (注意Redis也支持tab键补全)

![](https://img-blog.csdn.net/20180802095601998?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

4. FLUSHALL:删除所有库的数据

FLUSHDB:删除当前库的所有数据