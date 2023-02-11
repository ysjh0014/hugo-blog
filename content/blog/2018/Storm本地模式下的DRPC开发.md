---
title: "Storm本地模式下的DRPC开发"
date: 2018-11-14 20:57:28
draft: false
---
根据官方文档Local DRPC模式开发可以很容易的写出代码

下面是我运行过的代码：
package cn.ysjh.drpc; import org.apache.storm.Config; import org.apache.storm.LocalCluster; import org.apache.storm.LocalDRPC; import org.apache.storm.drpc.LinearDRPCTopologyBuilder; import org.apache.storm.task.OutputCollector; import org.apache.storm.task.TopologyContext; import org.apache.storm.topology.OutputFieldsDeclarer; import org.apache.storm.topology.base.BaseRichBolt; import org.apache.storm.tuple.Fields; import org.apache.storm.tuple.Tuple; import org.apache.storm.tuple.Values; import java.util.Map; public class LocalDRPCStorm { public static class MyBolt extends BaseRichBolt{ private OutputCollector outputCollector; @Override public void prepare(Map map, TopologyContext topologyContext, OutputCollector outputCollector) { this.outputCollector=outputCollector; } @Override public void execute(Tuple tuple) { Object value = tuple.getValue(0); String name = tuple.getString(1); String result="My name:"+name; this.outputCollector.emit(new Values(value,result)); } @Override public void declareOutputFields(OutputFieldsDeclarer outputFieldsDeclarer) { outputFieldsDeclarer.declare(new Fields("id","result")); } } public static void main(String[] ages){ LinearDRPCTopologyBuilder builder = new LinearDRPCTopologyBuilder("addUser"); builder.addBolt(new MyBolt()); LocalDRPC drpc = new LocalDRPC(); LocalCluster cluster = new LocalCluster(); cluster.submitTopology("localDRPC",new Config(),builder.createLocalTopology(drpc)); String execute = drpc.execute("addUser", "ysjh"); System.err.println("成功"+execute); cluster.shutdown(); drpc.shutdown(); } }

运行结果：

![](https://img-blog.csdnimg.cn/20181114205649792.png)

如果运行代码后找不到结果，可以使用Debug模式打个断点