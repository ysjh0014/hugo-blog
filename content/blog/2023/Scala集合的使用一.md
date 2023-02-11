---
title: "Scala集合的使用一"
date: 2018-10-05 12:03:22
draft: false
---
在Scala中，集合分为可变集合(mutable)和不可变集合(immutable)

可变集合： 长度可变，内容可变

不可变集合： 长度不可变，内容不可变

示例：

不可变集合：

![](https://img-blog.csdn.net/20181005102322286?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

可变集合：

![](https://img-blog.csdn.net/20181005102556451?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

**1.定长数组与变长数组**

Array数组中的内容都可变： 分为长度可变数组(ArrayBuffer)和长度不可变数组(Array)

**2.Seq序列**

不可变的序列示例：
object ImmutListTest { def main(args: Array[String]) { //创建一个不可变的集合 val lst1 = List(1,2,3) /将0插入到lst1的前面生成一个新的List val lst2 = 0 :: lst1 val lst3 = lst1.::(0) val lst4 = 0 +: lst1 val lst5 = lst1.+:(0) //将一个元素添加到lst1的后面产生一个新的集合 val lst6 = lst1 :+ 3 val lst0 = List(4,5,6) //将2个list合并成一个新的List val lst7 = lst1 ++ lst0 //将lst0插入到lst1前面生成一个新的集合 val lst8 = lst1 ++: lst0 //将lst0插入到lst1前面生成一个新的集合 val lst9 = lst1.:::(lst0) println(lst9) } }

注意：:: 操作符是右结合的，如 9 :: 5 :: 2 :: Nil 相当于 9 :: (5 :: (2 :: Nil)

可变的序列示例：
import scala.collection.mutable.ListBuffer object MutListTest { //构建一个可变列表，初始有3个元素1,2,3 val lst0 = ListBuffer[Int](1,2,3) //创建一个空的可变列表 val lst1 = new ListBuffer[Int] //向lst1中追加元素，注意：没有生成新的集合 lst1 += 4 lst1.append(5) //将lst1中的元素最近到lst0中， 注意：没有生成新的集合 lst0 ++= lst1 //将lst0和lst1合并成一个新的ListBuffer 注意：生成了一个集合 val lst2= lst0 ++ lst1 //将元素追加到lst0的后面生成一个新的集合 val lst3 = lst0 :+ 5 }

**3.Set集合**

不可变的Set
import scala.collection.immutable.HashSet object ImmutSetTest { val set1 = new HashSet[Int]() //将元素和set1合并生成一个新的set，原有set不变 val set2 = set1 + 4 //set中元素不能重复 val set3 = set1 ++ Set(5, 6, 7) val set0 = Set(1,3,4) ++ set1 println(set0.getClass) }

可变的Set

import scala.collection.mutable object MutSetTest { //创建一个可变的HashSet val set1 = new mutable.HashSet[Int]() //向HashSet中添加元素 set1 += 2 //add等价于+= set1.add(4) set1 ++= Set(1,3,5) println(set1) //删除一个元素 set1 -= 5 set1.remove(2) println(set1) }

**4.Map映射**

import scala.collection.mutable object MutMapTest { val map1 = new mutable.HashMap[String, Int]() //向map中添加数据map1("spark") = 1 map1 += (("hadoop", 2)) map1.put("storm", 3) println(map1) // 取值get getOrElse() //从map中移除元素 map1 -= "spark" map1.remove("hadoop") println(map1) }

**5.元组**

Scala 元组将固定数量的项目组合在一起，以便它们可以作为一个整体传递， 与数组或列表不同，元组可以容纳不同类型的对象，但它们也是不可变的
// 定义元组 var t = (1, "hello", true) // 或者 val tuple3 = new Tuple3(1, "hello", true) // 访问tuple中的元素 println(t._2) // 访问元组总的第二个元素 // 迭代元组 t.productIterator.foreach(println) // 对偶元组 val tuple2 = (1, 3) // 交换元组的元素位置, tuple2没有变化, 生成了新的元组 val swap = tuple2.swap

元组是类型 Tuple1，Tuple2，Tuple3 等等。目前在 Scala 中只能有 22 个上限，如果您需要更多个元素，那么可以使用集合而不是元组