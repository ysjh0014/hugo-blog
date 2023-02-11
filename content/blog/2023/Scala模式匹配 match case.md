---
title: "Scala模式匹配 match case"
date: 2018-10-05 16:39:32
draft: false
---
**1.匹配字符串/类型/守卫**
val arr = Array("YoshizawaAkiho", "YuiHatano", "AoiSola") val i = Random.nextInt(arr.length) println(i) val name = arr(i) println(name) name match { case "YoshizawaAkiho" => println("吉泽老师...") case "YuiHatano" => { println("波多老师...") } case _ => println("真不知道你们在说什么...") } //定义一个数组 val arr:Array[Any] = Array("hello123", 1, 2.0, CaseDemo02, 2L) //取出一个元素 val elem = arr(3) elem match { case x: Int => println("Int " + x) case y: Double if(y >= 0) => println("Double "+ y) // if 守卫 case z: String => println("String " + z) case w: Long => println("long " + w) case CaseDemo02 => { println("case demo 2") } case _ => { // 其他任意情况 println("no") println("default") } }

**2.匹配数组**

示例：
val arr = Array(1, 1, 7, 0, 2,3) arr match { case Array(0, 2, x, y) => println(x + " " + y) case Array(2, 1, 7, y) => println("only 0 " + y) case Array(1, 1, 7, _/*) => println("0 ...") // _/* 任意多个 case _ => println("something else") }

**3.匹配集合**

val lst = List(0, 3, 4) println(lst.head) println(lst.tail) lst match { case 0 :: Nil => println("only 0") case x :: y :: Nil => println(s"x $x y $y") case 0 :: a => println(s"value : $a") case _ => println("something else") }

**4.匹配元组**

val tup = (1, 3, 7) tup match { case (3, x, y) => println(s"hello 123 $x , $y") case (z, x, y) => println(s"$z, $x , $y") case (_, w, 5) => println(w) case _ => println("else") }

**5.匹配样例/样例对象**

//样例类，模式匹配，封装数据（多例）,不用 new 即可创建实例 case class SubmitTask(id: String, name: String) case class HeartBeat(time: Long) //样例对象，模式匹配（单例） case object CheckTimeOutTask val arr = Array(CheckTimeOutTask, new HeartBeat(123), HeartBeat(88888), new HeartBeat(666), SubmitTask("0001", "task-0001")) val i = Random.nextInt(arr.length) val element = arr(i) println(element) element match { case SubmitTask(id, name) => { println(s"$id, $name") } case HeartBeat(time) => { println(time) } case CheckTimeOutTask => { println("check") } }