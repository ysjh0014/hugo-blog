---
title: "Storm案例之自增数字求和"
date: 2018-11-12 21:05:43
draft: false
---
**1.案例需求**

实现自增数字相加的和 1+2+3+4+5+6+........

**2.需求分析**

Spout来发送数字作为input

使用Bolt来实现求和逻辑

将结果输出到控制台

**3.导入Storm的pom依赖**
<dependency> <groupId>org.apache.storm</groupId> <artifactId>storm-core</artifactId> <version>1.1.1</version> </dependency>

**4.具体代码实现**

package cn.ysjh; import org.apache.storm.Config; import org.apache.storm.LocalCluster; import org.apache.storm.spout.SpoutOutputCollector; import org.apache.storm.task.OutputCollector; import org.apache.storm.task.TopologyContext; import org.apache.storm.topology.OutputFieldsDeclarer; import org.apache.storm.topology.TopologyBuilder; import org.apache.storm.topology.base.BaseRichBolt; import org.apache.storm.topology.base.BaseRichSpout; import org.apache.storm.tuple.Fields; import org.apache.storm.tuple.Tuple; import org.apache.storm.tuple.Values; import org.apache.storm.utils.Utils; import java.util.Map; //* 使用Storm实现求和功能 /*/ public class SumStorm { //* Spout需要继承Base /*/ public static class DataSourceSpout extends BaseRichSpout{ private SpoutOutputCollector spoutOutputCollector; //* 初始化方法，只会被调用一次 /*/ @Override public void open(Map map, TopologyContext topologyContext, SpoutOutputCollector spoutOutputCollector) { this.spoutOutputCollector=spoutOutputCollector; } int num=0; //* 会产生数据，在实际生产中肯定是从消息队列中获取数据 这个方法是一个死循环，会一直运行 /*/ @Override public void nextTuple() { this.spoutOutputCollector.emit(new Values(++num)); System.out.println("数据："+num); //防止数据产生太快 Utils.sleep(1000); } //* 声明输出字段 /*/ @Override public void declareOutputFields(OutputFieldsDeclarer outputFieldsDeclarer) { //这里要和上边nextTuple()方法中的Values中的对应 outputFieldsDeclarer.declare(new Fields("number")); } } //* 数据的累计求和Bolt：接收数据并处理 /*/ private static class SumBolt extends BaseRichBolt { //* 初始化方法，会被执行一次 /*/ @Override public void prepare(Map map, TopologyContext topologyContext, OutputCollector outputCollector) { } int sum=0; //* 获取Spout发送过来的数据 Bolt中获取值可以根据index获取，也可以根据上一个环节中定义的fields名称获取，建议使用后一种方法 /*/ @Override public void execute(Tuple tuple) { Integer Num = tuple.getIntegerByField("number"); sum+=Num; System.out.println("Sum:"+sum); } //* /*/ @Override public void declareOutputFields(OutputFieldsDeclarer outputFieldsDeclarer) { } } public static void main(String[] args){ //TopologyBuilder根据Spout和Bolt来构建topology，Storm中任何一个作业都是通过Topology的方式进行提交的，Topology中需要指定Spout和Bolt执行的顺序 TopologyBuilder builder = new TopologyBuilder(); builder.setSpout("DataSourceSpout",new DataSourceSpout()); builder.setBolt("SumBolt",new SumBolt()).shuffleGrouping("DataSourceSpout"); //创建本地模式，只需使用LocalCluster类 LocalCluster cluster = new LocalCluster(); cluster.submitTopology("SumStorm",new Config(),builder.createTopology()); } }

可以看出先实现Spout和Bolt，最后实现Topology进行本地运行

注意：

这里是本地运行模式，不需要Storm集群环境，如果要提交到集群上运行，最后的LocalCluster需要修改为StormSubmitter将Topology提交到集群上运行

**5.运行截图**

![](https://img-blog.csdnimg.cn/20181112210513493.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==,size_16,color_FFFFFF,t_70)