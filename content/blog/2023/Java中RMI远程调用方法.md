---
title: "Java中RMI远程调用方法"
date: 2018-12-05 20:25:37
draft: false
---
之前有两篇文章是关于RPC的，Java中的RMI则是一种特殊的RPC实现方式，都是远程方法调用，即客户端可以调用服务器端的方法

下面实现一个Java RMI

1.创建远程接口类RmiTest.java
package cn.ysjh; import java.rmi.Remote; import java.rmi.RemoteException; public interface RMITest extends Remote { public String test() throws RemoteException; }

2.创建实现远程接口的类RmiTestImpl.java

package cn.ysjh; import java.rmi.RemoteException; import java.rmi.server.UnicastRemoteObject; public class RMITestImpl extends UnicastRemoteObject implements RMITest { public RMITestImpl() throws RemoteException { super(); } public String test() { return "hello world!"; } }

3.构建服务器端Server.java

package cn.ysjh; import java.net.MalformedURLException; import java.rmi.Naming; import java.rmi.RemoteException; import java.rmi.registry.LocateRegistry; public class Server { public Server() { try { RMITestImpl rmiTest = new RMITestImpl(); LocateRegistry.createRegistry(8880); try { Naming.rebind("rmi://localhost:8880/Service",rmiTest); System.out.println("Server start......."); } catch (MalformedURLException e) { e.printStackTrace(); } } catch (RemoteException e) { e.printStackTrace(); } } public static void main(String[] args){ new Server(); } }

4.构建客户端Client.java

package cn.ysjh; import javax.naming.InitialContext; import javax.naming.NamingException; import java.rmi.RemoteException; public class Client { public static void main(String[] args) throws RemoteException { try { InitialContext context = new InitialContext(); RMITest lookup = (RMITest) context.lookup("rmi://localhost:8880/Service"); System.out.println("Client start..."); System.out.println(lookup.test()); } catch (NamingException e) { e.printStackTrace(); } } }

运行时先运行服务器端Server.java，然后再运行客户端Client.java

运行效果截图：

![](https://img-blog.csdnimg.cn/20181205202457512.png)

![](https://img-blog.csdnimg.cn/20181205202519216.png)