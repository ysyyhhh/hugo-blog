---
title: 笔试算法题
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
1.已知平面上的一个圆和若干点 快速求出包含点数最少的多边形使得圆在多边形中

预处理：圆内的点删去

点排序，按照射线的角度排序。O(nlogn)

如黑色的三个点要排序，就是按与圆相切的射线的角度排序。

![image-20230414155250711](笔试算法题/img/image-20230414155250711.png)

把每一个点当作起点贪心。

每次贪心：

​	从一个S出发，选择一个点T ，点T角度最大，且满足ST与圆不相交 且 圆心在ST射线的右侧。（即顺时针）然后再以T点出发，选择下一个点，直到遍历过的角度大于360度。每次选择 logn，最多选择n次，因此每次贪心是nlogn。

做n次贪心，总时间复杂度是n^2logn

给出一无序数组 求所有长度大于等于k的连续子序列的中位数的最大值 定义中位数为第[l/2](向上取整)(l为数组长度)个数

二分答案，从小到大排序后二分，选择X为中位数。判断是否有大于等于X的中位数满足条件。

预处理数组为 -1 ， 0 ， 1（小于X，等于X，大于X），On跑一遍找是否有长度大于K的和大于等于0。 求前缀和并维护最小前缀和即可。