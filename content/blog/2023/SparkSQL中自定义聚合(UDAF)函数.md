---
title: "SparkSQL中自定义聚合(UDAF)函数"
date: 2018-10-23 20:29:05
draft: false
---
在学习Hive的时候我们已经了解到当内置函数无法满足你的业务处理需要时，此时就可以考虑使用用户自定义函数(UDF:user defined function)

用户自定义函数类别分为以下三种：

1).UDF：输入一行，返回一个结果(一对一)，在上篇案例 [使用SparkSQL实现根据ip地址计算归属地二](https://blog.csdn.net/ys_230014/article/details/83210636) 中实现的自定义函数就是UDF，输入一个十进制的ip地址，返回一个省份

2).UDTF：输入一行，返回多行(一对多)，在SparkSQL中没有，因为Spark中使用flatMap即可实现这个功能

3).UDAF：输入多行，返回一行，这里的A是aggregate，聚合的意思，如果业务复杂，需要自己实现聚合函数

下面就来介绍如何自定义UDAF聚合函数

以一个实际案例来介绍，这个案例是求几何平均数的，几何平均数不知道的可以去百度百科看，简单来说就是求n个数乘积的开n次方，但是如果这里的n很大，在单机上根本就运算不了怎么办，我们可以在Spark集群上执行这个任务

思路：

![](https://img-blog.csdn.net/20181022185645296?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

如图所示： 在集群的机器的分区内执行计算出各自的n和t，然后汇总到一起再执行Math.pow来计算几何平均数

具体代码实现：
package cn.ysjh0014.SparkSql import java.lang import org.apache.spark.sql.expressions.{MutableAggregationBuffer, UserDefinedAggregateFunction} import org.apache.spark.sql.types._ import org.apache.spark.sql.{Dataset, Row, SparkSession} object UDAFTest { def main(args: Array[String]): Unit = { val session: SparkSession = SparkSession.builder().appName("UDAFTest").master("local[/*]").getOrCreate() val udaf = new UDAFys //注册函数 // session.udf.register("udaf",udaf) val range: Dataset[lang.Long] = session.range(1, 11) // range.createTempView("table") // val df = session.sql("SELECT udaf(id) result FROM table") import session.implicits._ val df = range.agg(udaf($"id").as("geomean")) df.show() session.stop() } } class UDAFys extends UserDefinedAggregateFunction { //输入数据的类型 override def inputSchema: StructType = StructType(List( StructField("value", DoubleType) )) //产生中间结果的数据类型 override def bufferSchema: StructType = StructType(List( //相乘之后返回的积 StructField("project", DoubleType), //参与运算数字的个数 StructField("Num", LongType) )) //最终返回的结果类型 override def dataType: DataType = DoubleType //确保一致性，一般用true override def deterministic: Boolean = true //指定初始值 override def initialize(buffer: MutableAggregationBuffer): Unit = { //相乘的初始值，这里的要和上边的中间结果的类型和位置相对应 buffer(0) = 1.0 //参与运算数字个数的初始值 buffer(1) = 0L } //每有一条数据参与运算就更新一下中间结果(update相当于在每一个分区中的计算) override def update(buffer: MutableAggregationBuffer, input: Row): Unit = { //每有一个数字参与运算就进行相乘(包含中间结果) buffer(0) = buffer.getDouble(0) /* input.getDouble(0) //参与运算的数字个数更新 buffer(1) = buffer.getLong(1) + 1L } //全局聚合 override def merge(buffer1: MutableAggregationBuffer, buffer2: Row): Unit = { //每个分区计算的结果进行相乘 buffer1(0) = buffer1.getDouble(0) /* buffer2.getDouble(0) buffer1(1) = buffer1.getLong(1) + buffer2.getLong(1) } //计算最终的结果 override def evaluate(buffer: Row): Any = { math.pow(buffer.getDouble(0), 1.toDouble / buffer.getLong(1)) } }

运行结果：

![](https://img-blog.csdn.net/20181023202612552?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

上边的代码中也用了两种方式来实现SparkSQL，sql方式和DataFrame API的方式