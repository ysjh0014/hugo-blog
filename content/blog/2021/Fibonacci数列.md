---
title: "Fibonacci数列"
date: 2021-01-01 17:59:09
draft: false
tags:
- leetcode
- 算法题
categories: 
- leetcode
- 算法题
series:
- LeetCode题解
---
**来源：**

[牛客网2017校招真题](https://www.nowcoder.com/ta/2017test)

**题目描述：**

Fibonacci数列是这样定义的：
F[0] = 0
F[1] = 1
for each i ≥ 2: F[i] = F[i-1] + F[i-2]
因此，Fibonacci数列就形如：0, 1, 1, 2, 3, 5, 8, 13, ...，在Fibonacci数列中的数我们称为Fibonacci数。给你一个N，你想让其变为一个Fibonacci数，每一步你可以把当前数字X变为X-1或者X+1，现在给你一个数N求最少需要多少步可以变为Fibonacci数。

**输入描述：**
输入为一个正整数N1 ≤ N ≤ 1,000,000

**输出描述：**

输出一个最小的步数变为Fibonacci数"

**示例**

输入：
15

输出：

2

**思路：**

给定一个数将这个数每步加一或者减一使其变为一个斐波那契数，求最小步数，思路很简单，就是把给定的这个数在斐波那契数列中比较，看它在那两个数中间，然后看这两个数哪一个与它的差值最小，就是所需的最小步数

**代码：**
import java.io.BufferedReader; import java.io.IOException; import java.io.InputStreamReader; public class Main { public static void main(String []args) throws IOException { BufferedReader br = new BufferedReader(new InputStreamReader(System.in)); String input = br.readLine(); while (input != null) { int a = Integer.valueOf(input); int b = 1, c = 1, d; while (true) { d = b + c; b = c; c=d; if (d > a) { break; } } if (d - a < a - b) { System.out.println(d-a); return; } else { System.out.println(a-b); return; } } } }