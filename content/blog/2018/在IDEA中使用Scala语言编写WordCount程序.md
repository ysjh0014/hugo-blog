---
title: "在IDEA中使用Scala语言编写WordCount程序"
date: 2018-10-11 20:24:40
draft: false
---
**1.使用IDEA创建Maven项目**

**2.导入pom.xml文件**
<properties> <maven.compiler.source>1.8</maven.compiler.source> <maven.compiler.target>1.8</maven.compiler.target> <scala.version>2.11.8</scala.version> <spark.version>2.1.0</spark.version> <hadoop.version>2.6.0</hadoop.version> <encoding>UTF-8</encoding> </properties> <dependencies> <!-- 导入scala的依赖 --> <dependency> <groupId>org.scala-lang</groupId> <artifactId>scala-library</artifactId> <version>${scala.version}</version> </dependency> <!-- 导入spark的依赖 --> <dependency> <groupId>org.apache.spark</groupId> <artifactId>spark-core_2.11</artifactId> <version>${spark.version}</version> </dependency> <!-- 指定hadoop-client API的版本 --> <dependency> <groupId>org.apache.hadoop</groupId> <artifactId>hadoop-client</artifactId> <version>${hadoop.version}</version> </dependency> </dependencies> <build> <pluginManagement> <plugins> <!-- 编译scala的插件 --> <plugin> <groupId>net.alchim31.maven</groupId> <artifactId>scala-maven-plugin</artifactId> <version>3.2.2</version> </plugin> <!-- 编译java的插件 --> <plugin> <groupId>org.apache.maven.plugins</groupId> <artifactId>maven-compiler-plugin</artifactId> <version>3.5.1</version> </plugin> </plugins> </pluginManagement> <plugins> <plugin> <groupId>net.alchim31.maven</groupId> <artifactId>scala-maven-plugin</artifactId> <executions> <execution> <id>scala-compile-first</id> <phase>process-resources</phase> <goals> <goal>add-source</goal> <goal>compile</goal> </goals> </execution> <execution> <id>scala-test-compile</id> <phase>process-test-resources</phase> <goals> <goal>testCompile</goal> </goals> </execution> </executions> </plugin> <plugin> <groupId>org.apache.maven.plugins</groupId> <artifactId>maven-compiler-plugin</artifactId> <executions> <execution> <phase>compile</phase> <goals> <goal>compile</goal> </goals> </execution> </executions> </plugin> <!-- 打jar插件 --> <plugin> <groupId>org.apache.maven.plugins</groupId> <artifactId>maven-shade-plugin</artifactId> <version>2.4.3</version> <executions> <execution> <phase>package</phase> <goals> <goal>shade</goal> </goals> <configuration> <filters> <filter> <artifact>/*:/*</artifact> <excludes> <exclude>META-INF//*.SF</exclude> <exclude>META-INF//*.DSA</exclude> <exclude>META-INF//*.RSA</exclude> </excludes> </filter> </filters> </configuration> </execution> </executions> </plugin> </plugins> </build>

注意： 这里的Scala Spark Hadoop版本必须按照集群上的修改，特别是Scala和Spark的，要和你集群上的版本号一致，可以在Spark集群中使用Spark Shell模式查看版本号

**3.编写WordCount程序**
package cn.ysjh0014 import org.apache.spark.rdd.RDD import org.apache.spark.{SparkConf, SparkContext} object ScalaWordCount { def main(args: Array[String]): Unit = { //创建Spark配置，应用程序的名字 val conf = new SparkConf().setAppName("ScalaWordCount") //创建Spark程序执行的入口 val sc = new SparkContext(conf) //指定以后从哪读取数据创建RDD(弹性分布式数据集) val line = sc.textFile(args(0)) //切分压平 val word = line.flatMap(_.split(" ")) //将单词和1组成元组 val WordOne = word.map((_, 1)) //按照key进行聚合 val reduce = WordOne.reduceByKey(_ + _) //排序 val sort = reduce.sortBy(_._2, false) //将结果保存到hdfs sort.saveAsTextFile(args(1)) //释放资源 sc.stop() } }

**4.使用Maven打成jar包**

在IDEA中view---->Tool Windows--->Maven Projects--->Package，jar包在target下，有两个jar包，original-Spark-1.0-SNAPSHOT.jar是只将代码打成了jar包，Spark-1.0-SNAPSHOT.jar是将所有依赖也打成了jar包

**5.提交到Spark集群上测试**
bin/spark-submit \ --master spark://cdh0:7077 \ --class cn.ysjh0014.ScalaWordCount \ 包名+项目名 /opt/package/original-Spark-1.0-SNAPSHOT.jar \ jar包所在目录 hdfs://cdh0:8020/usr/ys/input/test.txt \ 读取数据的hdfs路径 hdfs://cdh0:8020/usr/output 保存数据到hdfs的路径

**6.查看运行结果**

![](https://img-blog.csdn.net/20181011202417211?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

至此运行成功