---
title: "SparkSql入门案例之三(Spark1.x)"
date: 2018-10-18 11:26:03
draft: false
---
案例一和案例二中是将RDD转换成DataFrame的方法不同，但是在转换后都是使用SQL的方式来编程的，这里就用DataFrame API(DSL 特定领域编程语言)的方式来实现

直接上代码：
package cn.ysjh0014.SparkSql import org.apache.spark.rdd.RDD import org.apache.spark.sql.types._ import org.apache.spark.sql.{DataFrame, Dataset, Row, SQLContext} import org.apache.spark.{SparkConf, SparkContext} object SparkSqlDemo3 { def main(args: Array[String]): Unit = { //这个程序可以提交到Spark集群中 val conf = new SparkConf().setAppName("SparkSql3").setMaster("local[4]") //这里的setMaster是为了在本地运行，多线程运行 //创建Spark Sql的连接 val sc = new SparkContext(conf) //SparkContext不能创建特殊的RDD，将Spark Sql包装进而增强 val SqlContext = new SQLContext(sc) //创建DataFrame(特殊的RDD，就是有schema的RDD)，先创建一个普通的RDD，然后再关联上schema val lines = sc.textFile(args(0)) //将数据进行处理 val RowRdd: RDD[Row] = lines.map(line => { val fields = line.split(",") val id = fields(0).toLong val name = fields(1) val age = fields(2).toInt val yz = fields(3).toDouble Row(id,name,age,yz) }) //结果类型，其实就是表头，用于描述DataFrame val sm: StructType = StructType(List( StructField("id", LongType, true), StructField("name", StringType, true), StructField("age", IntegerType, true), StructField("yz", DoubleType, true) )) //将RowRDD关联schema val df: DataFrame = SqlContext.createDataFrame(RowRdd,sm) //使用DataFrame API的方式 //不使用SQL的方式就不用注册临时表了 val df1: DataFrame = df.select("id","name","age","yz") //导入隐式转换 import SqlContext.implicits._ val df2: Dataset[Row] = df.orderBy($"age" desc,$"yz" asc) //这里的desc和asc是方法所以必须小写 df1.show() df2.show() //释放资源 sc.stop() } }

运行结果：

![](https://img-blog.csdn.net/20181018112529920?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

![](https://img-blog.csdn.net/20181018112543589?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)