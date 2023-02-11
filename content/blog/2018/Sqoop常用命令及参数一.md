---
title: "Sqoop常用命令及参数一"
date: 2018-09-13 17:39:41
draft: false
---
**常用命令列举**
import 将数据导入到集群 export 将集群数据导出 codegen 获取数据库中某张表数据生成 Java 并打包Jar create-hive-table 创建 Hive 表 eval 查看 SQL 执行结果 import-all-tables 导入某个数据库下所有表到 HDFS 中 job 用来生成一个 sqoop的任务,生成后,该任务并不执行,除非使用命令执行该任务 list-databases 列出所有数据库名 list-tables 列出某个数据库下所有表 merge 将 HDFS 中不同目录下面的数据合在一起,并存放在指定的目录中 metastore 记录 sqoop job 的元数据信息,如果不启动 metastore 实例则默认的元数据存储目录为:~/.sqoop,如果要更改存储目录可以 在 配 置 文 件sqoop-site.xml 中进行更改 help 帮助信息 version 打印 sqoop 版本信息

**公用参数：**

1.数据库连接
--connect 连接关系型数据库的 URL --connection-manager 指定要使用的连接管理类 --driver JDBC 的 driver class --help 打印帮助信息 --password 连接数据库的密码 --username 连接数据库的用户名 --verbose 在控制台打印出详细信息

2.import

--enclosed-by <char> 给字段值前后加上指定的字符 --escaped-by <char> 对字段中的双引号加转义符 --fields-terminated-by <char> 设定每个字段是以什么符号作为结束,默认为逗号 --lines-terminated-by <char> 设定每行记录之间的分隔符,默认是\n --mysql-delimiters Mysql 默认的分隔符设置,字段之间以逗号分隔,行之间以\n 分隔,默认转义符是\,字段值以单引号包裹 --optionally-enclosed-by <char> 给带有双引号或单引号的字段值前后加上指定字符

3.export

--input-enclosed-by <char> 对字段值前后加上指定字符 --input-escaped-by <char> 对含有转移符的字段做转义处理 --input-fields-terminated-by <char> 字段之间的分隔符 --input-lines-terminated-by <char> 行之间的分隔符 --input-optionally-enclosed-by <char> 给带有双引号或单引号的字段前后加上指定字符

4.hive

--hive-delims-replacement <arg> 用自定义的字符串替换掉数据中的\r\n 和\013 \010 等字符 --hive-drop-import-delims 在导入数据到 hive 时,去掉数据中的\r\n\013\010 这样的字符 --map-column-hive <map> 生成 hive 表时,可以更改生成字段的数据类型 --hive-partition-key 创建分区,后面直接跟分区名,分区字段的默认类型为string --hive-partition-value <v> 导入数据时,指定某个分区的值 --hive-home <dir> hive 的安装目录,可以通过该参数覆盖之前默认配置的目录 --hive-import 将数据从关系数据库中导入到 hive 表中 --hive-overwrite 覆盖掉在 hive 表中已经存在的数据 --create-hive-table 默认是 false,即,如果目标表已经存在了,那么创建任务失败 --hive-table 后面接要创建的 hive 表,默认使用 MySQL 的表名 --table 指定关系数据库的表名

命令参数：

1.import
--append 将数据追加到 HDFS 中已经存在的 DataSet 中,如果使用该参数,sqoop 会把数据先导入到临时文件目录,再合并 --as-avrodatafile 将数据导入到一个 Avro 数据文件中 --as-sequencefile 将数据导入到一个 sequence文件中 --as-textfile 将数据导入到一个普通文本文件中 --boundary-query <statement> 边界查询,导入的数据为该参数的值(一条 sql 语句)所执行的结果区间内的数据 --columns <col1, col2, col3> 指定要导入的字段 --direct 直接导入模式,使用的是关系数据库自带的导入导出工具,以便加快导入导出过程 --direct-split-size 在使用上面 direct 直接导入的基础上,对导入的流按字节分块,即达到该阈值就产生一个新的文件 --inline-lob-limit 设定大对象数据类型的最大值 --m 或–num-mappers 启动 N 个 map 来并行导入数据,默认 4 个 --query 或--e <statement> 将查询结果的数据导入,使用时必须伴随参--target-dir,--hive-table,如果查询中有where 条件,则条件后必须加上$CONDITIONS 关键字 --split-by <column-name> 按照某一列来切分表的工作单元,不能与--autoreset-to-one-mapper 连用(请参考官方文档) --table <table-name> 关系数据库的表名 --target-dir <dir> 指定 HDFS 路径 --warehouse-dir <dir> 与 14 参数不能同时使用,导入数据到 HDFS 时指定的目录 --where 从关系数据库导入数据时的查询条件 --z 或--compress 允许压缩 --compression-codec 指定 hadoop 压缩编码类,默认为 gzip(Use Hadoop codecdefault gzip) --null-string <null-string> string 类型的列如果 null,替换为指定字符串 --null-non-string <null-string> 非 string 类型的列如果 null,替换为指定字符串 --check-column <col> 作为增量导入判断的列名 --incremental <mode> mode:append 或 lastmodified --last-value <value> 指定某一个值,用于标记增量导入的位置

2.export

--direct 利用数据库自带的导入导出工具,以便于提高效率 --export-dir <dir> 存放数据的 HDFS 的源目录 -m 或--num-mappers <n> 启动 N 个 map 来并行导入数据,默认 4 个 --table <table-name> 指定导出到哪个 RDBMS 中的表 --update-key <col-name> 对某一列的字段进行更新操作 --update-mode <mode> updateonly
allowinsert(默认) --input-null-string <null-string> 参考 import --input-null-non-string <null-string> 参考 import --staging-table <staging-table-name> 创建一张临时表,用于存放所有事务的结果,然后将所有事务结果一次性导入到目标表中,防止错误 --clear-staging-table 如果第 9 个参数非空,则可以在导出操作执行前,清空临时事务结果表

3.codegen

将关系型数据库中的表映射为一个 Java 类,在该类中有各列对应的各个字段
--bindir <dir> 指定生成的 Java 文件、编译成的 class 文件及将生成文件打包为 jar 的文件输出路径 --class-name <name> 设定生成的 Java 文件指定的名称 --outdir <dir> 生成 Java 文件存放的路径 --package-name <name> 包名,如 com.z,就会生成 com和 z 两级目录 --input-null-non-string <null-str> 在生成的 Java 文件中,可以将 null 字符或者不存在的字符串设置为想要设定的值(例如空字符串) --input-null-string <null-str> 将 null 字符串替换成想要替换的值(一般与 5 同时使用) --map-column-java <arg> 数据库字段在生成的 Java 文件中会映射成各种属性,且默认的数据类型与数据库类型保持对应关系。该参数可以改变默认类型,例如:--map-column-java id=long,
name=String --null-non-string <null-str> 在生成 Java 文件时,可以将不存在或者 null 的字符串设置为其他值 --null-string <null-str> 在生成 Java 文件时,将 null字符串设置为其他值(一般与8 同时使用) --table <table-name> 对应关系数据库中的表名,生成的 Java 文件中的各个属性与该表的各个字段一一对应