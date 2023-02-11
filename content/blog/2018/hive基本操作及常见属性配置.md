---
title: "hive基本操作及常见属性配置"
date: 2018-07-12 14:58:54
draft: false
---
前面已经对hive进行了简单的讲解，包括如何在hive中创建一张表，并向表中加载数据，下面说一些hive的其它基本操作

下边的讲解中的数据库和表为db_hive和student

基本操作:

1.查看表的信息

用desc和select能够查看表的字段和字段下边的数据，这个和sql语句中的一样

查看表的具体信息:

desc formatted db_hive.student;

![](https://img-blog.csdn.net/20180712143248152?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

可以看出有所属的数据库，表的字段，创建时间，所有者，位置等等具体信息

2.查看hive自带的函数:

show functions;

desc function upper; 查看upper函数如何使用

desc function extended upper; 查看upper函数使用示例

![](https://img-blog.csdn.net/20180712143707749?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

属性配置:

1.在命令行上显示当前数据库和表的行头信息

当你使用hive进行查询操作时，它只会显示字段下面的数据，不会显示该字段，并且也不会显示你正在使用的是哪个数据库，这样很容易出错，看起来也不方便，所以一般都会进行配置文件的修改，让这些都显示出来

在conf/hive-site.xml中添加以下配置:

<property>
<name>hive.cli.print.header</name>
<value>true</value>

</property>

<property>
<name>hive.cli.print.current.db</name>
<value>true</value>

</property>

注意:可能细心的会发现我上边的所有语句都数据库加表名进行使用的，如果不使用，则你需要指定数据库，即use+数据库名，不然会很容易出错，毕竟不同数据库的表名也可以相同

2.hive数据仓库位置配置

默认: hdfs中的/user/hive/warehouse

注意: /*在仓库目录下，没有对默认的default数据库常见文件夹

/*如果某张表属于default数据库，直接在数据仓库目录下创建一个文件夹，