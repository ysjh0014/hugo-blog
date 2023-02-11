---
title: "第一个Spark Streaming案例程序"
date: 2018-10-29 18:32:12
draft: false
---
前面的文章大概的介绍了Spark Streaing流式处理框架，说的通俗点，实际上就是在Spark Core的基础上进行了封装，然后将小批次的数据进行处理，处理完了进程并不会停止，而是会一直存在，这样只要有数据进来，就会进行处理，从而实现了流式处理

下面就来一个实例进行感受：

这里选择使用Linux下的nc工具来产生socket数据，然后Spark Streaming从这个socket server中读数据进行处理计算

1.首先放出Spark Streaing的代码：
package cn.ysjh import org.apache.spark.streaming.{Milliseconds, StreamingContext} import org.apache.spark.streaming.dstream.{DStream, ReceiverInputDStream} import org.apache.spark.{SparkConf, SparkContext} object SparkStreamingTest { def main(args: Array[String]): Unit = { //实时计算要创建StreamingContext，StreamingContext是对SparkContext的封装， val conf: SparkConf = new SparkConf().setAppName("SparkStreaming").setMaster("local[4]") val context: SparkContext = new SparkContext(conf) //第二个参数是小批次产生的时间间隔，Milliseconds是毫秒 val streaming: StreamingContext = new StreamingContext(context,Milliseconds(5000)) //有了SparkContext就可以创建SparkStreaming的抽象DStream //从一个socket端口中读取数据 val lines: ReceiverInputDStream[String] = streaming.socketTextStream("192.168.220.134",6789) //对DStream进行操作,你操作这个抽象(代理，描述)，就像操作一个本地集合一样 val words: DStream[String] = lines.flatMap(_.split(" ")) val wordOne: DStream[(String, Int)] = words.map((_,1)) val result: DStream[(String, Int)] = wordOne.reduceByKey(_+_) //打印结果 result.print() //启动Spark Streaming程序 streaming.start() //等待优雅的退出 streaming.awaitTermination() } }

2.在一台虚拟机中安装nc，在实际的系统中安装nc的命令不同

Ubuntu：
apt-get install netcat

Centos：

yum install nc

3.在IDEA中运行Spark Streaming程序

4.在安装了nc工具包的虚拟机上输入：
nc -lk 6789

然后随便输入数据，以空格隔开，就会在IDEA中看到运行的结果了，只要输入一条数据就会计算统计出一次结果，而且是每隔5秒输出一次

运行结果示例：

![](https://img-blog.csdnimg.cn/2018102918310410.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==,size_16,color_FFFFFF,t_70)

可以很明确的看出Spark Streaming就是一直在运行的Spark程序