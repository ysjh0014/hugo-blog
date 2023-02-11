---
title: "Scala基础语法五"
date: 2018-10-04 20:19:17
draft: false
---
**1.柯里化**

柯里化(Currying)指的是将原来接受两个参数的函数变成新的接受一个参数的函数的过程，新的函数返回一个以原有第二个参数为参数的函数

示例：

定义一个方法
def add(x: Int, y: Int) = x + y

调用这个方法应该是add(1，2)

现在将这个方法变形
def add(x:Int)(y:Int) = x + y

这时候调用就是add(1)(2)

最后结果都一样是 3，这种方式（过程）就叫柯里化，经过柯里化之后，函数的通用性有所降低，但是适用性有所提高

演变过程：
def add(x: Int) = (y: Int) => x +

(y: Int) => x + y 为一个匿名函数,，也就意味着 add 方法的返回值为一个匿名函数

**2.偏函数**

被包在花括号内没有 match 的一组 case 语句是一个偏函数，它是 PartialFunction[A, B]的一个实例，A 代表参数类型，B 代表返回类型，常用作输入模式匹配

示例：
def fun(a: String): Int = { if (a.equals("a")) 100 else 0 } def fun1: PartialFunction[String, Int] = { case "a" => 100 case _ => 0 } def main(args: Array[String]): Unit = { println(fun("a")) println(fun1("a")) }

上边两个方法最终输出结果都是一样的，但是fun1方法是偏函数

**3.数组的定义**

数组的定义，定义一个固定长度的数组， 长度可变,，内容可变：
var x:Array[String] = new Array[String](3)

或者

var y = new Array[String](3)

定义定长数组, 长度不可变, 内容可变：

val z = Array(1,2,3)

修改第 0 个元素的内容

z(0) = 100

**4.map|flatten|flatMap|foreach 方法的使用**

map：map 方法是将 array 数组中的每个元素进行某种映射操作， (x: Int) => x /* 2 为一个匿名函数，x 就是 array 中的每个元素

定义一个数组：
val arr = Array[Int](2, 4, 6, 9, 3)
 
val y = arr map((x: Int) => x /* 2) val z = arr.map(x => x /* 2) 编译器会自动推测 x 的数据类型 val x = array.map(_ /* 2) _ 表示入参, 表示数组中的每个元素值

上边三种写法都可以

----------------------------------------分割线------------------------------------------

先定义一个数组
val words = Array("hello tom hello jim hello jerry", "hello Hatano")

将数组中的每个元素进行分割

val splitWords: Array[Array[String]] = words.map(wd => wd.split(" "))

此时数组中的每个元素进过 split 之后变成了 Arra， flatten 是对 splitWords里面的元素进行扁平化操作

val flattenWords = splitWords.flatten

上述的 2 步操作, 可以等价于 flatMap, 意味先 map 操作后进行 flatten 操作

val result: Array[String] = words.flatMap(wd => wd.split(" "))

打印每个元素

flattenWords.foreach(println) result.foreach(println) 这两个打印的效果一样