---
title: "Scala基础语法四"
date: 2018-10-04 19:08:34
draft: false
---
**1.可变参数函数**
def ss(a: Int/*) = { for (p <- a) { println(p) } }

可变参数一般跟在所有参数的最后

**2.默认参数值函数**
def add(a: Int = 1, b: Int = 7): Unit = { println(s"a + b = ${a + b}") }

默认参数值函数调用时，如果不传递参数，则会使用函数或者方法的的默认参数值，如果传递了参数值，则使用传递的参数值，会自动覆盖默认的参数值

**3.高阶函数**

高阶函数： 将其他函数作为参数或其结果是函数的函数

示例：

定义一个方法, 参数为带一个整型参数返回值为整型的函数 f 和 一个整型参数 v, 返回值为一个函数
def apply(f: Int => String, v: Int) = f(v)

定义一个方法, 参数为一个整型参数, 返回值为 String

def layout(x: Int) = "[" + x.toString() + "]"

调用

println(apply(layout, 10))

输出结果：

![](https://img-blog.csdn.net/20181004183249281?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

**4.部分参数应用函数**

如果函数传递所有预期的参数，则表示已完全应用它， 如果只传递几个参数并不是全部参数，那么将返回部分应用的函数。这样就可以方便地绑定一些参数，其余的参数可稍后填写补上

示例：
def log(date: Date, message: String) = { println(s"$date, $message") } val date = new Date() val logBoundDate: (String) => Unit = log(date, _: String) def main(args: Array[String]): Unit = { logBoundDate("fuck jerry ") logBoundDate("fuck 涛涛 ") }

上边的代码中首先定义个输出的方法, 参数为 date， message，然后logBoundDate调用 log 的时候， 传递了一个具体的时间参数， message 为待定参数，因此logBoundDate 成了一个新的函数,，只有 log 的部分参数(message)，调用 logBoundDate 的时候, 只需要传递待传的 message 参数即可

最终做到两次赋值给message时的date都完全一样

![](https://img-blog.csdn.net/20181004190818963?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)