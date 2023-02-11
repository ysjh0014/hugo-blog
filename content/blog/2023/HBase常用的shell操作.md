---
title: "HBase常用的shell操作"
date: 2018-11-05 21:32:52
draft: false
---
**1.status**

显示服务器状态
hbase>status 'cdh0'

**2.whoami**

显示HBase当前用户
hbase>whoami

**3.count**

统计指定表的记录数
hbase>count 'test'

**4.describe**

展示表结构信息
hbase>describe 'test'

**5.exist**

检查表是否存在，适用于表量特别多的情况
hbase>exist 'test'

**6. is_enabled/is_disabled**

检查表是否启用或禁用
hbase>is_enabled 'test' hbase>is_disabled 'test'

**7.alter**

该命令可以改变表和列族的模式

为当前表增加列族：
hbase> alter 'test', NAME => 'CF2', VERSIONS => 2

为当前表删除列族：

hbase> alter 'test', 'delete' => ’CF2’

**8.disable**

禁用一张表
hbase>disable 'test'

**9.drop**

删除一张表，记得在删除表之前必须先禁用
hbase>drop 'test'

**10.delete**

删除一行中一个单元格的值
hbase> delete 'test', 'rowKey, 'CF:C'

**11.truncate**

清空表数据，即禁用表-删除表-创建表
hbase>truncate 'test'