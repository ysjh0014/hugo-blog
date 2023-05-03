---
title: "Java面试题之反射"
date: 2020-04-01 16:08:50
draft: false
featured_image: "https://hugo-ys.oss-cn-hangzhou.aliyuncs.com/static/img/java.png"
tags:
- Java
categories: 
- Java
---
**1.什么是反射**

在运行状态中，对于任何一个类，都能够知道这个类的所有属性和方法，对于任何一个对象，都能够调用它的任意一个方法和属性，这种动态获取的信息以及动态调用对象的方法的功能称为Java语言的反射机制

**2.反射的作用**

动态的创建类的实例，将类绑定到现有的对象中，或者从现有的对象中获取类型

应用程序需要在运行时从某个特定的程序集中载入一个特定的类

**3.反射创建类实例的三种方式**

以类A为例：

Class class1=A.Class；

Class class2=new A().getClass()；

Class class3=Class.forName("A")；

**4.动态代理的几种实现方式**

JDK动态代理和CGLIB代理

JDK动态代理：

只能为接口创建代理实例

其实现主要是通过java.lang.reflect.Proxy和java.lang.reflect.InvocationHandler接口

Proxy类主要用来获取动态代理对象，InvocationHandler接口用来约束调用者实现，每一个动态代理类都必须要实现InvocationHandler这个接口的invoke方法，并且每个代理类的实例都关联到一个handler，通过Proxy.newProxyInstance创建的代理对象是在JVM运行时动态生成的一个对象，这个代理对象就会实现我们传入的接口的方法，当我们通过代理对象调用一个方法的时候，这个方法的调用就会被转发为由InvocationHandler这个接口的invoke方法来进行调用

代码实现：
public interface Sourceable { public void method(); }
 
public class Source implements Sourceable { @Override public void method() { System.out.println("这是起初的方法"); } }
 
import java.lang.reflect.InvocationHandler; import java.lang.reflect.Method; public class SourceHandler implements InvocationHandler { private Object source; public SourceHandler(Object source){ this.source=source; } @Override public Object invoke(Object proxy, Method method, Object[] args) throws Throwable { before(); Object object = method.invoke(source, args); after(); return object; } public void before(){ System.out.println("before!"); } public void after(){ System.out.println("after!"); } }
 
import java.lang.reflect.Proxy; public class TestSource { public static void main(String[] args) { Source source = new Source(); Sourceable obj=(Sourceable) Proxy.newProxyInstance(source.getClass().getClassLoader(), source.getClass().getInterfaces(), new SourceHandler(source)); obj.method(); } }

CGLIB动态代理：

CGLIB采用动态创建子类的方式生成代理对象，所以不能对目标类中的final或者private方法进行代理

CGLIB采用字节码技术，其原理是通过字节码技术为一个类创建子类，并在子类中采用方法拦截的技术拦截所有父类方法的调用，实现Enhancer类生成代理类的方法getProxy(SuperClass.class)，该方法通过扩展父类的class来创建代理对象，实现MethodInterceptor接口的intercept()方法，该方法拦截所有目标类方法的调用，通过代理类proxy.invokeSuper(obj，args)方法调用父类中的方法

优缺点：

CGLIB创建的动态代理对象的性能比JDK创建的动态代理对象的性能高，但CGLIB在创建代理对象时所花费的时间比JDK动态代理多，对于Singleton的代理对象或者具有实例池的代理，因为不需要频繁的创建代理对象，所以比较适合采用CGLIB动态代理技术，反之则采用JDK动态代理技术

**5.快速失败(fail-fast)和安全失败(fail-safe)的区别**

快速失败(fail-fast)：

在用迭代器遍历一个集合对象时，如果遍历过程中对集合对象的内容进行了修改(增删改查)，则会抛出Concurrent Modification Exception

原理：迭代器在遍历时直接访问集合中的内容，并且在遍历过程中使用一个modCount变量，集合在被遍历期间如果内容发生变化，就会改变modCount的值，每当迭代器使用hashNext()/next()遍历下一个元素之前，都会检测modCount变量是否为expectedmodCount值，是的话就返回遍历，否则抛出异常，终止遍历

注意：这里异常的抛出条件是检测到modCount！=expectedmodCount这个条件，如果集合发生变化时修改，modCount值刚好又设置为了expectedmodCount值，则异常不会抛出，因此，不能依赖于这个异常是否抛出而进行并发操作的编程，这个异常只建议用于检测并发修改的bug

场景：java.util包下的集合类都是快速失败的，不能在多线程下发生并发修改(迭代过程中被修改)

安全失败(fail-safe)：

采用安全失败机制的集合容器，在遍历时不是直接在集合内容上访问的，而是先复制原有集合内容，在拷贝的集合上进行遍历

原理：由于迭代时是对原集合的拷贝进行遍历，所以在遍历过程中对原集合所做的修改并不能被迭代器检测到，所以不会触发Concurrent Modification Exception

缺点：基于拷贝内容的优点是避免了Concurrent Modification Exception，但是，迭代器并不能访问到修改后的内容，即迭代器遍历的是开始遍历那一刻拿到的集合拷贝，在遍历期间原集合发生的修改迭代器是不知道的

场景：java.util.concurrent包下的容器都是安全失败，可以在多线程下并发使用，并发修改