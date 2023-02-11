---
title: "SparkSQL入门案例之一(SparkSQL1.x)"
date: 2018-10-17 19:10:33
draft: false
---
SparkSQL 1.x和2.x的编程API有一些变化，企业中都有使用，所以这里两种方式都将使用案例进行学习

先使用SparkSQL1.x的案例

开发环境跟之前开发SparkCore程序的一样，IDEA+Maven+Scala

**1.导入SparkSQL的pom依赖**

在之前的博文 Spark案例之根据ip地址计算归属地中的pom依赖中加上下面的依赖即可
<dependency> <groupId>org.apache.spark</groupId> <artifactId>spark-sql_2.11</artifactId> <version>${spark.version}</version> </dependency>

**2.具体代码实现，因为代码中已经有很详细的说明了，所以这里就直接放出来了**

package cn.ysjh0014.SparkSql import org.apache.spark.rdd.RDD import org.apache.spark.sql.{DataFrame, SQLContext} import org.apache.spark.{SparkConf, SparkContext} object SparkSqlDemo1 { def main(args: Array[String]): Unit = { //这个程序可以提交到Spark集群中 val conf = new SparkConf().setAppName("SparkSql").setMaster("local[4]") //这里的setMaster是为了在本地运行，多线程运行 //创建Spark Sql的连接 val sc = new SparkContext(conf) //SparkContext不能创建特殊的RDD，将Spark Sql包装进而增强 val SqlContext = new SQLContext(sc) //创建DataFrame(特殊的RDD，就是有schema的RDD)，先创建一个普通的RDD，然后再关联上schema val lines = sc.textFile(args(0)) //将数据进行处理 val boyRdd: RDD[Boy] = lines.map(line => { val fields = line.split(",") val id = fields(0).toLong val name = fields(1) val age = fields(2).toInt val yz = fields(3).toDouble Boy(id,name,age,yz) }) //该RDD装的是Boy类型的数据，有了schema信息，但是还是一个RDD，所以应该将RDD转换成DataFrame //导入隐式转换 import SqlContext.implicits._ val df: DataFrame = boyRdd.toDF //变成DataFrame后就可以使用两种API进行编程了 //1.使用SQL的方式 //把DataFrame注册成临时表 df.registerTempTable("body") //过时的方法 //书写SQL(sql方法其实是Transformation) val result: DataFrame = SqlContext.sql("SELECT /* FROM body ORDER BY yz desc, age asc") //注意： 这里的SQL语句该大写的必须大写 //查看结果(出发Action) result.show() //释放资源 sc.stop() } } case class Boy(id: Long, name: String, age: Int, yz: Double) //样例类

**3.创建测试数据**

创建一个txt文件，然后在里面输入以下数据
1,zhangsan,15,99 2,lisi,16,98 3,wangwu,20,100 4,xiaoming,11,97 5,xioali,8,92

**4.运行测试**

在运行前传入参数args(0)

![](https://img-blog.csdn.net/20181017190731201?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

运行结果：

![](https://img-blog.csdn.net/2018101719093930?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

可以看出输出结果是以一张表的形式展现的