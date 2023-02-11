---
title: "Scala面向对象一"
date: 2018-10-05 16:18:49
draft: false
---
**1.Scala单例对象**

在 Scala 中，是没有 static 这个东西的，但是它也为我们提供了单例模式的实现方法，那就是使用关键字 object， object 对象不能带参数
object test { def test1(a: String) = { println(a) } } object Test2 { def main(args: Array[String]): Unit = { test.test1("智障") println(test) } }

输出结果：

![](https://img-blog.csdn.net/20181005122121784?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

object是一个单例对象，不能new，object中定义的方法和变量都是静态的，可以通过对象名.方法和对对象名.变量访问

**2.Scala类**
class Student { var name: String = _ val age: Int = 10 } object Test { val name: String = "zhangsan" def main(args: Array[String]): Unit = { val student = new Student() student.name = "laoYang" println(s"student.name ====== ${student.name} ${student.age}") println("Test.name ======" + Test.name) } }

在 Scala 中，类并不用声明为 public

如果你没有定义构造器，类会有一个默认的空参构造器

var修饰的变量，这个变量对外提供 getter setter 方法

val修饰的变量，对外提供了 getter 方法，没有 setter

var name：String = "zhangsan"中的_表示一个占位符，编译器会根据你变量的具体类型赋予相应初始值，使用_占位符时变量类型必须指定，并且val 修饰的变量不能使用占位符

var student=new Student()中调用空参构造器,可以加() 也可以不加
class Student1(val name: String, var age: Int) { var gender: String = _ def this(name: String, age: Int, gender: String) { this(name, age) this.gender = gender } }

定义在类后面的为类主构造器，一个类可以有多个辅助构造器，辅助构造器，使用 def this，在辅助构造器中必须先调用类的主构造器

**3.访问权限**

1)构造器的访问权限
class Student2 private (val name: String, var age: Int) { var gender: String = _ def this(name: String, age:Int, gender: String){ this(name, age) this.gender = gender } }

private加在主构造器前面标识这个主构造器是私有的，外部不能访问这个构造器

2)成员变量的访问权限
class Student3 private (val name: String, private var age: Int) { var gender: String = _ def this(name: String, age:Int, gender: String){ this(name, age) this.gender = gender } private[this] val province: String = "北京市" def getAge = 18 } object Student3 { def main(args: Array[String]): Unit = { val s3 = new Student3("Angelababy", 30) s3.age = 29 println(s"${s3.age}") } }

age 在这个类中是有 getter setter 方法的，但是前面如果加上了 private 修饰，也就意味着 age 只能在这个类的内部以及其伴生类对象中可以访问修改，其他外部类不能访问

private[this]关键字标识该属性只能在类的内部访问， 伴生类不能访问

object Student3是类的伴生对象，伴生对象可以访问类的私有方法和属性

3)类包的访问权限
private[this] class Student4(val name: String, private var age: Int) { var xx: Int = _ }

private[包名] class 放在类声明最前面，是修饰类的访问权限，也就是说类在某些包下不可见或不能访问

private[sheep] class 代表 student4 在 sheep 包下及其子包下可以见，同级包中不能访问

4)伴生类

在Scala 中，当单例对象与某个类共享同一个名称时，他被称作是这个类的伴生对象，必须在同一个源文件里定义类和它的伴生对象，类被称为是这个单例对象的伴生类，类和它的伴生对象可以互相访问其私有成员