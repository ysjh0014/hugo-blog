---
title: "基于Hadoop的RPC原理实现"
date: 2018-11-14 19:55:10
draft: false
---
上一篇文章简单的讲解了一下RPC的概念和原理

简单来说就是一台机器上的应用想调用另一台机器上的函数或者方法，由于不在同一个内存空间中，所以不能直接调用，要使用RPC协议来调用

下边就来基于Hadoop来实现RPC调用

1.加入Hadoop的依赖包
<repositories> <repository> <id>cloudera</id> <url>http://repository.cloudera.com/artifactory/cloudera-repos</url> </repository> </repositories> <dependency> <groupId>org.apache.hadoop</groupId> <artifactId>hadoop-client</artifactId> <version>2.5.0-cdh5.3.6</version> </dependency>

如果是CDH版本的Hadoop，需要加入repository，如果是apache版本的则不需要

2.创建服务器端

UserServer接口：
package cn.ysjh.drpc; public interface UserService { public static final long versionID=8888; public void addUser(String name,int age); }

UserServerImpl实现类：

package cn.ysjh.drpc; public class UserServerImpl implements UserService { @Override public void addUser(String name, int age) { System.out.println("姓名："+name+" "+"年龄"+age); } }

ServerRPC服务器端代码：

package cn.ysjh.drpc; import org.apache.hadoop.conf.Configuration; import org.apache.hadoop.ipc.RPC; //* RPC服务 /*/ public class ServerRPC { public static void main(String[] args) throws Exception{ Configuration configuration = new Configuration(); RPC.Builder builder = new RPC.Builder(configuration); RPC.Server server = builder.setProtocol(UserService.class) .setInstance(new UserServiceImpl()) .setBindAddress("localhost") .setPort(9999) .build(); server.start(); } }

3.实现Client端

package cn.ysjh.drpc; import org.apache.hadoop.conf.Configuration; import org.apache.hadoop.ipc.RPC; import java.io.IOException; import java.net.InetSocketAddress; //* 客户端 /*/ public class UserClient { public static void main(String[] args) throws IOException { Configuration configuration = new Configuration(); long clientVersion=8888; UserService proxy = RPC.getProxy(UserService.class, clientVersion, new InetSocketAddress("localhost", 9999), configuration); proxy.addUser("lisi",22); System.out.println("添加成功"); } }

4.运行代码

先运行服务器端代码，然后运行client端代码

只要client端有请求，server端的方法就会被调用