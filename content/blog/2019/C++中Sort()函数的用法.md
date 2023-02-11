---
title: "C++中Sort()函数的用法"
date: 2019-01-19 17:05:20
draft: false
---
Sort函数是C++中排序方法的一种，C语言中有qsort函数，Sort函数的头文件为：
/#include <algorithm>

Sort函数是使用快速排序并结合插入排序和堆排序来实现的，具体可以参看我的文章：STL中sort用的是什么排序算法

Sort函数的用法：

1.sort(begin,end) 默认为升序，从小到大排序，时间复杂度为nlog(n)

代码实现如下：
/#include<iostream> /#include<algorithm> using namespace std; int main() { int a[10]={9,6,3,8,5,2,7,4,1,0}; for(int i=0;i<10;i++) cout<<a[i]<<" "; sort(a,a+10); for(int i=0;i<10;i++) cout<<a[i]<<" "; return 0; }

2.从上边的例子中可以看出，默认升序的时候是两个参数，如果要随意定义升序和降序的话，就需要添加第三个参数，有两种实现方法

第一种方法：

加入一个比较函数complare()，然后使用sort(begin,end,complare)，代码实现如下：
/#include<iostream> /#include<algorithm> using namespace std; bool complare(int a,int b) { return a>b; } int main() { int a[10]={9,6,3,8,5,2,7,4,1,0}; for(int i=0;i<10;i++) cout<<a[i]<<" "; sort(a,a+10,complare); for(int i=0;i<10;i++) cout<<a[i]<<" "; return 0; }

第二种方法：

第一种方法虽然实现了随意定义升序和降序，但是却需要加入比较函数，比较麻烦，第二种方法可以直接加入第三个参数来告诉函数是升序还是降序

从小到大排序：less<数据类型>()

从大到小排序：greater<数据类型>()

代码实现：
/#include<iostream> /#include<algorithm> using namespace std; int main() { int a[10]={9,6,3,8,5,2,7,4,1,0}; sort(a,a+10,less<int>()); for(int i=0;i<10;i++) cout<<a[i]<<" "; return 0; }