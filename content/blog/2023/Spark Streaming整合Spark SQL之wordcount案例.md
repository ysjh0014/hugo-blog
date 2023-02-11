---
title: "Spark Streaming整合Spark SQL之wordcount案例"
date: 2018-11-02 19:00:21
draft: false
---
完整源代码地址： [https://github.com/apache/spark/blob/v2.3.2/examples/src/main/scala/org/apache/spark/examples/streaming/SqlNetworkWordCount.scala](https://github.com/apache/spark/blob/v2.3.2/examples/src/main/scala/org/apache/spark/examples/streaming/SqlNetworkWordCount.scala)

案例源码：
package cn.ysjh import org.apache.spark.SparkConf import org.apache.spark.rdd.RDD import org.apache.spark.sql.SparkSession import org.apache.spark.streaming.{Seconds, StreamingContext, Time} object SparkStreamingSql { def main(args: Array[String]): Unit = { val cf: SparkConf = new SparkConf().setAppName("SparkStreamingSql").setMaster("local[2]") val streaming: StreamingContext = new StreamingContext(cf, Seconds(5)) val lines = streaming.socketTextStream("192.168.220.134", 6789) val words = lines.flatMap(_.split(" ")) // 将单词DStream的RDD转换为DataFrame并运行SQL查询 words.foreachRDD { (rdd: RDD[String], time: Time) => // 获取SparkSession的单例实例 val spark = SparkSessionSingleton.getInstance(rdd.sparkContext.getConf) import spark.implicits._ //将RDD [String]转换为RDD [case class]到DataFrame val wordsDataFrame = rdd.map(w => Record(w)).toDF() // 使用DataFrame创建临时视图 wordsDataFrame.createOrReplaceTempView("words") // 使用SQL对表进行单词计数并打印它 val wordCountsDataFrame = spark.sql("select word, count(/*) as total from words group by word") println(s"========= $time =========") wordCountsDataFrame.show() } streaming.start() streaming.awaitTermination() } // 将RDD转换为DataFrame的案例类 case class Record(word: String) // 实例化SparkSession的单例实例 object SparkSessionSingleton { @transient private var instance: SparkSession = _ def getInstance(sparkConf: SparkConf): SparkSession = { if (instance == null) { instance = SparkSession .builder .config(sparkConf) .getOrCreate() } instance } } }

可以看出将Spark Streaming中接收到的数据创建成表，然后使用Spark SQL来进行一系列的操作，在实际生产中使用的非常多

运行截图：

![](https://img-blog.csdnimg.cn/20181102185919717.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==,size_16,color_FFFFFF,t_70)

这里仍然使用netcat来产生socket数据进行测试