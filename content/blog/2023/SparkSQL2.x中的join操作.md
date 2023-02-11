---
title: "SparkSQL2.x中的join操作"
date: 2018-10-19 17:24:46
draft: false
---
前面讲了各种各样的案例，总的来说就是SparkSQL1.x和SparkSQL2.x的不同编程方式，以及创建DataFrame的不同方式和使用Sql，DataFrame API编程的方式

但是上边的案例中都是一张表，没有进行多表的联合，即使用join，下面就来使用代码详细的理解join如何进行操作

先上代码：
package cn.ysjh0014.SparkSql import org.apache.spark.sql.{DataFrame, Dataset, SparkSession} object SparkSQLJoin { def main(args: Array[String]): Unit = { val session: SparkSession = SparkSession.builder().appName("JoinTest").master("local[/*]").getOrCreate() //创建数据 import session.implicits._ val table1: Dataset[String] = session.createDataset(List("1,xiaoli,china","2,xiaohua,usa","3,xiaming,England")) //对数据进行整理 val Dept: Dataset[(Long, String, String)] = table1.map(line => { val word: Array[String] = line.split(",") val id = word(0).toLong val name = word(1).toString val country = word(2).toString (id, name, country) }) val table2: Dataset[String] = session.createDataset(List("china,中国","usa,美国")) //对数据进行整理 val Dept1: Dataset[(String, String)] = table2.map(line => { val word = line.split(",") val nation = word(0).toString val nation1 = word(1).toString (nation, nation1) }) //转换成DataFrame val df: DataFrame = Dept.toDF("id","name","country") val df1: DataFrame = Dept1.toDF("nation","nation1") //第一种，创建视图(表) // df.createTempView("table1") // df1.createTempView("table2") // // val ys: DataFrame = session.sql("SELECT name,nation1 FROM table1 JOIN table2 ON country=nation") //第二种，DataFrame API val ys: DataFrame = df.join(df1,$"country"===$"nation","left") ys.show() session.stop() } }

运行结果：

![](https://img-blog.csdn.net/20181019173058961?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

整体思路：

先创建SparkSession，然后创建数据以及DataFrame，之后使用两种方式实现Join操作，一种是创建视图的Sql方式，一种是使用DataFrame API的方式，不用创建视图