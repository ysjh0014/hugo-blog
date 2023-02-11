---
title: "Sqoop常用命令及参数二"
date: 2018-09-13 17:47:12
draft: false
---
4.create-hive-table

生成与关系数据库表结构对应的 hive 表结构
--hive-home <dir> Hive 的安装目录,可以通过该参数覆盖掉默认的 Hive 目
录 --hive-overwrite 覆盖掉在 Hive 表中已经存在的数据 --create-hive-table 默认是 false,如果目标表已经存在了,那么创建任务会失
败 --hive-table 后面接要创建的 hive 表 --table 指定关系数据库的表名

5.eval

可以快速的使用 SQL 语句对关系型数据库进行操作,经常用于在 import 数据之前,了解一下 SQL 语句是否正确,数据是否正常,并可以将结果显示在控制台
--query 或--e 后跟查询的 SQL 语句

6.import-all-tables

可以将 RDBMS 中的所有表导入到 HDFS 中,每一个表都对应一个 HDFS 目录
--as-avrodatafile 这些参数的含义均和 import对应的含义一致 --as-sequencefile --as-textfile --direct --direct-split-size <n> --inline-lob-limit <n> --m 或—num-mappers <n> --warehouse-dir <dir> -z 或--compress --compression-codec

7.job

用来生成一个 sqoop 任务,生成后不会立即执行,需要手动执行
--create <job-id> 创建 job 参数 --delete <job-id> 删除一个 job --exec <job-id> 执行一个 job --help 显示 job 帮助 --list 显示 job 列表 --meta-connect <jdbc-uri> 用来连接 metastore 服务 --show <job-id> 显示一个 job 的信息 --verbose 打印命令运行时的详细信息

8.metastore

记录了 Sqoop job 的元数据信息,如果不启动该服务,那么默认 job 元数据的存储目录为
~/.sqoop,可在 sqoop-site.xml 中修改
--shutdown 关闭 metastore