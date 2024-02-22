---
title: Modern C++
date: 2023-11-05
lastmod: 2024-02-22
author: ['Ysyy']
categories: ['']
tags: ['c++']
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
## lambda表达式

描述:

- 一个匿名函数对象
- 一个可调用的代码单元
- 一个函数对象的语法糖

语法规则:
[](){};  
[]: lambda表达式的引导符
(): 参数列表
{}: 函数体

具体示例

```C++
for (auto &thread: threads) {
 thread = std::thread([&taskId, num_total_tasks, runnable] {
  for (int id = taskId++; id < num_total_tasks; id = taskId++)
   runnable->runTask(id, num_total_tasks);
 });
}
```

在示例中, thread的初始化是一个lambda表达式, 该lambda表达式的参数列表为空, 函数体为

```C++
{
  for (int id = taskId++; id < num_total_tasks; id = taskId++)
   runnable->runTask(id, num_total_tasks);
 }
```

这样一个thread可以负载多个任务.

如果不用lambda表达式, 那么就需要定义一个函数, 然后将函数的地址传递给thread, 这样就会增加代码量.

也就是说实际上 thread的参数可以是一个函数对象, 也可以是一个函数指针, 也可以是一个lambda表达式.