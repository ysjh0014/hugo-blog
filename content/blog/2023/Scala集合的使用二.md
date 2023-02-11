---
title: "Scala集合的使用二"
date: 2018-10-05 12:06:26
draft: false
---
**1.集合常用的方法**

map， flatten， flatMap，filter， sorted， sortBy，sortWith， grouped

fold(折叠)，foldLeft， foldRight，reduce， reduceLeft， aggregate，union，

intersect(交集)，diff(差集)， head， tail， zip，mkString， foreach， length，slice， sum

**2.并行化集合par**

调用集合的 par 方法， 会将集合转换成并行化集合
//创建一个 List val lst0 = List(1,7,9,8,0,3,5,4,6,2) //折叠：有初始值（无特定顺序） val lst11 = lst0.par.fold(100)((x, y) => x + y) //折叠：有初始值（有特定顺序） val lst12 = lst0.foldLeft(100)((x, y) => x + y) //聚合 val arr = List(List(1, 2, 3), List(3, 4, 5), List(2), List(0)) val result = arr.aggregate(0)(_+_.sum, _+_)

**3.Map和Option**

在 Scala 中 Option 类型样例类用来表示可能存在或也可能不存在的值(Option 的子类有 Some和 None)，Some 包装了某个值，None 表示没有值
// Option 是 Some 和 None 的父类 // Some 代表有（多例），样例类 // None 代表没有（单例），样例对象 val mp = Map("a" -> 1, "b" -> 2, "c" -> 3) val r: Int = mp("d") // Map 的 get 方法返回的为 Option, 也就意味着 rv 可能取到也有可能没取到 val rv: Option[Int] = mp.get("d") // 如果 rv=None 时， 会出现异常情况 val r1 = rv.get // 使用 getOrElse 方法， // 第一个参数为要获取的 key, // 第二个参数为默认值， 如果没有获取到 key 对应的值就返回默认值 val r2 = mp.getOrElse("d", -1) println(r2)