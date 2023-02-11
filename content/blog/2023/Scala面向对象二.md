---
title: "Scala面向对象二"
date: 2018-10-05 16:28:13
draft: false
---
**1.抽象类**

在Scala 中，使用 abstract 修饰的类称为抽象类， 在抽象类中可以定义属性、未实现的方法和具体实现的方法
abstract class Animal { println("Animal's constructor ....") /*/* 定义一个 name 属性 val name: String = "animal" /*/* 没有任何实现的方法 def sleep() /*/* 带有具体的实现的方法 def eat(f: String): Unit = { println(s"$f") } }

**2.继承**

继承是面向对象的概念，用于代码的可重用性， 被扩展的类称为超类或父类，扩展的类称为派生类或子类，Scala 可以通过使用 extends 关键字来实现继承其他类或者特质
class Dog extends Animal { println("Dog's constructor ...") override val name: String = "Dog" def sleep(): Unit = { println("躺着睡...") } override def eat(f: String): Unit = { println("") }

父类已经实现了的功能，子类必须使用 override 关键字重写

父类没有实现的方法，子类必须实现

**3.final和type关键字**

final：

final 修饰的类不能被继承

final 修饰的属性不能重写

final 修饰的方法不能被重写

type：

Scala 里的类型，除了在定义 class trait object 时会产生类型，还可以通过 type 关键字来声明类型。

type 相当于声明一个类型别名：

例如下边的示例中把 String 类型用 S
type S = String val name: S = "小星星" println(name)

通常 type 用于声明某种复杂类型，或用于定义一个抽象类型

**4.样例类/样例对象**

样例类,使用 case 关键字 修饰的类, 其重要的特征就是支持模式匹配

样例类默认是实现了序列化接口的
case class Message(msgContent: String) //*/* /* 样例 object, 不能封装数据, 其重要特征就是支持模式匹配 /*/ case object CheckHeartBeat object TestCaseClass extends App{ 可以使用 new 关键字创建实例, 也可以不使用 val msg = Message("hello") println(msg.msgContent) }