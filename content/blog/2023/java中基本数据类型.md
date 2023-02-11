---
title: "java中基本数据类型"
date: 2018-05-04 16:14:22
draft: false
---
8种基本数据类型：

4种整数类型：int short long byte

2种浮点数类型：float double

1种字符类型：char

1种布尔类型：boolean

基本数据类型 大小(二进制) 字节(bit)数 默认值

int 32 4 0

short 16 2 0

long 64 8 0

byte 8 1 0

float 32 4 0.0

double 64 8 0.0

char 16 2 空

boolean 8 1 false

而每种数据类型还对应各自的封装数据类型

int-----Integer

long------Long

short------Short

byte-------Byte

float-----Float

double------Double

char-------Character

boolean-------Boolean

我们要知道为什么有了基本数据类型还要有封装数据类型？

1.某些情况下，数据必须作为对象出现，这时候就必须使用封装数据类型：比如List中要添加对象时

2使用封装类使我们可以更加方便的操作数据。比如封装类具有一些基本类型不具备的方法，比如valueOf(), toString(), 以及方便的返回各种类型数据的方法，如Integer的shortValue(), longValue(), intValue()等.

装箱和拆箱：

装箱：把基本的数据类型装换成包装类型

例如Integer i=1;在编译时会调用Integer.valueOf方法来装箱，即自动装箱

拆箱：把包装类型装换成基本数据类型

例如Integer i=1; int j=i;在编译时会调用i.intValue方法来拆箱，即自动拆箱

int和Integer的深入比较:

1.两个通过new生成的Integer对象永远不相等，因为new生成的是两个对象，内存地址不同

2.Integer和int比较时，只要两个变量的值相等，则结果为true，因为上面提到的自动装箱和拆箱

3.非new生成的Integer和new生成的Integer比较是结果为false，因为内存地址不同，非new生成的变量指向java常量池中的对象，而new生成的Integer变量指向堆中新建的对象

4.两个非new生成的Integer对象，如果值在-128到127之间，则结果为true,不在此区间则为false,java对于-128到127之间的书会进行缓存，会直接从缓存中取，始终是一个Integer对象，如果不在此区间，则会在堆中重新建立Integer对象