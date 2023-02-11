---
title: "SparkSQL的JDBC数据源"
date: 2018-10-24 10:59:53
draft: false
---
通过JDBC连接到关系型数据库，然后可以读取表的信息以及表中的数据，既可以将结果查询展示出来，也可以将查询到的结果重新写入到数据库中

直接上代码：
package cn.ysjh0014.SparkSql import java.util.Properties import org.apache.spark.sql._ object SparkSqlJdbc { def main(args: Array[String]): Unit = { val session: SparkSession = SparkSession.builder().appName("SparkSqlJdbc").master("local[/*]").getOrCreate() import session.implicits._ val resource: DataFrame = session.read.format("jdbc").options( Map("url" -> "jdbc:mysql://localhost:3306/lianxi?serverTimezone=GMT%2B8", "driver" -> "com.mysql.jdbc.Driver", "dbtable" -> "table1", "user" -> "root", "password" -> "root" )).load() // resource.printSchema() //打印出表的表头信息 // resource.show() // val filterd: Dataset[Row] = resource.filter(r => { // r.getAs[Int](1) <= 15 // }) // // filterd.show() //lambda表达式 val r: Dataset[Row] = resource.filter($"age" <= 15) // val r: Dataset[Row] = resource.where($"age" <= 15) val s: DataFrame = r.select($"id",$"name",$"age") //将查询到的数据再写入到数据库中，ignore是如果表存在不做任何操作，如果不存在则创建表并写入数据，append是在表中追加数据，overwrite是写覆盖数据 val props = new Properties() props.put("user","root") props.put("password","root") s.write.mode("ignore").jdbc("jdbc:mysql://localhost:3306/bigdata?serverTimezone=GMT%2B8", "logs", props) // r.show() // s.show() session.close() } }

如上边的代码所示：

使用SparkSQL连接到Mysql数据库后既可以查询到表的表头信息，也可以查询到表中的数据，查询表中数据时可以使用lambda表达式，比较简洁，还可以将查询到的数据写入到数据库中

注意：

连接数据库时可能会报时区错误，这是因为数据库的时区不对，可以再jdbc中添加 ?serverTimezone=GMT%2B8 即可，例如我上边中的代码 "jdbc:mysql://localhost:3306/lianxi?serverTimezone=GMT%2B8"