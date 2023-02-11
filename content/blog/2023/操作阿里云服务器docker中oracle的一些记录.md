---
title: "操作阿里云服务器docker中oracle的一些记录"
date: 2018-12-16 09:14:38
draft: false
---
首先我的oracle是在docker中安装的(因为感觉在windows下安装太麻烦，并且不好卸载)，并且我的docker是运行在阿里云服务器上的，这样只需要远程连接操作即可

**1.连接到oracle**

使用以下命令进入到docker镜像中
docker exec -it 装有oracle的docker镜像名 bash

使用dba权限登录到sqlplus

sqlplus / as sysdba

输入用户名密码即可登录到oracle

![](https://img-blog.csdnimg.cn/2018121019422640.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==,size_16,color_FFFFFF,t_70)

**2.使用sqlplus查看当前用户，系统参数，修改初始化参数**

查看当前用户
show user;

查看系统参数

show parameters;

查看具体参数

show parameter db_block_size;

**3.创建用户并赋予权限**

创建用户
create user 用户名 identified by 密码;

查看所有用户

select /*from all_users;

查看当前用户

select /*from user_users;

修改用户密码

alter user 用户名 identified by 新密码;

删除用户

drop user 用户名 cascade;

赋予用户登录的权限

grant create session to 用户名;

赋予用户建表的权限

grant create table to 用户名;

赋予用户所有权限

grant dba to 用户名;

**4.表空间**

创建表空间

查看表空间存储位置目录
select file_name from dba_data_files;

创建临时表空间

create temporary tablespace user_temp tempfile '/u01/app/oracle/oradata/XE/xyrj_temp.dbf' size 50m autoextend on next 50m maxsize 20480m extent management local;

查看所有表空间

select tablespace_name from dba_tablespaces; select tablespace_name from user_tablespaces;

查询表空间中所有表的名称

select table_name from dba_all_tables where tablespace_name = tablespacename;

查看表空间的状态

select tablespace_name,status from dba_tablespaces;

**设置表空间的在线或读写状态**

表空间的状态属性主要有在线(online)，离线(offline)，只读(read only)和读写(read write)这四种，其中只读与读写状态属于在线状态的特殊情况，通过设置表空间的状态属性，我们可以对表空间的使用进行管理
在线：
当表空间的状态为online时，才允许访问该表空间中的数据
如果表空间不是online状态的，可以使用alter tablespace语句将其状态修改为online，语句如下
alter tablespace tablespace_name online;

离线：
当表空间的状态为offline时，不允许访问该表空间中的数据。例如向表空间中创建表或者读取表空间的表灯数据操作都将无法进行，这时可以对表空间进行脱机备份，也可以对应用程序进行升级和维护等。
如果表空间不是offline状态的，可以使用alter tablespace语句将其状态修改为offline，其语句如下：

alter tablespace tablespace_name offline parameter;

其中，parameter表示将表空间切换为offline状态时可以使用的参数，主要可以应用如下的几个参数：
normal
temporary
immediate
for recover

只读
当表空间的状态为read only时，虽然可以访问表空间的数据，但范文仅仅仅限于阅读，而不能进行任何的更新和删除操作，目的是为了保证表空间的数据安全。
如果表空间不是read only状态的，可以使用ater tablespace语句将其状态修改为read only，其语句的形式如下：
alter tablespace tablespace_name read only;

不过，将表空间的状态修改为read only之前，需要注意如下的事项：
1.表空间必须处于online状态
2.表空间不能包含任何事务的回退段
3.表空间不能正处于在线的数据库备份期间
读写
当表空间的状态为read write时，可以对表空间进行正常访问，包括对表空间中的数据进行查询，更新和操作。
如果表空间不是read write状态的，可以使用alter tablespace语句将其状态修改为read write，语句形式如下：

alter tablespace tablespace_name read write;

修改表空间的状态为read write，也需要保证表空间处于online状态