---
title: "Scala基础语法三"
date: 2018-10-04 17:03:03
draft: false
---
**1.方法的定义与调用**

1)方法的定义与调用

def 方法名(参数：参数类型，。。。。) ：方法返回值类型 = {方法体}

当然上边的方法定义的方式中有许多是可以省略的，例如：

省略方法的返回值类型

![](https://img-blog.csdn.net/20181004131156804?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

注意： 方法的返回值类型可以不写，编译器可以自动推断出来，但是对于递归函数，必须指定返回

省略传入方法中的参数

![](https://img-blog.csdn.net/20181004131544432?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

![](https://img-blog.csdn.net/20181004131628242?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

注意： 如上图中的例子所示，省略了参数之后可以写括号也可以不写括号，但是定义方法时写了括号，在调用该方法时写不写括号都可以调用，而在定义方法时没有写括号，则在调用该方法时就不能写括号，否则会报错

2)方法转变为函数

方法名 _

![](https://img-blog.csdn.net/20181004133847668?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

解读： (Int，Int，Int)=>Int=<function3>

左边是该函数的参数列表，中间是该函数的返回值类型，右边该函数有几个参数就是function几

注意： 方法名与_之间是有空格的

**2.函数的定义与调用**

函数定义方式一：

![](https://img-blog.csdn.net/20181004135042845?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

调用： f1(2) ， 其中 f1 为函数的引用, 也可以叫做函数名， function1 表示一个参数的函数

函数定义方式二：

![](https://img-blog.csdn.net/20181004143144664?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

没有任何参数的函数，函数的返回值为 Int 类型
var ys:()=>Int=()=>1

**3.传值调用与传名调用**

首先来一段代码示例：
object NumTest { var money = 100 def huaQian = { money = money - 10 } def shuQian = { huaQian money } def test(x：Int) = { for (s <- 0 to 3) println(s"剩余${x}元") } def test1(x: => Int) = { for (s <- 0 to 3) println(s"剩余${x}元") } def main(args: Array[String]): Unit = { test(shuQian) test1(shuQian) } }

输出结果：

![](https://img-blog.csdn.net/20181004165527846?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

上述代码中test方法中的调用就是传值调用，test1方法的调用就是传名调用，传值调用在调用前就是一个固定的值，调用的时候必须要传入一个值，而test1则是传入一个函数，这里的shuQian虽然是一个方法，但是传入的时候已经隐式的转变为函数了