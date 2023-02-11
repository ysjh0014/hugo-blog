---
title: "docker中的hive以及mysql安装"
date: 2018-06-27 16:07:02
draft: false
---
我的hadoop集群是部署在docker上的，所以hive和mysql的安装也要在docker上，在ubuntu系统中使用命令apt-get install mysql-server就可以直接安装mysql，但是在docker容器中使用这个命令会报没有权限的错误，在root下也会报这个错，所以一直没有安装成功，后来使用docker官方镜像安装才成功

1.mysql的安装:

首先拉去镜像

docker pull mysql:5.7

创建用于挂载的目录

mkdir -p /my/mysql/datadir /*用于挂载mysql数据文件

mkdir -p /my/mysql/conf.d /*用于挂载mysql的配置文件

创建容器

docker run --name=mysql -p 3306 -v /my/mysql/datadir:/my/mysql/datadir -v /my/mysql/conf.d:/my/mysql/conf.d -e MYSQL_ROOT_PASSWORD=root -i -t mysql:5.7

命令解析:

-p : 开放容器端口

-v : 挂载宿主目录到容器目录

-e : 指定root密码

连接mysql：

mysql -h+容器ip -uroot -p

输入root密码

2.将mysql作为hive的元数据的存储的数据库

这里前提是你已经安装了hive，并且能够正常运行，可以参考我之前的文章[hive安装部署及初步使用](https://blog.csdn.net/ys_230014/article/details/80743034)，主要就是在hive-en.sh中进行路径的配置和创建相关的目录

要想将mysql作为元数据库，这里只需要再配置一个文件就可以了,首先进入到hive的conf目录下

vi hive-site.xml

在里面输入下边的内容

<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<configuration>
<property>
<name>javax.jdo.option.ConnectionURL</name>
<value>jdbc:mysql://172.17.0.5:3306/hive?createDatabaseIfNotExist=true&amp;useSSL=false</value> /*这里改为你自己的容器ip
</property>
<property>
<name>javax.jdo.option.ConnectionDriverName</name>
<value>com.mysql.jdbc.Driver</value>
</property>
<property>
<name>javax.jdo.option.ConnectionUserName</name>
<value>root</value>
</property>
<property>
<name>javax.jdo.option.ConnectionPassword</name>
<value>root</value>
</property>

</configuration>

将mysql的驱动jar包放到hive的lib目录下

然后再运行hive，就可以在mysql数据库中生成一张hive表，用来保存元数据，如果不使用mysql来保存元数据，就会保存在自带的derby数据库中，这时候如果打开多个hive客户端，就会产生ava.sql.SQLException异常

这里需要注意的是hive的版本最好不要太高，我第一次用的是1.2.2的版本，第一次启动hive正常，再启动时就会报错，除非把mysql中新生成的数据库删除，后来换成0.14.0版本的，就没有这个错误了