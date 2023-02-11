---
title: "SparkSQL入门案例之二(SparkSQL1.x)"
date: 2018-10-17 20:01:52
draft: false
---
**SparkSQL入门案例一中的思路主要是：**

1).创建SparkContext
2).创建SQLContext
3).创建RDD
4).创建一个类，并定义类的成员变量
5).整理数据并关联class
6).将RDD转换成DataFrame（导入隐式转换）
7).将DataFrame注册成临时表
8).书写SQL（Transformation）
9).执行Action

**还有另外一种思路和写法：**

1).创建SparkContext
2).创建SQLContext
3).创建RDD
4).创建StructType（schema）
5).整理数据将数据跟Row关联
6).通过rowRDD和schema创建DataFrame
7).将DataFrame注册成临时表
8).书写SQL（Transformation）
9).执行Action

这两种方法主要是将RDD变成DataFrame的方式不同

**具体代码实现流程：**
package cn.ysjh0014.SparkSql import org.apache.spark.rdd.RDD import org.apache.spark.sql.types._ import org.apache.spark.sql.{DataFrame, Row, SQLContext} import org.apache.spark.{SparkConf, SparkContext} object SparkSqlDemo2 { def main(args: Array[String]): Unit = { //这个程序可以提交到Spark集群中 val conf = new SparkConf().setAppName("SparkSql2").setMaster("local[4]") //这里的setMaster是为了在本地运行，多线程运行 //创建Spark Sql的连接 val sc = new SparkContext(conf) //SparkContext不能创建特殊的RDD，将Spark Sql包装进而增强 val SqlContext = new SQLContext(sc) //创建DataFrame(特殊的RDD，就是有schema的RDD)，先创建一个普通的RDD，然后再关联上schema val lines = sc.textFile(args(0)) //将数据进行处理 val RowRdd: RDD[Row] = lines.map(line => { val fields = line.split(",") val id = fields(0).toLong val name = fields(1) val age = fields(2).toInt val yz = fields(3).toDouble Row(id,name,age,yz) }) //结果类型，其实就是表头，用于描述DataFrame val sm: StructType = StructType(List( StructField("id", LongType, true), StructField("name", StringType, true), StructField("age", IntegerType, true), StructField("yz", DoubleType, true) )) //将RowRDD关联schema val df: DataFrame = SqlContext.createDataFrame(RowRdd,sm) //变成DataFrame后就可以使用两种API进行编程了 //使用SQL的方式 //把DataFrame注册成临时表 df.registerTempTable("body") //过时的方法 //书写SQL(sql方法其实是Transformation) val result: DataFrame = SqlContext.sql("SELECT /* FROM body ORDER BY yz desc, age asc") //注意： 这里的SQL语句该大写的必须大写 //查看结果(出发Action) result.show() //释放资源 sc.stop() } }

运行结果和案例一中的一模一样