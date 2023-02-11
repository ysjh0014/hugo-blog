---
title: "Hbase使用Java API测试表是否存在"
date: 2018-09-27 19:38:53
draft: false
---
**1.创建Maven工程，添加pom.xml和配置文件**

pom.xml
<dependencies> <dependency> <groupId>org.apache.hbase</groupId> <artifactId>hbase-server</artifactId> <version>1.3.1</version> </dependency> <dependency> <groupId>org.apache.hbase</groupId> <artifactId>hbase-client</artifactId> <version>1.3.1</version> </dependency> </dependencies>

配置文件： 因为Hbase要依赖于Hadoop和Zookeeper，所以需要相应的配置文件

Hadoop中的core-site.xml，hdfs-site.xml，log4j.properties

Hbase中的hbase-site.xml

这些配置文件在集群中都可以找到，然后复制到项目的resources目录下，项目结构如下图所示

![](https://img-blog.csdn.net/20180927193340876?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

**2.测试表是否存在代码**
package cn.ysjh; import java.io.IOException; import org.apache.hadoop.conf.Configuration; import org.apache.hadoop.hbase.HBaseConfiguration; import org.apache.hadoop.hbase.TableName; import org.apache.hadoop.hbase.client.Admin; import org.apache.hadoop.hbase.client.Connection; import org.apache.hadoop.hbase.client.ConnectionFactory; import org.apache.hadoop.hbase.client.HBaseAdmin; public class HbaseTest { public static Configuration conf; static{ //使用 HBaseConfiguration 的单例方法实例化 conf = HBaseConfiguration.create(); } //判断表是否存在 public static boolean isTableExist(String tableName) throws IOException { //在 HBase 中管理、访问表需要先创建 HBaseAdmin 对象 Connection connection = ConnectionFactory.createConnection(conf); Admin admin=connection.getAdmin(); return admin.tableExists(TableName.valueOf(tableName)); } public static void main(String[] args) throws IOException { System.out.println(isTableExist("aaa")); } }

然后开启Hbase集群，进行测试

注意： 这里直接运行代码会报不能解析主机名的错，应该在宿主机的hosts文件中加上集群机器的ip和对应的主机名，这样才能解析出集群的主机名