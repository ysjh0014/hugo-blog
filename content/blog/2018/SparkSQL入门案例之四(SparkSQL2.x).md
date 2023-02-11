---
title: "SparkSQL入门案例之四(SparkSQL2.x)"
date: 2018-10-18 11:55:10
draft: false
---
前几个案例讲的都是都是SparkSQL1.x的编程，所以这里就讲SparkSQL2.x的编程

直接上代码，这里的代码是在前边案例的基础上的：
package cn.ysjh0014.SparkSql import org.apache.spark.SparkConf import org.apache.spark.rdd.RDD import org.apache.spark.sql.types._ import org.apache.spark.sql.{DataFrame, Dataset, Row, SparkSession} object SparkSqlTest1 { def main(args: Array[String]): Unit = { //SparkSQL2.x的编程API(SparkSession) //SparkSession是SparkSQL2.x的入口 val session: SparkSession = SparkSession.builder().appName("SqlTest1").master("local[4]").getOrCreate() //getOrCreate()是创建SparkSession的 //创建RDD val lines: RDD[String] = session.sparkContext.textFile(args(0)) //整理数据 val RowRdd: RDD[Row] = lines.map(line => { val fields = line.split(",") val id = fields(0).toLong val name = fields(1) val age = fields(2).toInt val yz = fields(3).toDouble Row(id, name, age, yz) }) //结果类型，其实就是表头，用于描述DataFrame val sm: StructType = StructType(List( StructField("id", LongType, true), StructField("name", StringType, true), StructField("age", IntegerType, true), StructField("yz", DoubleType, true) )) //将RowRDD关联到Schema val df: DataFrame = session.createDataFrame(RowRdd,sm) import session.implicits._ val df1: Dataset[Row] = df.where($"yz">98).orderBy($"age" desc) df1.show() session.stop() } }

可以清楚的看出，SparkSQL2.x是创建SparkSession，而1.x是创建SparkContext