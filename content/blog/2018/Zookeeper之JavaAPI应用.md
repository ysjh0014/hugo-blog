---
title: "Zookeeper之JavaAPI应用"
date: 2018-08-31 16:49:27
draft: false
---
1.eclipse环境搭建

首先创建一个java工程，然后将解压后的zookeeper文件中的zookeeper-3.4.10.jar、jline-0.9.94.jar、log4j-1.2.16.jar、netty-3.10.5.Final.jar、slf4j-api-1.6.1.jar、slf4j-log4j12-1.6.1.jar拷贝到工程的lib目录，最后build一下，导入工程，并且拷贝log4j.properties文件到项目根目录

log4j.properties代码:
/# Define some default values that can be overridden by system properties
zookeeper.root.logger=INFO, CONSOLE
zookeeper.console.threshold=INFO
zookeeper.log.dir=.
zookeeper.log.file=zookeeper.log
zookeeper.log.threshold=DEBUG
zookeeper.tracelog.dir=.
zookeeper.tracelog.file=zookeeper_trace.log

/#
/# ZooKeeper Logging Configuration
/#

/# Format is "<default threshold> (, <appender>)+

/# DEFAULT: console appender only
log4j.rootLogger=${zookeeper.root.logger}

/# Example with rolling log file
/#log4j.rootLogger=DEBUG, CONSOLE, ROLLINGFILE

/# Example with rolling log file and tracing
/#log4j.rootLogger=TRACE, CONSOLE, ROLLINGFILE, TRACEFILE

/#
/# Log INFO level and above messages to the console
/#
log4j.appender.CONSOLE=org.apache.log4j.ConsoleAppender
log4j.appender.CONSOLE.Threshold=${zookeeper.console.threshold}
log4j.appender.CONSOLE.layout=org.apache.log4j.PatternLayout
log4j.appender.CONSOLE.layout.ConversionPattern=%d{ISO8601} [myid:%X{myid}] - %-5p [%t:%C{1}@%L] - %m%n

/#
/# Add ROLLINGFILE to rootLogger to get log file output
/# Log DEBUG level and above messages to a log file
log4j.appender.ROLLINGFILE=org.apache.log4j.RollingFileAppender
log4j.appender.ROLLINGFILE.Threshold=${zookeeper.log.threshold}
log4j.appender.ROLLINGFILE.File=${zookeeper.log.dir}/${zookeeper.log.file}

/# Max log file size of 10MB
log4j.appender.ROLLINGFILE.MaxFileSize=10MB
/# uncomment the next line to limit number of backup files
/#log4j.appender.ROLLINGFILE.MaxBackupIndex=10

log4j.appender.ROLLINGFILE.layout=org.apache.log4j.PatternLayout
log4j.appender.ROLLINGFILE.layout.ConversionPattern=%d{ISO8601} [myid:%X{myid}] - %-5p [%t:%C{1}@%L] - %m%n
/# Add TRACEFILE to rootLogger to get log file output
/# Log DEBUG level and above messages to a log file
log4j.appender.TRACEFILE=org.apache.log4j.FileAppender
log4j.appender.TRACEFILE.Threshold=TRACE
log4j.appender.TRACEFILE.File=${zookeeper.tracelog.dir}/${zookeeper.tracelog.file}

log4j.appender.TRACEFILE.layout=org.apache.log4j.PatternLayout
/#/#/# Notice we are including log4j's NDC here (%x)
log4j.appender.TRACEFILE.layout.ConversionPattern=%d{ISO8601} [myid:%X{myid}] - %-5p [%t:%C{1}@%L][%x] - %m%n

2.API实战，包括创建Zookeeper客户端，创建子节点，获取子节点，判断znode是否存在，前提是zookeeper集群开启

package cn.ysjh0014;

import java.io.IOException;

import org.apache.zookeeper.CreateMode;
import org.apache.zookeeper.KeeperException;
import org.apache.zookeeper.WatchedEvent;
import org.apache.zookeeper.Watcher;
import org.apache.zookeeper.ZooDefs.Ids;
import org.apache.zookeeper.ZooKeeper;
import org.junit.Before;
import org.junit.Test;

public class TestZookeeper {

//连接zookeeper的server端
private String connectString="192.168.220.128:2181,192.168.220.129:2181,192.168.220.130:2181";
//超时时间设置
private int sessionTimeout=2000;
ZooKeeper zkClient;
//初始化zookeeper客户端
@Before
public void initClient() throws IOException {
zkClient=new ZooKeeper(connectString, sessionTimeout, new Watcher() {
@Override
public void process(WatchedEvent event) {
System.out.println(event.getPath()+"\t"+event.getType());
}
});
}
//创建子节点
@Test
public void create() throws KeeperException, InterruptedException {
//create中的参数分别为 节点名，节点值，节点权限，节点的类型
String path=zkClient.create("/ys", "jh".getBytes(), Ids.OPEN_ACL_UNSAFE, CreateMode.PERSISTENT);
System.out.println(path);
}

//获取子节点
@Test
public void getChildren() throws Exception {
List<String> children = zkClient.getChildren("/", true);

for (String child : children) {
System.out.println(child);
}
// 延时阻塞
Thread.sleep(Long.MAX_VALUE);
}

// 判断znode是否存在
@Test
public void exist() throws KeeperException, InterruptedException {
Stat stat = zkClient.exists("/ys", false);

System.out.println(stat == null ? "not exist" : "exist");
}

}