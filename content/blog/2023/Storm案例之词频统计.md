---
title: "Storm案例之词频统计"
date: 2018-11-14 10:59:51
draft: false
---
**1.案例需求**

在本地模式下使用Storm实现统计指定文件中的词频个数统计

**2.需求分析**

Spout来读取指定文件的数据，并把每一行数据发送出去

Bolt来实现具体逻辑，单词分割和统计

将结果输出到控制台

Spout——>Bolt——>Bolt

**3.导入Storm的依赖，在上一篇求和案例中有这个依赖，这里就不再重复了**

**4.具体代码**
package cn.ysjh; import org.apache.commons.io.FileUtils; import org.apache.storm.Config; import org.apache.storm.LocalCluster; import org.apache.storm.spout.SpoutOutputCollector; import org.apache.storm.task.OutputCollector; import org.apache.storm.task.TopologyContext; import org.apache.storm.topology.OutputFieldsDeclarer; import org.apache.storm.topology.TopologyBuilder; import org.apache.storm.topology.base.BaseRichBolt; import org.apache.storm.topology.base.BaseRichSpout; import org.apache.storm.tuple.Fields; import org.apache.storm.tuple.Tuple; import org.apache.storm.tuple.Values; import java.io.File; import java.io.IOException; import java.util./*; public class WordcountStorm { private static class DataSourceSpout extends BaseRichSpout{ private SpoutOutputCollector spoutOutputCollector; @Override public void open(Map map, TopologyContext topologyContext, SpoutOutputCollector spoutOutputCollector) { this.spoutOutputCollector=spoutOutputCollector; } //* 读取指定文件夹下的数据：D:\测试数据 把每一行数据发射出去 /*/ @Override public void nextTuple() { //获取文件，txt是指只读取指定文件夹下的txt后缀的文件，true是指是否支持递归 Collection<File> files = FileUtils.listFiles(new File("D:\\测试数据\\storm"), new String[]{"txt"}, true); for(File file:files){ try { //获取文件中的内容 List<String> lines = FileUtils.readLines(file); //获取文件中的每行内容，并将它发射出去 for(String line:lines){ this.spoutOutputCollector.emit(new Values(line)); } //数据读取完毕之后改变文件名，否则会一直执行 FileUtils.moveFile(file, new File(file.getAbsolutePath() + System.currentTimeMillis())); } catch (IOException e) { e.printStackTrace(); } } } @Override public void declareOutputFields(OutputFieldsDeclarer outputFieldsDeclarer) { outputFieldsDeclarer.declare(new Fields("lines")); } } //* 词频分割Bolt /*/ private static class SplitBolt extends BaseRichBolt{ private OutputCollector outputCollector; @Override public void prepare(Map map, TopologyContext topologyContext, OutputCollector outputCollector) { this.outputCollector=outputCollector; } //* 对lines按照逗号进行切分 /*/ @Override public void execute(Tuple tuple) { String lines = tuple.getStringByField("lines"); String[] split = lines.split(","); //将数据发射到下一个Bolt for (String word:split){ this.outputCollector.emit(new Values(word)); } } @Override public void declareOutputFields(OutputFieldsDeclarer outputFieldsDeclarer) { outputFieldsDeclarer.declare(new Fields("words")); } } //* 词频统计Bolt /*/ private static class CountBolt extends BaseRichBolt{ @Override public void prepare(Map map, TopologyContext topologyContext, OutputCollector outputCollector) { } Map<String,Integer> map=new HashMap<>(); @Override public void execute(Tuple tuple) { String words = tuple.getStringByField("words"); Integer count = map.get(words); if(count==null){ count=0; } count++; map.put(words,count); System.out.println("---------------"); Set<Map.Entry<String, Integer>> set = map.entrySet(); for (Map.Entry<String, Integer> entry:set){ System.out.println(entry); } } @Override public void declareOutputFields(OutputFieldsDeclarer outputFieldsDeclarer) { } } public static void main(String[] args){ TopologyBuilder builder = new TopologyBuilder(); builder.setSpout("DataSourceSpout",new DataSourceSpout()); builder.setBolt("SplitBolt",new SplitBolt()).shuffleGrouping("DataSourceSpout"); builder.setBolt("CountBolt",new CountBolt()).shuffleGrouping("SplitBolt"); LocalCluster cluster = new LocalCluster(); cluster.submitTopology("WordcountStorm",new Config(),builder.createTopology()); } }

**5.运行截图**

![](https://img-blog.csdnimg.cn/20181114105736736.png)

可以看出，Storm是一行一行的读取数据的，读取一行统计一行并进行累加

注意： 文件中的数据读取完之后会将文件名改变，否则会一直重复读取该文件

前面的求和和这里的词频统计案例都是在本地模式下运行的，如果想要提交到集群上进行运行测试，需要将代码中的LocalCluster修改为StormSubmitter，但是最好的方式是在代码中写入一个if判断，如果是本地模式就使用LocalCluster，集群模式就使用StormSubmitter，这里感兴趣的可以试一试

如果要在集群上运行，使用
storm jar jar包目录 jar包中的类名

来运行Storm任务