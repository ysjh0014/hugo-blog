---
title: "SparkSQL的Json数据源"
date: 2018-10-24 22:03:44
draft: false
---
SparkSql可以读取Json类型的文件

代码示例：
package cn.ysjh0014.SparkSql import org.apache.spark.sql.{DataFrame, Dataset, Row, SparkSession} object SparkSqlJson { def main(args: Array[String]): Unit = { val session: SparkSession = SparkSession.builder().appName("JsonSource").master("local[4]").getOrCreate() import session.implicits._ //读取json类型的数据 val json: DataFrame = session.read.json("D:\\test") val result: Dataset[Row] = json.where($"age"<=15) result.show() session.stop() } }

运行结果：

![](https://img-blog.csdn.net/20181024220037860?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

可以看出，读取Json类型的数据可以读取到Schema信息，是将Json数据中的属性值转化成的，但是当你在Json文件中的某一列中添加一个新的属性值时，就不能读取成功，会报错，这是因为Json数据与检验和不一致，将文件目录中的Json检验和文件删除，重新运行，就可以重新读取到该数据，并且新添加的属性值也会显示出来，Json数据源的Schema信息只能显示出少数的几种数据类型