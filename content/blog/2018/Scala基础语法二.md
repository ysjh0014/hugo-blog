---
title: "Scala基础语法二"
date: 2018-10-04 11:24:24
draft: false
---
**1.条件表达式**
var test = if (age > 10) "ysjh" else "ys" print("test=" + test)

输出结果：

![](https://img-blog.csdn.net/20181004091625919?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

这里将条件表达式的返回值赋给了变量test，如果出现没有返回值的情况，即不符合条件表达式的条件，则编译器默认返回Unit，如果有多行数据，最后一行的值作为条件表达式的返回值

if....else if.....else if.......较多时，可以使用代码块{ }
val score = 76 val res4 = { if(score > 60 && score < 70) "及格" else if(score >=70 && score < 80) "良好" else "优秀" }

**2.循环语句**

1)遍历打印出数组
val test = Array(1, 2, 3, 4, 5, 6) for (ys <- test) { print(ys) }

2)通过下标获取数组中的元素

for (i <- 0 to 2) { print(test(i)) } for (i <- 0 until 2) { print(test(i)) }

注意： to 和 until 的区别就是 to 包含为前后都为闭区间, until 为前闭后开区间

3)输出数组中的偶数
for(ys <- test if ys % 2 == 0) { println(ys) }

for循环中可以增加守卫

4)双重for循环
for (i <- 1 to 3; j <- 1 to 3 if i<=j) { println(i+"/*"+j+"="+(i /* j) + " ") }

输出结果：

![](https://img-blog.csdn.net/20181004110258298?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

5)yield关键字

yield关键字会产生一个新的集合

![](https://img-blog.csdn.net/20181004111806263?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

**3.运算符**

Scala中与Java一样也有运算符，只不过这些运算符实际上都是方法，例如：

1+2 实际上是： 1.+(2)

a 方法 b 可以写成 a.方法(b)