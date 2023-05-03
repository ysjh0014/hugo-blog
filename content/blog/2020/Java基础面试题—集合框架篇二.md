---
title: "Java基础面试题—集合框架篇二"
date: 2020-03-04 16:41:23
draft: false
featured_image: "https://hugo-ys.oss-cn-hangzhou.aliyuncs.com/static/img/java.png"
tags:
- Java
categories: 
- Java
---
**11.ArrayList和LinkedList的比较**

（1）ArrayList是实现了基于动态数组的数据结构，因为地址连续，一旦数据存储好了，查询操作效率会比较高(在内存里是连着放的)

（2）因为地址连续，ArrayList要移动数据，所以插入和删除操作效率比较低

（3）LinkedList是基于链表的数据结构，地址是任意的，所以在开辟内存空间的时候不需要等一个连续的地址，对于新增和删除操作，LinkedList效率比较高

（4）因为LinkedList要移动指针，所以查询操作性能比较低

当需要对数据进行多次访问的时候选用ArrayList，当需要对数据进行多次增加删除修改时采用LinkedList

**12.HashSet与TreeSet的比较**

（1）TreeSet是二叉树实现的，TreeSet中的数据是自动排好序的，不允许放入null值

（2）HashSet是哈希表实现的，HashSet中的数据是无序的，可以放入null值，但只能放入一个null值，两者中的值都不能重复，就如数据库中唯一约束

（3）HashSet要求放入的对象必须实现hashcode()方法，放入的对象是以hashcode码作为标识的，而具有相同内容的String对象，hashcode是一样的，所以放入的内容不能重复，但是同一个类的对象可以放入不同的实例

HashSet是基于hash算法实现的，其性能通常都优于TreeSet。所以我们通常都使用HashSet，在我们需要排序的功能时，才使用TreeSet

**13.HashMap和ConcurrentHashMap的区别**

（1）HashMap不是线程安全的，ConcurrentHashMap是线程安全的

（2）ConcurrentHashMap采用锁分段技术，将整个Hash桶进行了分段segment，也就是将这个大的数组分成了几个小的片段segment，而且每个小的片段segment上面都有锁存在，那么在插入元素的时候就需要先找到应该插入到哪一个片段segment，然后再在这个片段上进行插入，而且这里还需要获得segment锁

（3）ConcurrentHashMap让锁的粒度更精细一些，并发性能更好

**14.HashTable和ConcurrentHashMap的区别**

它们都可以用作多线程的环境，但是当HashTable的大小增加到一定程度时，性能会急剧下降，因为迭代是要被锁定很长的时间。因为ConcurrentHashMap引入了分割(segmentation)，不论它变得多大，仅仅需要锁定map的某个部分，而其它的线程不需要等到迭代完成才能访问map。简而言之，在迭代的过程中，ConcurrentHashMap仅仅锁定map的某个部分，而HashTable是锁定整个map