---
title: "Java中创建线程的四种方法"
date: 2019-03-01 16:50:30
draft: false
---
**1.继承Thread类创建线程类**

(1).定义Thread类的子类，并重写该类的run方法，该run方法的方法体就是线程要完成的任务，因此把run()方法称为执行体

(2).创建Thread子类的实例，就是创建了线程对象

(3).调用线程对象的start()方法来启动该线程

具体代码：
package com.thread; public class FirstThreadTest extends Thread{ int i = 0; //重写run方法，run方法的方法体就是现场执行体 public void run() { for(;i<100;i++){ System.out.println(getName()+" "+i); } } public static void main(String[] args) { for(int i = 0;i< 100;i++) { System.out.println(Thread.currentThread().getName()+" : "+i); if(i==20) { new FirstThreadTest().start(); new FirstThreadTest().start(); } } } }

**2.通过Runnable接口创建线程类**

(1).定义Runnable接口的实现类，并重写该接口的run()方法

(2).创建Runnable实现类的实例，并以此实例作为Thread的target来创建Thread对象，该Thread对象才是真正的线程对象

(3).调用线程对象的start()方法来启动该线程

具体代码：
package com.thread; public class RunnableThreadTest implements Runnable { private int i; public void run() { for(i = 0;i <100;i++) { System.out.println(Thread.currentThread().getName()+" "+i); } } public static void main(String[] args) { for(int i = 0;i < 100;i++) { System.out.println(Thread.currentThread().getName()+" "+i); if(i==20) { RunnableThreadTest rtt = new RunnableThreadTest(); new Thread(rtt,"新线程1").start(); new Thread(rtt,"新线程2").start(); } } } }

**3.通过Callable和Future创建线程**

(1).创建Callable接口的实现类，并实现call()方法，该call()方法将作为线程执行体，并且有返回值

(2).创建Callable实现类的实例，使用FutureTask类来包装Callable对象，该FutureTask对象封装了该Callable对象的call()方法的返回值。（FutureTask是一个包装器，它通过接受Callable来创建，它同时实现了Future和Runnable接口）

(3).使用FutureTask对象作为Thread对象的target创建并启动新线程

(4).调用FutureTask对象的get()方法来获得子线程执行结束后的返回值

具体代码：
package com.thread; import java.util.concurrent.Callable; import java.util.concurrent.ExecutionException; import java.util.concurrent.FutureTask; public class CallableThreadTest implements Callable<Integer> { public static void main(String[] args) { CallableThreadTest ctt = new CallableThreadTest(); FutureTask<Integer> ft = new FutureTask<>(ctt); for(int i = 0;i < 100;i++) { System.out.println(Thread.currentThread().getName()+" 的循环变量i的值"+i); if(i==20) { new Thread(ft,"有返回值的线程").start(); } } try { System.out.println("子线程的返回值："+ft.get()); } catch (InterruptedException e) { e.printStackTrace(); } catch (ExecutionException e) { e.printStackTrace(); } } @Override public Integer call() throws Exception { int i = 0; for(;i<100;i++) { System.out.println(Thread.currentThread().getName()+" "+i); } return i; } }

**4.使用Executor框架来创建线程池**

newFixThreadPool(int n)：固定大小的线程池

使用于为了满足资源管理需求而需要限制当前线程数量的场合，适用于负载比较重的服务器

具体代码：
import java.util.concurrent.ExecutorService; import java.util.concurrent.Executors; public class Test { public static void main(String[] args) { ExecutorService ex=Executors.newFixedThreadPool(5); for(int i=0;i<5;i++) { ex.submit(new Runnable() { @Override public void run() { for(int j=0;j<10;j++) { System.out.println(Thread.currentThread().getName()+j); } } }); } ex.shutdown(); } }

newSingleThreadPoolExecutor：单线程池

需要保证顺序执行各个任务的场景

具体代码：
import java.util.concurrent.ExecutorService; import java.util.concurrent.Executors; public class Test { public static void main(String[] args) { ExecutorService ex=Executors.newSingleThreadExecutor(); for(int i=0;i<5;i++) { ex.submit(new Runnable() { @Override public void run() { for(int j=0;j<10;j++) { System.out.println(Thread.currentThread().getName()+j); } } }); } ex.shutdown(); } }

newCashedThreadPool：缓存线程池

当提交任务速度高于线程池中任务处理速度时，缓存线程池会不断地创建线程

适用于提交短期的异步小程序，以及负载较轻的服务器

具体代码：
import java.util.concurrent.ExecutorService; import java.util.concurrent.Executors; public class Test { public static void main(String[] args) { ExecutorService ex=Executors.newCachedThreadPool(); for(int i=0;i<5;i++) { ex.submit(new Runnable() { @Override public void run() { for(int j=0;j<10;j++) { System.out.println(Thread.currentThread().getName()+j); } } }); } ex.shutdown(); } }