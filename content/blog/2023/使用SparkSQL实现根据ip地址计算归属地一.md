---
title: "使用SparkSQL实现根据ip地址计算归属地一"
date: 2018-10-19 18:06:53
draft: false
---
之前使用过RDD实现过这个案例，如果不知道可以去参考我写的博文，这里要实现的就是在之前那个基础上进行修改的，具体实现思路就是将ip地址规则和访问日志文件中的数据进行整理然后转换成DataFrame之后注册成表，然后写Sql语句进行Join操作

具体代码实现：
package cn.ysjh0014.SparkSql import cn.ysjh0014.TestIp import org.apache.spark.sql.{DataFrame, Dataset, SparkSession} object IpLocationSQL { def main(args: Array[String]): Unit = { val session: SparkSession = SparkSession.builder().appName("IpLocationSQL").master("local[4]").getOrCreate() //取到HDFS中的ip规则 import session.implicits._ val rulesLines: Dataset[String] = session.read.textFile(args(0)) //整理ip规则数据 val Dept: Dataset[(Long, Long, String)] = rulesLines.map(line => { val fields = line.split("[|]") val startNum = fields(2).toLong val endNum = fields(3).toLong val province = fields(6) (startNum, endNum, province) }) val ipRulesRdd: DataFrame = Dept.toDF("startNum","endNum","province") //创建RDD，读取访问日志 val accessLines: Dataset[String] = session.read.textFile(args(1)) //整理数据 val result : Dataset[Long] = accessLines.map(log => { //将log日志的每一行进行切分 val fields = log.split("[|]") val ip = fields(1) //将ip转换成十进制 val ipNum = TestIp.ip2Long(ip) ipNum }) val DFResult: DataFrame = result.toDF("ipNum") //创建视图 val table1: Unit = ipRulesRdd.createTempView("table1") val table2: Unit = DFResult.createTempView("table2") //写SQL val ys: DataFrame = session.sql("SELECT province,count(/*) counts FROM table1 JOIN table2 ON (ipNum>=startNum AND ipNum<=endNum) GROUP BY province ORDER BY counts DESC") ys.show() session.stop() } }

运行结果：

![](https://img-blog.csdn.net/20181019180646224?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

你执行上边代码的时候会发现很慢，数据量大的时候会更慢，这是因为进行查找的时候是一条一条数据进行比较的，而没有使用之前的二分查找，所以效率不高