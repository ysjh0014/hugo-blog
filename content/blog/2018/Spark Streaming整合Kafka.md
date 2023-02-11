---
title: "Spark Streaming整合Kafka"
date: 2018-10-29 20:27:14
draft: false
---
前面的文章 [第一个Spark Streaming案例程序](https://blog.csdn.net/ys_230014/article/details/83510782) 中使用socket server来写入数据，可以很明显的看出使用每次打印出的结果都只是当前输入的，并不能累加之前发的数据一起计算，多个程序一起读取数据的话，也会出现重复读取数据的情况，而且如果机器出现故障，也不能知道数据读取到哪了，所以实际中使用肯定不会使用socket，这时候就要使用之前已经学习过的Kafka了，Kafka有很明显的优势，能够计算偏移量，将不同的数据写入到不同的主题中等等

1.首先导入Spark Streaming与Kafka整合的依赖：
<dependency> <groupId>org.apache.spark</groupId> <artifactId>spark-streaming-kafka-0-8_2.11</artifactId> <version>${spark.version}</version> </dependency>

这个依赖可以去官方文档中找，不同的Spark版本与Kafka整合的版本略微有些不同，例如我用的Spark2.1.0的版本：

![](https://img-blog.csdnimg.cn/20181029185111551.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==,size_16,color_FFFFFF,t_70)例

2.Spark Streaming整合Kafka的代码：
package cn.ysjh import org.apache.spark.SparkConf import org.apache.spark.streaming.dstream.{DStream, ReceiverInputDStream} import org.apache.spark.streaming.kafka.KafkaUtils import org.apache.spark.streaming.{Seconds, StreamingContext} object SparkStreamingKafka { def main(args: Array[String]): Unit = { val conf: SparkConf = new SparkConf().setAppName("SparkStreamingKafka").setMaster("local[4]") val streaming: StreamingContext = new StreamingContext(conf, Seconds(5)) val zkAddress = "cdh0:2181,cdh1:2181,cdh2:2181" val groupID="g1" val topic=Map[String,Int]("first"->1) //创建DStream，需要KafkaDStream val data: ReceiverInputDStream[(String, String)] = KafkaUtils.createStream(streaming,zkAddress,groupID,topic) //对数据进行处理，Kafka的ReceiverInputDStream[(String, String)]里面装的是一个元组(key是写入的key，value是实际写入的内容) val lines: DStream[String] = data.map(_._2) val word: DStream[String] = lines.flatMap(_.split(" ")) val words: DStream[(String, Int)] = word.map((_,1)) val result: DStream[(String, Int)] = words.reduceByKey(_+_) result.print() streaming.start() streaming.awaitTermination() } }

3.运行程序

4.在Kafka中启动一个生产者，如果在Kafka中没有first这个topic，先创建first这个topic
bin/kafka-console-producer.sh --broker-list cdh0:9092 --topic first

然后在生产者中输入数据，一空格隔开，就可以看出Spark Streaming实时计算出的结果了，到此Spark Streaming整合Kafka成功