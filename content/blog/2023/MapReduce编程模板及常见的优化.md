---
title: "MapReduce编程模板及常见的优化"
date: 2018-06-01 20:10:46
draft: false
---
MapReduce编程模板:

public class ModuleWordCount extends Configured implements Tool{
// map
public static class WordCountMap extends Mapper<LongWritable, Text, Text, IntWritable>{
@Override
protected void setup(Context context)
throws IOException, InterruptedException {
//Nothing
}
@Override
protected void map(LongWritable key, Text value, Context context)
throws IOException, InterruptedException {
//TODO
}
@Override
protected void cleanup(
Context context)
throws IOException, InterruptedException {
//Nothing
}
}
//reduce
public static class WordCountReduce extends Reducer<Text, IntWritable, Text, IntWritable>{
@Override
protected void setup(Context context)
throws IOException, InterruptedException {
//Nothing
}
@Override
protected void reduce(Text key, Iterable<IntWritable> values,
Context context)
throws IOException, InterruptedException {
//TODO
}
@Override
protected void cleanup(
Context context)
throws IOException, InterruptedException {
//Nothing
}
}
//driver
public int run(String[] args) throws Exception{
//得到配置信息
Configuration configurable=getConf();
//创建Job
Job job=Job.getInstance(configurable,this.getClass().getSimpleName());
//运行jar
job.setJarByClass(this.getClass());
//设置job
//1. input
Path inPath=new Path(args[0]);
FileInputFormat.addInputPath(job, inPath);
//2.map
job.setMapperClass(WordCountMap.class);
job.setMapOutputKeyClass(Text.class);

job.setMapOutputValueClass(IntWritable.class);

/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*Shuffle/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*

//分区

job.setParttitionerClass(cls);

//排序

job.setSortComparatorClass(cls);

//combiner

job.setCombinerClass(cls);

//分组

job.setGroupingComparatorClass(cls);

/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*

//3.reduce
job.setReducerClass(WordCountReduce.class);
job.setOutputKeyClass(Text.class);

job.setOutputValueClass(IntWritable.class);

//设置reduce数量

job.setNumReduceTasks(数量);

//4.output
Path outPath=new Path(args[1]);
FileOutputFormat.setOutputPath(job, outPath);
//提交job
boolean completion = job.waitForCompletion(true);
return completion ? 0 : 1;
}
public static void main(String[] args) throws Exception{
//得到配置信息
Configuration configurable=new Configuration();

//设置压缩

configurable.set("mapreduce.map.output.compress","true");

configurable.set("mapreduce.map.output.compress.codec","压缩方法");

int status=ToolRunner.run(configurable, new ModuleWordCount(), args);
System.exit(status);
}

}

上边的源码就是MapRedecu编程模板，并且进行了一定的优化，主要有3处优化:

1)在map和reduce方法中添加了setup和cleanup方法，setup方法可以进行初始化，cleanup方法进行资源的关闭

2)shuffle阶段:

/*将小文件合并成大文件时多少个合并成一个大文件:

![](https://img-blog.csdn.net/20180601212029324)

/*map task输出的<key,value>对放在内存缓冲区的大小:

![](https://img-blog.csdn.net/20180601212209579)

/*spill溢写达到多少会溢出:

![](https://img-blog.csdn.net/20180601212404599)

3)压缩(compress): 这个可以在配置文件中配置，也可以在代码中进行设置

配置文件中: ![](https://img-blog.csdn.net/20180601200447398)

第一个是开启压缩，默认为false不开启，第二个是压缩所采用的技术，即所采用的哪种压缩方法，

4)Reduce的个数设置，这个在实际应用中是比较重要的，默认是一个，并不是越多越好，而是尽量保持在一个波动范围内