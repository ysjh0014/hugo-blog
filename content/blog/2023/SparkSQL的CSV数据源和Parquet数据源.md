---
title: "SparkSQL的CSV数据源和Parquet数据源"
date: 2018-10-25 14:15:16
draft: false
---
1.CSV数据源
package cn.ysjh0014.SparkSql import org.apache.spark.sql.{DataFrame, Dataset, Row, SparkSession} object SparkSqlCsv { def main(args: Array[String]): Unit = { val session: SparkSession = SparkSession.builder().appName("JsonSource").master("local[4]").getOrCreate() import session.implicits._ //读取csv类型的数据 val csv: DataFrame = session.read.csv("D:\\测试数据\\test2\\part-00000-f967e149-5ac3-4f9b-ba2a-16f22c6c3496.csv") csv.show() session.stop() } }

运行结果：

![](https://img-blog.csdn.net/20181025121722869?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

可以看出CSV类型的文件只能读出有几列，并不能读出每列的列名信息，只能默认用_c0等代替，并且每列的数据类型也只能用识别为String

![](https://img-blog.csdn.net/20181025122022135?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

2.Parquet数据源(更加智能，可以提高程序的执行效率)
package cn.ysjh0014.SparkSql import org.apache.spark.sql.{DataFrame, Dataset, Row, SparkSession} object SparkSqlCsv { def main(args: Array[String]): Unit = { val session: SparkSession = SparkSession.builder().appName("JsonSource").master("local[4]").getOrCreate() import session.implicits._ //读取csv类型的数据 val parquet: DataFrame = session.read.parquet("D:\\测试数据\\test3\\part-00000-d1028b5e-9ca5-41ba-b221-ffc982712763.snappy.parquet") // val parquet: DataFrame = session.read.format("parquet").load("D:\\测试数据\\test3\\part-00000-d1028b5e-9ca5-41ba-b221-ffc982712763.snappy.parquet") // parquet.printSchema() parquet.show() session.stop() } }

Parquet文件在文件中直接打开你会发现全是数字，不是你原来保存的内容，这是他独特的保存方式，既保存了数据，又保存了Schema信息，保存了有哪些列，列的类型，偏移量信息，将相同的列的数据保存到l一起