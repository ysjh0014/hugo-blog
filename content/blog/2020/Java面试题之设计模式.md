---
title: "Java面试题之设计模式"
date: 2020-04-01 14:15:13
draft: false
featured_image: "https://hugo-ys.oss-cn-hangzhou.aliyuncs.com/static/img/java.png"
---
**单例模式：**

定义：

确保某一个类只有一个实例，而且自行实例化并向整个系统提供这个实例

简单来说就是这个类只能有一个实例，必须自己创建自己的唯一实例，必须给其它所有对象提供这一实例

适用场景：

在一个系统内，要求一个类有且仅有一个对象

优点：

减少了内存，避免了对资源的多重占用，对于创建比较繁琐的对象，只生成一个实例，减少了系统的性能开销

缺点：

单例模式一般没有接口，扩展困难

**实现：**

1).饿汉式单例(立即加载)
public class Danli { private Danli(){ } private static Danli danli=new Danli(); public static Danli getInstance(){ return danli; } }

单例类通过将构造方法设置为private避免类在外部被实例化，其他类只能通过getInstance()方法访问到这个唯一实例

饿汉式单例在类加载初始化时就创建好一个静态的对象供外部使用，这个对象不会改变，所以本身就是线程安全的

2).懒汉式单例(延迟加载方式)
public class Danli { private Danli(){ } private static Danli danli=null; public static Danli getInstance(){ if(danli==null){ danli=new Danli(); } return danli; } }

该写法在多线程模式下会产生多个danli对象，应该对在多线程环境下的懒汉式单例就行改进

public class Danli { private Danli() { } private static Danli danli = null; public static Danli getInstance() { if (danli == null) { synchronized (Danli.class) { if(danli==null) { danli = new Danli(); } } } return danli; } }

在方法上加synchronized同步锁解决了多线程环境下多个实例对象的问题，但是运行效率低下，下一个线程想要获取对象，就必须等待上一个线程释放锁以后，所以使用双重检查进一步优化，可以避免整个方法被锁，可以提高执行效率

双重检查懒加载的问题：在Java指令中创建对象和赋值操作是分开进行的，即danli=new Danli()语句分两步执行，但JVM不能保证这两个操作的先后顺序，可能JVM会为新的Danli实例分配空间，然后直接赋值给Instance成员，然后再去初始化这个Danli实例，这样当其它线程访问到Danli实例时，因为没有初始化就会报错

针对上边的问题，可以使用Volatile来禁止重排序，但是在JDK1.5之前Volatile不能解决重排序问题，在此之前可以采取静态内部类的方法实现

3).静态内部类实现
public class Danli{ private Danli(){ } //静态内部类 private static class Inner{ private static Danli danli=new Danli(); } public static Danli getInstance(){ return Inner.danli; } }

静态内部类虽然保证了单例模式在多线程下的线程安全，但是在遇到序列化对象时，默认的方式运行得到的结果就是多例的

4).static静态代码块实现
public class Danli{ private Danli(){ } private static Danli danli=null; //静态代码块 static{ danli=new Danli(); } public static Danli getInstance(){ return danli; } }

5).内部枚举类实现

public class SingletonFactory { // 内部枚举类 private enum EnmuSingleton{ Singleton; private Singleton8 singleton; //枚举类的构造方法在类加载是被实例化 private EnmuSingleton(){ singleton = new Singleton8(); } public Singleton8 getInstance(){ return singleton; } } public static Singleton8 getInstance() { return EnmuSingleton.Singleton.getInstance(); } } class Singleton8{ public Singleton8(){} }

**工厂方法模式：**

定义：

定义一个用于创建对象的接口，让子类决定实例化哪一个类，工厂方法将一个类的实例化延迟到子类

适用场景：

有大量产品需要创建，并且具有共同的接口时，可以通过工厂方法模式进行创建

**简单工厂模式：**

**抽象工厂方法：**

**装饰模式：**

定义：

动态的给一个对象添加一些新的功能，要求装饰对象和被装饰对象实现同一个接口，装饰对象持有被装饰对象的实例

简单理解就是，这里有有3个类，人类，男人类，女人类，人类是父类，男人类和女人类是子类，如果使用继承这里就多了2个类，但是用装饰模式，可以直接将人类装饰城男人类或者女人类，不用进行继承，降低了类与类之间的耦合

适用场景：

需要扩展一个类的功能，需要动态的给一个对象增加一个功能，这些功能可以再动态撤销

缺点：

产生过多相似的对象，不容易排查错误

**观察者模式：**

定义：

定义对象间一种一对多的依赖关系，使得每当一个对象改变状态，则所有依赖它的对象都会得到通知并自动更新

适用场景：

需要遍历集合中的对象

**外观模式：**