---
title: "Java中的类加载过程"
date: 2020-03-23 11:05:32
draft: false
featured_image: "https://hugo-ys.oss-cn-hangzhou.aliyuncs.com/static/img/java.png"
tags:
- Java
categories: 
- Java
---
**1.类加载机制**

虚拟机把描述类的数据从class文件加载到内存，并对数据进行校验、转换、解析、初始化，最终形成可以被虚拟机直接使用的Java类型，这就是虚拟机的类加载机制

Java是使用双亲委派模型来进行类加载的，这样能够有效确保一个类的全局唯一性，当程序中出现多个限定名相同的类时，类加载器在执行加载时，始终只会加载其中的某一个类

双亲委派模型工作过程可以参考我的博文：

[Java面试题之JVM](https://blog.csdn.net/ys_230014/article/details/88577648)

**2.类加载步骤**

**加载**

在加载过程会完成3件事情：

通过一个类的全限定名获取该类的二进制流

将该二进制流中的静态存储结构转化为方法区运行时数据结构

在内存中生成该类的class对象，作为该类的数据访问入口

**验证**

验证类数据信息是否符合JVM规范，是否是一个有效的字节码文件，主要完成下面4种验证

文件格式验证：验证字节流是否符合class文件的规范，如主次版本号是否在当前虚拟机范围内，常量池中的常量是否有不被支持的类型

元数据验证：对字节码描述的信息进行语义分析，如这个类是否有父类，是否继承了不被继承的类等

字节码验证：是整个验证过程中最复杂的一个阶段，通过验证数据流和控制流的分析，确定程序语义是否正确，主要针对方法体的验证，如方法中的类型转换是否正确，跳转指令是否正确等

符号引用验证：这个在后面的解析过程中发生，主要是为了确保解析动作能正确执行

**准备**

准备阶段是为类的静态变量分配内存并将其初始化为默认值，这些内存都将在方法区中进行分配。准备阶段不分配类中的实例变量的内存，实例变量将会在对象实例化时随着对象一起分配在Java堆中

**解析**

该阶段主要完成符号引用到直接引用的转换动作。解析动作并不一定在初始化动作完成之前，也可能在初始化之后

**注意：**

验证 准备 解析阶段又合称为链接阶段，链接阶段要做的是将加载到JVM中的二进制字节流的类数据信息合并到JVM的运行时状态中

**初始化**

初始化阶段是类加载过程的最后一步 , 前面的几个阶段, 除了在加载阶段用户应用程序可以通过自定义类加载器參与之外, 其余动作完全由虚拟机主导和控制。到了初始化阶段, 才真正开始执行类中定义的 Java程序代码

为静态变量赋值，执行static代码块，static代码块只有JVM能够调用，如果是多线程需要同时初始化一个类，仅仅只能允许其中一个线程对其执行初始化操作，其它线程必须等待

**3.类加载器**

实现通过类的权限定名获取该类的二进制字节流的代码块叫做类加载器

启动类加载器：用来加载java核心类库，无法被Java程序直接引用

扩展类加载器：用来加载Java的扩展库。Java虚拟机的实现会提供一个扩展库目录，该类加载器在此目录中查找并加载Java类

系统类加载器：它根据Java应用的类路径来加载Java类。一般来说，Java应用的类都是由它来加载的，可以通过ClassLoader.getSystemClassLoader()来获取它

用户自定义类加载器：通过继承java.lang.ClassLoader类的方式实现