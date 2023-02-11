---
title: "二分查找(Java版)"
date: 2018-12-21 17:54:52
draft: false
---
**二分查找：**

二分查找又称折半查找，它是一种效率较高的查找方法

**实现原理：**

通俗点来讲就是将要查找部分对折，然后分为三部分：中间部分，中间前的部分，中间后的部分，将要查找的和中间部分进行比较，如果小于中间部分则在中间部分的前面找，否则就在中间部分的后边找，然后再重复上边折半的过程

**使用要求或者前提：**

通过实现原理可以看出，要想使用二分查找必须满足一定条件或者前提，即查找序列必须是顺序结构和有序的

**二分查找的优缺点：**

优点是比较次数少，查找速度快，平均性能好

其缺点是要求待查表为有序表，且插入删除困难

因此，折半查找方法适用于不经常变动而查找频繁的有序列表

**代码实现：**

1.非递归实现
public static int test1(int [] arr,int value){ int low=0,high=arr.length-1; if(value<arr[low]||value>arr[high]||low>high){ return -1; } while (low<=high){ int mid=(low+high)/2; if(arr[mid]>value){ high=mid-1; } if(arr[mid]<value){ low=mid+1; } if(arr[mid]==value){ return mid; } } return -1; }

2.递归实现

public static int test(int[] arr,int value,int low,int high){ if(value<arr[low]||value>arr[high]||low>high){ return -1; } int mid=(low+high)/2; if(arr[mid]>value){ return test(arr,value,low,mid-1); } if(arr[mid]<value){ return test(arr,value,mid+1,high); } return mid; }

3.测试

public static void main(String[] args) { int [] A={1,3,5,7,9,10,20,34}; System.out.println(test(A,34,0,8)); System.out.println(test1(A,1)); }