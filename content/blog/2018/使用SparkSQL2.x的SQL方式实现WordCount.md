---
title: "使用SparkSQL2.x的SQL方式实现WordCount"
date: 2018-10-18 12:19:55
draft: false
---
代码里面有很详细的说明

代码实现：
package cn.ysjh0014.SparkSql import org.apache.spark.sql.{DataFrame, Dataset, SparkSession} object SparkSQLWordCount { def main(args: Array[String]): Unit = { //创建SparkSession val session: SparkSession = SparkSession.builder().appName("SQLWordCount").master("local[4]").getOrCreate() //读数据，是lazy //Dataset也是一个分布式数据集，是对RDD的进一步分装 //Dataset只有一列，默认这列叫value val lines: Dataset[String] = session.read.textFile(args(0)) //导入隐式转换 import session.implicits._ val word: Dataset[String] = lines.flatMap(_.split(",")) //注册表 word.createTempView("test") //执行SQL val result: DataFrame = session.sql("SELECT value,COUNT(/*) counts FROM test GROUP BY value ORDER BY counts DESC") result.show() session.stop() } }

运行后你会发现他的速度会变慢，这是因为他会生成执行计划，然后再运行计算