---
title: "Dubbo之API配置"
date: 2019-07-11 09:51:37
draft: false
---
1.代码实现

既然是通过API配置实现的，那么就是要创建纯Java项目或者Maven项目，我这里以Maven项目为例

(1).首先下载dubbo和zookeeper相关的的jar包
<dependencies> <dependency> <groupId>com.alibaba</groupId> <artifactId>dubbo</artifactId> <version>2.6.0</version> <exclusions> <exclusion> <groupId>org.springframework</groupId> <artifactId>spring-context</artifactId> </exclusion> <exclusion> <groupId>org.springframework</groupId> <artifactId>spring-beans</artifactId> </exclusion> <exclusion> <groupId>org.springframework</groupId> <artifactId>spring-web</artifactId> </exclusion> </exclusions> </dependency> <dependency> <groupId>com.101tec</groupId> <artifactId>zkclient</artifactId> <version>0.2</version> </dependency> </dependencies>

(2).创建服务提供者

public class Test { public static void main(String[] args) throws InterruptedException { // 服务实现 XxxService xxxService = new XxxServiceImpl(); // 当前应用配置 ApplicationConfig application = new ApplicationConfig(); application.setName("xxx"); RegistryConfig registry = new RegistryConfig(); registry.setProtocol("zookeeper"); registry.setAddress("192.168.1.233:2181"); //这里填写zookeeper的地址 // 服务提供者协议配置 ProtocolConfig protocol = new ProtocolConfig(); protocol.setName("dubbo"); protocol.setPort(12345); protocol.setThreads(200); // 注意：ServiceConfig为重对象，内部封装了与注册中心的连接，以及开启服务端口 // 服务提供者暴露服务配置 ServiceConfig<XxxService> service = new ServiceConfig<XxxService>(); // 此实例很重，封装了与注册中心的连接，请自行缓存，否则可能造成内存和连接泄漏 service.setApplication(application); service.setRegistry(registry); // 多个注册中心可以用setRegistries() service.setProtocol(protocol); // 多个协议可以用setProtocols() service.setInterface(XxxService.class); service.setRef(xxxService); service.setVersion("1.0.0"); // 暴露及注册服务 service.export(); Thread.sleep(20 /* 1000); } }

(3).服务消费者

public class TestClient { public static void main(String[] args) { // 当前应用配置 ApplicationConfig application = new ApplicationConfig(); application.setName("yyy"); // 注意：ReferenceConfig为重对象，内部封装了与注册中心的连接，以及与服务提供方的连接 RegistryConfig registry = new RegistryConfig(); registry.setProtocol("zookeeper"); registry.setAddress("192.168.1.233:2181"); // 引用远程服务 ReferenceConfig<XxxService> reference = new ReferenceConfig<XxxService>(); // 此实例很重，封装了与注册中心的连接以及与提供者的连接，请自行缓存，否则可能造成内存和连接泄漏 reference.setApplication(application); reference.setRegistry(registry); // 多个注册中心可以用setRegistries() reference.setInterface(XxxService.class); reference.setVersion("1.0.0"); // 和本地bean一样使用xxxService XxxService service = reference.get();// 注意：此代理对象内部封装了所有通讯细节，对象较重，请缓存复用 System.out.println(service.sayHello("CHenmin")); } }

(4).运行结果

![](https://img-blog.csdnimg.cn/20190711093931317.png)

上边就是dubbo使用API配置实现的方式，这种配置还是有使用场景的，我就是遇到了使用spring配置文件配置方式解决不了的情况，使用API配置就可以解决