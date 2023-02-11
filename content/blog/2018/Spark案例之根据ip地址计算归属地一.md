---
title: "Spark案例之根据ip地址计算归属地一"
date: 2018-10-13 08:12:44
draft: false
---
**1.需求**

根据访问日志中的ip地址计算出访问者的归属地，并且按照省份，计算出访问次数，最后将计算好的结果写入到Mysql中

**2.思路分析**

1)整理访问日志中的数据，切分出ip字段，然后将ip字段转换成十进制

2)加载ip地址的规则，取出有用的字段，然后将数据缓存到内存中(Executor中的内存)

3)将访问日志中的ip与ip地址的规则进行匹配(使用二分查找进行匹配)

4)取出对应的省份名称，将其和1组合在一起

5)按照省份名进行聚合

6)将聚合后的数据写入Mysql

**3.本地模式实现**

有了上边的分析，我们可以现在本地模式下实现一下，在本地模式下实现起来比较简单

先放上实现代码和效果：
package cn.ysjh0014 import scala.io.{BufferedSource, Source} object TestIp { //将ip地址转换为十进制 def ip2Long(ip: String): Long = { val fragments = ip.split("[.]") var ipNum = 0L for (i <- 0 until fragments.length) { ipNum = fragments(i).toLong | ipNum << 8L } ipNum } //读取ip规则，并整理 def readRules(path: String): Array[(Long, Long, String)] = { //读取ip规则 val bf: BufferedSource = Source.fromFile(path) val lines: Iterator[String] = bf.getLines() //对ip规则进行整理，并放入到内存 val rules: Array[(Long, Long, String)] = lines.map(line => { val fileds = line.split("[|]") val startNum = fileds(2).toLong val endNum = fileds(3).toLong val province = fileds(7) (startNum, endNum, province) }).toArray rules } //二分查找 def binarySearch(lines: Array[(Long, Long, String)], ip: Long): Int = { var low = 0 var high = lines.length - 1 while (low <= high) { val middle = (low + high) / 2 if ((ip >= lines(middle)._1) && (ip <= lines(middle)._2)) return middle if (ip < lines(middle)._1) high = middle - 1 else { low = middle + 1 } } -1 } def main(args: Array[String]): Unit = { //读取ip规则，这些数据是在内存中 val rules: Array[(Long, Long, String)] = readRules("D:\\视频\\小牛学堂-2018\\06-Spark安装部署到高级-10天\\spark-04-Spark案例讲解\\课件与代码\\ip\\ip.txt") //将ip转换为十进制 val ipNum = ip2Long("120.78.142.230") //查找 val index = binarySearch(rules, ipNum) var Num = "未查找到该ip所在地区" if (index != -1) { Num = rules(index)._3 } print(Num) } }

运行结果：

![](https://img-blog.csdn.net/2018101221313221?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

上边测试的ip地址是我买的阿里云的服务器的公网ip地址，广东省深圳市的，还算比较准确，这个ip地址的规则实在淘宝买的，比较早期的，不是很全面

日志文件及ip地址规则下载： 链接：[https://pan.baidu.com/s/1G0DuO2pTbYCm-Ym6V0KDYw](https://pan.baidu.com/s/1G0DuO2pTbYCm-Ym6V0KDYw%C2%A0)
提取码：6egh