---
title: "使用SparkSQL实现根据ip地址计算归属地二"
date: 2018-10-22 18:08:40
draft: false
---
在使用SparkSQL实现根据ip地址计算归属地一 中虽然实现了最终目的，但是当数据量大的时候Join的代价是很大的，因为其他机器上都没有这个ip地址规则，所以要想进行比较只能从其他机器上拉过来再进行比较，那么如何进行优化呢，我们通过之前的使用SparkCore中的RDD的操作方式很容易就会想到将ip地址规则给缓存起来，即在每台机器上都有IP地址规则，这样就不用从其他机器上拉取了，也就减少了大量的I/O操作，实现优化

废话不多说，直接上代码：
package cn.ysjh0014.SparkSql import cn.ysjh0014.TestIp import org.apache.spark.broadcast.Broadcast import org.apache.spark.sql.{DataFrame, Dataset, SparkSession} object IpLocationSQL1 { def main(args: Array[String]): Unit = { val session: SparkSession = SparkSession.builder().appName("IpLocationSQL1").master("local[4]").getOrCreate() //取到HDFS中的ip规则 import session.implicits._ val rulesLines: Dataset[String] = session.read.textFile(args(0)) //整理ip规则数据 val rules: Dataset[(Long, Long, String)] = rulesLines.map(line => { val fields = line.split("[|]") val startNum = fields(2).toLong val endNum = fields(3).toLong val province = fields(6) (startNum, endNum, province) }) //收集ip规则到Driver端 val Rules: Array[(Long, Long, String)] = rules.collect() //广播(必须使用SparkContext),返回到Driver端 val broadcast: Broadcast[Array[(Long, Long, String)]] = session.sparkContext.broadcast(Rules) //创建RDD，读取访问日志 val accessLines: Dataset[String] = session.read.textFile(args(1)) //整理数据 val result: Dataset[Long] = accessLines.map(log => { //将log日志的每一行进行切分 val fields = log.split("[|]") val ip = fields(1) //将ip转换成十进制 val ipNum = TestIp.ip2Long(ip) ipNum }) val DFResult: DataFrame = result.toDF("ip_Num") DFResult.createTempView("table") //定义一个自定义函数(UDF),并注册，该函数的功能是输入一个ip地址对应的十进制，返回一个省份名称 session.udf.register("IpProvice", (ipNum: Long) => { //查找ip地址规则，实现已经广播，已经在Executor中了，使用广播变量的引用就可以获取ip规则对应的数据了 val IpRules: Array[(Long, Long, String)] = broadcast.value //根据ip地址对应的十进制查找省份名称 val index = TestIp.binarySearch(IpRules, ipNum) var provice = "未知" if (index != -1) { provice = IpRules(index)._3 } provice }) //执行Sql val r: DataFrame = session.sql("SELECT IpProvice(ip_Num) provice, COUNT(/*) counts FROM table GROUP BY provice ORDER BY counts DESC") r.show() session.stop() } }

运行结果就不展示了，都是跟之前的案例结果一样，这里重点在于优化的过程，而不是结果，很明显上边的代码至少不用进行Join操作了，当数据量大的时候会有优势

注意：

这里的代码是在之前的案例基础上的，但是运行之后会发现报错，如下图：

![截图如下](C:\Users\34634\Pictures\timg.jpg)

这是因为spark版本的问题，将pom文件中的spark版本由原来的2.1.0改为2.2.0就不会报错了