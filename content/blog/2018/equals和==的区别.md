---
title: "equals和==的区别"
date: 2018-05-05 15:46:09
draft: false
---
==用来判断两个变量的值是否相等，变量分为基本数据类型变量和引用类型变量，基本数据类型变量

直接比较两个变量的值，引用数据类型变量要比较引用类型的内存的地址

![](https://img-blog.csdn.net/20180505150056819)

equals用来比较两个对象，Object类中的equals方法是比较两个对象的地址，而对于String、Date、Integer之类的equals方法已经被重写了，比较时是看两个值是否相等，对于没有重写equals方法的对象，如果需要重写，equals方法和hashcode方法必须同时重写才能达到比较的是值而不是地址的目的，因为重写了equals方法使其相等条件是values值相等，但是hashcode值不等，最终结果依然是false，并且equals相等，hashcode不相等也不符合hashcode的规则(一般equals相等，hashcode也要相等)，这会导致严重的问题

这里提供一个重写的实例代码:

package cn.ysjh;
public class shiyan {
private int i;
private String name;
public String getName() {
return name;
}
public void setName(String name) {
this.name = name;
}
public int getI() {
return i;
}
public void setI(int i) {
this.i = i;
}
public boolean equals(Object o) {
if (o == this) return true;
if (!(o instanceof shiyan)) {
return false;
}
shiyan user = (shiyan) o;
return user.i==i&&user.name.equals(name);
}
public int hashCode() {
int result = 17;
result = 31 /* result + name.hashCode();
result = 31 /* result + i;
return result;
}
public shiyan(String name) {
super();
this.name = name;
}

}

![](https://img-blog.csdn.net/20180505154429873)

没有重写shiyan类的equals和hashcode方法前，上图中的结果为false，因为两个对象的地址不同，重写之后现在结果为true