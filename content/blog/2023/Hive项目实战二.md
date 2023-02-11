---
title: "Hive项目实战二"
date: 2018-09-12 12:31:22
draft: false
---
**1.数据清洗**

1)数据分析

![](https://img-blog.csdn.net/20180910210650938?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

在video.txt中，视频可以有多个所属分类,每个所属分类用&符号分割,并且分割的两边有空格字符,多个相关视频又用“\t”进行分割。为了分析数据时方便对存在多个子元素的数据进行操作,我们首先进行数据重组清洗操作。

具体做法：将所有的类别用“&”分割,同时去掉两边空格,多个相关视频 id 也使用“&”进行分割，这里看起来将"&"换成"\t"更方便，但是如果这样做就会将视频所属分类分割成不同字段，这样就没有办法进行清洗了

2)注意事项

这里的数据清洗不涉及reduce操作，所以只用map即可，视频的相关视频id可以没有，但是比如评论数必须有值，没有评论即为0，所以如果一条数据的字段缺少，也是脏数据，是要被清洗的

**2.数据清洗具体代码编写**

创建的是maven工程，所以要首先导入pom.xml文件
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd"> <modelVersion>4.0.0</modelVersion> <groupId>com.z</groupId> <artifactId>youtube</artifactId> <version>0.0.1-SNAPSHOT</version> <packaging>jar</packaging> <name>youtube</name> <url>http://maven.apache.org</url> <properties> <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding> </properties> <repositories> <repository> <id>centor</id> <url>http://central.maven.org/maven2/</url> </repository> </repositories> <dependencies> <dependency> <groupId>junit</groupId> <artifactId>junit</artifactId> <version>3.8.1</version> <scope>test</scope> </dependency> <dependency> <groupId>org.apache.hadoop</groupId> <artifactId>hadoop-client</artifactId> <version>2.7.2</version> </dependency> <dependency> <groupId>org.apache.hadoop</groupId> <artifactId>hadoop-yarn-server-resourcemanager</artifactId> <version>2.7.2</version> </dependency> </dependencies> </project>

代码具体编写思路：

首先创建一个工具类，在工具类中对数据进行清洗，即过滤掉不合法数据，去掉视频类别中“&”符号两边的空格，将“\t”换成“&”

工具类MapUtils.java
package cn.ys; public class MapUtils { public static String getTest(String ori) { String[] splits = ori.split("\t"); //1、过滤不合法数据 if(splits.length < 9) return null; //2、去掉&符号左右两边的空格 splits[3] = splits[3].replaceAll(" ", ""); StringBuilder sb = new StringBuilder(); //3、\t 换成&符号 for(int i = 0; i < splits.length; i++){ sb.append(splits[i]); if(i < 9){ if(i != splits.length - 1){ sb.append("\t"); } }else{ if(i != splits.length - 1){ sb.append("&"); } } } return sb.toString(); } }

这里可以在MapUtils.java中插入以下代码进行简单的测试，看是否能够达到数据清洗的目的

public static void main(String[] args) { String test="f9_oaKYFM0c dvpwiiii 732 People & Blogs 107 328022 3.6 321 619 5Ud0t3KQfmk QBGoAH-w4OM kD7-AU0L1RU i1e0x6w6U3M llwhZMC65C0 vce_NSpJr98 8JDcV1IF8Tw lwbeuufziAE 2yvWSNhXyME ky6vPrqTIIA fvA0HpquoB0 Nds-Bg60VmU v3AXDJtYP2w tDXS6x1HDWk qs7tqSv-hgc b36Zg7yXfEg yoMckc0X1gA VzngDp2n2GA GkmbiZMnrfI Ad09vdpHmFM "; System.out.println(getTest(test)); }

Map类MapVideo.java

package cn.ys; import java.io.IOException; import org.apache.commons.lang.StringUtils; import org.apache.hadoop.hdfs.util.EnumCounters.Map; import org.apache.hadoop.io.NullWritable; import org.apache.hadoop.io.Text; import org.apache.hadoop.mapreduce.Mapper; public class MapVideo extends Mapper<Object, Text, NullWritable, Text>{ @Override protected void map(Object key, Text value, Mapper<Object, Text, NullWritable, Text>.Context context) throws IOException, InterruptedException { Text text = new Text(); String video = MapUtils.getTest(value.toString()); text.set(video); if(video!=null) { context.write(NullWritable.get(),text); } } }

Run类MapRun.java

package cn.ys; import java.io.IOException; import org.apache.hadoop.conf.Configuration; import org.apache.hadoop.fs.FileSystem; import org.apache.hadoop.fs.Path; import org.apache.hadoop.io.NullWritable; import org.apache.hadoop.mapreduce.Job; import org.apache.hadoop.mapreduce.lib.input.FileInputFormat; import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat; import org.apache.hadoop.util.Tool; import org.apache.hadoop.util.ToolRunner; import com.sun.jersey.core.impl.provider.entity.XMLJAXBElementProvider.Text; public class MapRun implements Tool{ private Configuration conf = null; public Configuration getConf() { // TODO Auto-generated method stub return this.conf; } public void setConf(Configuration conf) { this.conf=conf; } public int run(String[] args) throws Exception { conf = this.getConf(); conf.set("inpath", args[0]); conf.set("outpath", args[1]); Job job = Job.getInstance(conf, "youtube-video-etl"); job.setJarByClass(MapRun.class); job.setMapperClass(MapVideo.class); job.setMapOutputKeyClass(NullWritable.class); job.setMapOutputValueClass(Text.class); job.setNumReduceTasks(0); this.initJobInputPath(job); this.initJobOutputPath(job); return job.waitForCompletion(true) ? 0 : 1; } private void initJobOutputPath(Job job) throws IOException { Configuration conf = job.getConfiguration(); String outPathString = conf.get("outpath"); FileSystem fs = FileSystem.get(conf); Path outPath = new Path(outPathString); if(fs.exists(outPath)){ fs.delete(outPath, true); } FileOutputFormat.setOutputPath(job, outPath); } private void initJobInputPath(Job job) throws IOException { Configuration conf = job.getConfiguration(); String inPathString = conf.get("inpath"); FileSystem fs = FileSystem.get(conf); Path inPath = new Path(inPathString); if(fs.exists(inPath)){ FileInputFormat.addInputPath(job, inPath); }else{ throw new RuntimeException("HDFS 中该文件目录不存在:" + inPathString); } } public static void main(String[] args) { try { int resultCode = ToolRunner.run(new MapRun(), args); if(resultCode == 0){ System.out.println("Success!"); }else{ System.out.println("Fail!"); } System.exit(resultCode); } catch (Exception e) { e.printStackTrace(); System.exit(1); } } }

**3.打包到集群上运行Mapreduce**

在eclipse中打成jar包，项目右键Run As----Maven build，使用-P local clean package打包

![](https://img-blog.csdn.net/20180912122258195?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

这个命令是不将项目的依赖一起打包的，因为hadoop集群中已经有这些依赖包了，就算没有，也不会将这些依赖打包，这样可以保证运行效率

将打包好的jar包传输到集群上，然后将数据文件上传到HDFS中
bin/hdfs dfs -put 数据文件在集群中的位置 要上传到hdfs中的目录

然后运行此mapreduce任务

bin/hadoop jar jar包在集群中的位置 cn.ys.MapRun 输入目录(数据文件在hdfs中的位置) 输出目录

mapreduce任务跑完之后可以在输出目录中查看数据是否清洗成功