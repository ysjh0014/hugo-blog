---
title: "Sqoop中脚本打包"
date: 2018-09-13 16:51:58
draft: false
---
1.创建一个.opt文件
touch test.opt;

2.编写Sqoop脚本

vi test.opt;

将下面代码写入脚本文件中

export \ --connect jdbc:mysql://172.17.0.6:3306/ys \ --username root \ --password root \ --table test \ --num-mappers 1 \ --export-dir /user/hive/warehouse/staff_hive \ --input-fields-terminated-by "\t"

3.运行脚本

bin/sqoop --options-file 脚本所在目录