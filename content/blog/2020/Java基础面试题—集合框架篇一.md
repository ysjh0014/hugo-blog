---
title: "Java基础面试题—集合框架篇一"
date: 2020-03-03 21:06:29
draft: false
---
**1.Java集合框架是什么？说出一些集合框架的优点**

每种编程语言中都有集合，最初的Java版本包含几种集合类：Vector、Stack、HashTable和Array。随着集合的广泛使用，Java1.2提出了囊括所有集合接口、实现和算法的集合框架。在保证线程安全的情况下使用泛型和并发集合类，Java已经经历了很久。它还包括在Java并发包中，阻塞接口以及它们的实现。集合框架的部分优点如下：

（1）使用核心集合类降低开发成本，而非实现我们自己的集合类

（2）随着使用经过严格测试的集合框架类，代码质量会得到提高

（3）通过使用JDK附带的集合类，可以降低代码维护成本

（4）复用性和可操作性

**2.ArrayList和Vector的异同点**

相同点：

这两个类都实现了List接口(List接口继承了Collection接口)，它们都是有序集合，即存储在这两个集合中的元素的位置都是有序的，相当于一种动态的数组，我们以后可以按位置索引号取出某个元素，并且其中的数据是允许重复的，这是与HashSet之类的集合最大的不同之处，HashSet之类的集合不可以按索引号去检索其中的元素，也不允许有重复的元素

不同点：

(1)同步性

Vector是线程安全的，也就是说它的方法是线程同步的，而ArrayList是线程不安全的，它的方法之间是线程不同步的。如果只有一个线程会访问到集合，最好使用ArrayList，因为它不考虑线程安全，效率会高一些；如果有多个线程会访问到集合，最好使用Vector，因为不需要我们自己再去考虑和编写线程安全的代码

(2)数据增长

ArrayList和Vector都有一个初始的容量大小，当存储进它们里面的元素个数超过了容量时，就需要增加ArrayList和Vector的存储空间，不是只增加一个存储单元，而是增加多个存储单元，每次增加的存储单元的个数在内存空间利用与程序效率之间要取得一定的平衡。Vector默认增长为原来的两倍，而ArrayList的增长策略在文档中没有明确规定(从源代码看到的是增长为原来的1.5倍)。ArrayList与Vector都可以设置初始的空间大小，Vector还可以设置增长的空间大小，而ArrayList没有提供设置增长空间的方法

**3.HashMap和HashTable的区别**

(1)HashMap是非线程安全的，HashTable是线程安全的，HashMap是HashTable的轻量级实现，它们都实现了Map接口

(2)HashMap的键和值都允许有null值存在，而HashTable则不行

(3)因为线程安全的问题，HashMap要比HashTable效率高

(4)HashMap不是同步的，所以更适合于单线程环境，而HashTable适合于多线程环境

但是一般现在不建议用HashTable，因为：

1.HashTable是遗留类，内部实现很多没有优化和冗余

2.即使在多线程环境下，现在也有同步的ConcurrentHashMap替代，所以没有必要因为是多线程而使用HashTable

**4.List和Map的区别**

List是存储单列数据的集合，Map是存储键值这样双列数据的集合，List中存储的数据是有序的，并且允许重复，Map中存储的数据是没有顺序的，其键是不能重复的，但是它的值是可以有重复的

**5.List、Set、Map是否继承自Collection接口？**

List和Set是，Map不是

**6.List、Set、Map三个接口，存取元素时，各有什么特点？**

首先，List和Set具有相似性，它们都是单列元素的集合，所以它们有一个共同的父接口，Collection。Set里面不允许有重复的元素，即不能有两个相等，注意：这个不仅仅是相同，而是两个元素的equals相等，就不能存储。Set取元素时，不能细说要取第几个，只能以Iterator接口取得所有的元素，再逐一遍历各个元素

List表示有先后顺序的集合，注意：这里的顺序不是按年龄、大小、价格之类的排序。当我们多次调用add(Object)方法时，每次加入的对象就像火车站买票有排队顺序一样，按先来后到的顺序排序，有时候也可以插队，即调用add(intindex，Object)方法，就可以指定当前对象在集合中的存放位置。一个对象可以被反复存储进List中，每调用一次add方法，这个对象就被插入进集合中一次，并不是把这个对象本身存储进集合中，而是在集合中用一个索引变量指向这个对象，当这个对象被add多次时，即相当于有集合中有多个索引指向了这个对象，List除了可以用Iterator接口取得所有的元素，再逐一遍历各个元素之外，还可以调用get(index)来明确说明取第几个

Map和List、Set不同，它是双列的集合，其中有put(Object key，Object value)方法，每次存储时要存储一对key-value，不能存储重复的key，这里的重复也是按照equals比较的，Map可以根据key获取相应的value，即get(Object key)，返回值为key所对应的value，另外，也可以获得所有的key的结合，所有value的结合，还可以获得key和value组合成的Map.Entry对象的集合

List中有先后顺序，也可以有重复的元素。Set不能有重复元素，内部排序，Map保存key-value键值对，value可以重复

**6.为什么Map接口不继承Collection接口？**

尽管Map集合和它的实现也是集合框架的一部分，但是Map不是集合，集合也不是Map。因此Map继承Collection没有意义。反之亦然，如果Map继承Collection接口，那么元素去哪？Map存储key-value对，它提供抽取key或者value列表集合的方法，但是它不适合"一组对象"规范

**7.去掉一个Vector集合中重复的元素**
Vector newVector = new Vector(); For (int i=0;i<vector.size();i++) { Object obj = vector.get(i); if(!newVector.contains(obj); newVector.add(obj); }

还有一种简单的方法，利用了Set不允许有重复元素：

*HashSet set = new HashSet(vector)；*

**8.Collection和Collections的区别**

Collection是集合类的上级接口，继承它的接口主要有Set和List

Collections是针对集合类的一个帮助类，他提供一系列静态方法实现对各种集合的搜索、排序、线程安全化等操作

**9.ArrayList、Vector、LinkedList的存储性能和特性**

ArrayList和Vector都是使用数组方式存储数据，数组元素数大于实际存储的数据数以便增加和插入元素，他们都允许直接按序号索引元素，但是插入元素要涉及数组元素移动等内存操作，所以索引数据快，而插入数据慢，Vector由于使用了synchronized方法(线程安全)，通常性能上较ArrayList差。LinkedList使用双向链表实现存储，按序号索引数据需要进行前向或后向遍历，索引就变慢了，但是插入数据时只需要记录本项的前后项即可，所以插入数据速度较快

LinkedList也是线程不安全的，LinkedList提供了一些方法，使得LinkedList可以被当作堆栈来使用

**10.Array和ArrayList有何区别？**

存储内容：

Array数组可以容纳基本类型和对象，而ArrayList只能容纳对象

Array只能存储相同数据类型的数据，而ArrayList可以存储不同数据类型的数据

空间大小：

Array的长度是固定的，而ArrayList的长度是可变的

方法上：

ArrayList方法上比Array更多样化