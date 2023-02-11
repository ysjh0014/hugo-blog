---
title: "在Spark Shell中编写WordCount程序"
date: 2018-10-11 12:32:19
draft: false
---
Spark Shell是一个交互式的命令行，里面可以写Spark程序(Scala语言)，也是一个客户端，用于提交Spark程序

**1.启动Spark Shell**
bin/spark-shell

![](https://img-blog.csdn.net/20181010204310978?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

上边是没有指定Master地址的启动方式，启动后用的是spark的local模式运行的，是模拟了spark集群运行的过程
bin/spark-shell --master spark://cdh0:7077,cdh1:7077

上边是指定了Master地址的启动方式，会将任务提交到集群，这时候使用jps查看，可以看到机器上的SparkSubmit和CoarseGrainedExecutorBackend进程都已经存在了，SparkSubmit会连接Master，并申请计算资源，然后Master进行资源调度(让Worker来启动Executor)

**2.向hdfs中上传一个用来测试的数据文件**

例如： test.txt
hdfs yarn hadoop hdfs yarn mapreduce hadoop yarn hdfs mapreduce

然后上传到hdfs中

**3.在Spark Shell中编写WordCount程序**

在Spark Shell中使用Scala编写Spark程序
sc.textFile("hdfs://cdh0:8020/usr/ys/input/test.txt").flatMap(_.split(" ")).map((_,1)).reduceByKey(_+_).saveAsTextFile("hdfs://cdh0:8020/usr/output")

参数说明：

sc是SparkContext对象，该对象是提交spark程序的入口

textFile("hdfs://cdh0:8020/usr/ys/input/test.txt")是向hdfs中读取数据

flatMap(_.split(" "))是先map后进行扁平化操作

map((_,1))是将单词和1构成元组

reduceByKey(_+_)是按照key进行reduce，并将value累加

saveAsTextFile("hdfs://cdh0:8020/usr/output2")是保存到hdfs的目录中

**4.在hdfs中查看结果**

bin/hdfs dfs -cat /usr/output//*

![](https://img-blog.csdn.net/20181011123054782?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)