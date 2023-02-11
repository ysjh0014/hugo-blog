---
title: "SparkSQL写入多种文件格式"
date: 2018-10-24 21:11:27
draft: false
---
需求：

将数据库中的数据读取出来并以text json csv parquet四种格式写入到本地文件或者hdfs中

csv格式：能够以excel的形式打开

代码实现：
package cn.ysjh0014.SparkSql import java.util.Properties import org.apache.spark.sql._ object SparkSqlJdbc { def main(args: Array[String]): Unit = { val session: SparkSession = SparkSession.builder().appName("SparkSqlJdbc").master("local[1]").getOrCreate() import session.implicits._ val resource: DataFrame = session.read.format("jdbc").options( Map("url" -> "jdbc:mysql://localhost:3306/lianxi?serverTimezone=GMT%2B8", "driver" -> "com.mysql.jdbc.Driver", "dbtable" -> "table1", "user" -> "root", "password" -> "root" )).load() //lambda表达式 val r: Dataset[Row] = resource.where($"age" <= 15) val s: DataFrame = r.select($"id",$"name",$"age") val s: DataFrame = r.select($"name") //将查询到的数据再写入到数据库中 // val props = new Properties() // props.put("user","root") // props.put("password","root") // s.write.mode("append").jdbc("jdbc:mysql://localhost:3306/bigdata?serverTimezone=GMT%2B8", "logs", props) s.write.text("D:\\测试数据\\text1") r.write.json("D:\\测试数据\\test") r.write.csv("D:\\测试数据\\test2") r.write.parquet("D:\\测试数据\\test3") session.close() } }

注意：

以text格式写入的时候只能写入一列，并且是String类型的，否则会报错，因为写入的时候不仅写入的是数据，还会将schema信息写入，所以text格式的不能写入多列，写入一列默认是value，