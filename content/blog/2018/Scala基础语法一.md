---
title: "Scala基础语法一"
date: 2018-10-04 08:51:34
draft: false
---
**1.数据类型**

Scala的数据类型和Java差不多

有 7 种数值类型 Byte、Char、Short、Int、Long、Float 和 Double（无包装类型）和 Boolean、Unit 类型

Scala的继承层级如图所示：

![](https://img-blog.csdn.net/20181003224937952?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

这里的Any相当于Java中的Object

注意：

Unit 表示无值，和Java中的void 等同，用作不返回任何结果的方法的结果类型，而在Scala中Unit它的形式是()，例如在Scala语法中的mian函数定义：
def main(args: Array[String]): Unit = { }

**2.变量的定义**

定义变量使用 var 或者 val 关键字

语法： var|val 变量名称 (： 数据类型) = 变量值
var name="zhangsan" val age=19 val sex="男"

注意：

使用 val 修饰的变量， 值不能为修改， 相当于 java 中 final 修饰的变量

使用 var 修饰的变量，值可以修改

定义变量时, 可以指定数据类型, 也可以不指定, 不指定时编译器会自动推测变量的数据类型

**3.字符串的格式化输出**

1)普通输出
println("name="+name,"age="+age) println("name="+name + "age="+age)

输出结果：

![](https://img-blog.csdn.net/20181004084433306?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

注意： 这里的用逗号和不用逗号是不一样的，用逗号输出结果会多一个括号

2)文字'f'插值器允许创建一个格式化的字符串，类似于 C 语言中的 printf，在使用'f'插值器时，所有变量引用都应该是 printf 样式格式说明符，如％d，％i，％f 等
print(f"姓名 $name%s 年龄 $age%1.0f")

输出结果：

![](https://img-blog.csdn.net/20181004084546607?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

3)'s'允许在处理字符串时直接使用变量，在 println 语句中将 String 变量($name)附加到普通字符串中
print(s"name=$name,age=$age,url=$url")

输出结果：

![](https://img-blog.csdn.net/20181004084734739?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

4)字符串插入器还可以处理任意表达式
print(s"1+1=${1+1}")

输出结果：

![](https://img-blog.csdn.net/20181004084900579?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)