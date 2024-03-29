---
title: 写在前面
date: 2024-03-01
lastmod: 2024-03-02
author: ['Ysyy']
categories: ['']
tags: ['算法']
description: 
weight: None
draft: False
comments: True
showToc: True
TocOpen: True
hidemeta: False
disableShare: False
showbreadcrumbs: True
---
笔试上机题型基本是经典算法题，且难度最多leetcode hard。但面试的题目类型很多，如场景题、NP问题等没有最优解的问题。

以下对求职面试算法题做整理，主要面向ACMer，是对比赛中不常见的算法题的补充。

篇幅有限，仅给出简要思路，正解代码可选择该篇，或者自行搜索。背代码没用，经过思考后自己写一遍，面试时才能写出来。

# 经典算法题

## 链表

### 链表翻转 空间O(1) 时间O(n)

### 归并排序链表O(n) 时间O(nlogn)

## 排序

### 手写快排

### [无序数组中找第k大数](https://leetcode.cn/problems/kth-largest-element-in-an-array/)  O(n)

补充: 第K大数,而不是第K个不同的数.

和求排序后的第k个数本质一致,转换一下即可.

#### 思路

回忆一下二分法和快排:

- 二分法形成一棵二叉树. 每层所有序列长度总和为n, 二叉树高度为h, 时间复杂度为 O(n*h)

- 最优的情况: 每个结点的左儿子和右儿子序列长度相等. h = logn, 时间复杂度为O(n*logn)
- 因此快排最优是O(n*logn)

#### 如何优化到O(n)

显然, 对于求排序后第k个数. 在二分时,每次可以只选择一个儿子继续搜索.

即在最优情况下,每次二分结果为 l,mid,r

- mid == k ,答案就是a[mid]
- mid > k, 只需要继续在 (l,mid-1)中搜
- mid < k, 只需要继续在 (mid+1,r)中搜

因此与快排形成的二叉树不同. 该方法每层的搜索总长度是递减的.

即 n + n/2 + n/4 + n/8 + ... 

易得上述公式的近似为 2*n, 时间复杂度O(n)



## 串

### [字符串全排列](https://leetcode.cn/problems/zi-fu-chuan-de-pai-lie-lcof/)



### 最长回文子串-- O(n)

思路:[马拉车算法](https://blog.csdn.net/qq_51116518/article/details/117370554) 

#### 证明O(n):

即证while内的p[i]++ 执行次数总和为O(n)级别

首先考虑什么情况下才需要进入while循环

- i < mx, 且 i  为 id 所在回文串的右四等分点之后.
  - 此时p[i]是以mx-i 开始增加, 即i + p[i] >= mx
  - 也就是while内的操作每执行一次 mx++
- i > mx, 无法使用之前的预处理.
  - while内的操作没执行一次 mx++

可知while的操作次数等于 mx从0加到n-1的次数, 因此while内操作次数的总和为n

总时间复杂度 O(n)





## 动态规划(非背包)

### 最长公共连续子序列 O(nm)

### 最长上升子序列 O(nlogn)

思路:动态规划+二分



## 背包类

货币面值组成

### [砝码称重](https://www.acwing.com/problem/content/description/3420/) O(n * s)

题意：有天平和 N 个砝码重量是 Wi。可以称出多少种不同的重量？砝码可以放在天平两边。

N<100 ,Σwi < 1e5

思路：

01背包， 称重为i的可以从 abs(i-w) ， i+w 中转移。 不过要注意开个滚动数组防止重复放砝码。



## 数学题

#### 小凯的疑惑

# 思维题

#### 小球称重问题





# NP问题

## 集合覆盖问题



# 杂项

### [随机加权采样算法 alias](https://leetcode.cn/problems/random-pick-with-weight/)

https://www.cnblogs.com/Lee-yl/p/12749070.html



# 杂谈后话

写一点求职的经验和所见所闻吧！**不保证时效性和真实性，参考与否自行斟酌**。

**面评记录对求职的影响**：

- 针对人群：想**刷面试经验**，而**不是真正急着找工作的**。
- 请**珍惜每次面试机会**，尤其是面试喜欢的公司时。
- 面试一般都会有记录和面试评价。
- 所见所闻：大佬A大二时投递了理想公司的实习，意图刷该公司的面试经验。结果表现不佳，导致在真正需要找实习的时候，因之前的面评太差，导致没过简历/排序靠后（记不太清了）