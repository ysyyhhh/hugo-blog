---
title: 程序分析
date: 2023-04-19
lastmod: 2024-03-02
author: ['Ysyy']
categories: ['']
tags: ['软件测试']
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
- **程序优化**侧重于提高程序的性能，通过对程序中关键函数的跟踪或者运行时信息的统计，找到系统性能的瓶颈，从而采取进一步行动对程序进行优化，同时减少资源使用。
- **程序正确性**侧重于确保程序执行它应该做的事情，帮助开发者找出错误代码的位置。（本文以程序正确性的分析为主）

### 程序分析方法：

第一类是**静态程序分析**，即在不执行程序的情况下进行程序分析。

第二类是**动态程序分析**，即通过运行程序或者在程序运行期间进行分析。

[动态分析方法包括：调试、覆盖测试、剖面测试、动态切片、动态污点分析等](https://wiki.mbalib.com/wiki/动态分析)[1](https://wiki.mbalib.com/wiki/动态分析)。

当然，也有很多研究工作是关于**如何有效结合静态和动态程序分析**的。同时，因为通常无法拿到真正的程序正确性的需求，绝大多数的程序分析技术着重于分析**通用的程序正确性需求**，比如如果有断言的话，我们尽量分析断言会不会被违背，再比如分析是否存在整数或者缓存溢出，再或者检测指针相关的安全漏洞等。

**符号执行**（通过用求解每条程序路上上的条件来生成测试用例）

**模型检测**（通过抽象并遍历所有的程序行为来判断程序是不是正确）

**模糊测试**（通过优化大量的生成测试用例）

[模型检查、符号执行、抽象解释等](https://zhuanlan.zhihu.com/p/396531255)[1](https://zhuanlan.zhihu.com/p/396531255)。

对基于静态分析（比如抽象解释，或者 lint）的工具，一个重要的问题就是**如何减少假警报**的。

而对于动态分析（比如测试）而言，对应的问题就是**如何减少漏报**。

除了把静态分析做的更精确（比如设计更复杂的 lint 规则），和把动态分析做的更完备（比如提要求更高的覆盖率标准）

还有一个趋势，就是**结合不同的程序分析技术取长补短**。比如 hybrid fuzzing 的做法是，通过有效的结合符号执行与模糊测试来提高测试的覆盖率。

[插桩、覆盖率、动态切片、动态污点分析等。](https://zhuanlan.zhihu.com/p/396531255)[1](https://zhuanlan.zhihu.com/p/396531255)



[技术分享 | 浅谈程序分析](https://zhuanlan.zhihu.com/p/396531255)