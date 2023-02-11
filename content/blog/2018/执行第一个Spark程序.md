---
title: "执行第一个Spark程序"
date: 2018-10-10 20:22:01
draft: false
---
我们这里使用官方的Spark自带的一个程序来体验一下Spark的运行

Spark自带的例子是利用蒙特·卡罗算法求PI

在Spark目录下执行下面命令
bin/spark-submit \ >--master spark://cdh0:7077 \ >--class org.apache.spark.examples.SparkPi \ examples/jars/spark-examples_2.11-2.1.0.jar 100

说明：

bin/spark-submit是提交Spark程序的命令， --master spark://cdh0:7077 是指定Master在哪台机器上，--class org.apache.spark.examples.SparkPi指定包名，Spark运行程序jar包的所在目录，100是指采样次数

注意： 这里的--master可以指定多个Master，保证高可用，例如：spark://cdh0:7077,cdh1:7077

运行结果：

![](https://img-blog.csdn.net/20181010195519703?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

当然在WEBUI界面中也可以查看到：

![](https://img-blog.csdn.net/20181010195623113?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

说明：

Cores指使用核数(线程数)，这里默认使用所有的核数，Memory per Node指使用的内存，默认是用1G，这里在实际生产环境中都是要自己进行指定的，在提交Spark程序时加入以下命令：
--executor-memory 1G \ --每个executor使用的内存大小 --total-executor-cores 2 \ --整个程序使用的核数

注意： 这里--executor-memory不仅支持G，还支持mb

这里的Spark程序运行时间比较短，如果你扩大采样次数，你就会发现在运行Spark任务的当前机器上多了两个进程，而其他Worker节点机器上也多了一个进程

![](https://img-blog.csdn.net/201810102008502?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

![](https://img-blog.csdn.net/20181010200904997?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

SparkSubmit进程是提交Spark程序的，CoarseGrainedExecutorBackend进程是用来计算的，这两个进程都是Spark程序运行完之后就会被销毁的，以此来释放资源