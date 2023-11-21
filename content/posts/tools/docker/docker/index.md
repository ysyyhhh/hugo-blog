---
title: docker Usage
date: 2023-10-11
lastmod: 2023-11-21
author: ['Ysyy']
categories: ['']
tags: ['docker']
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
## 多阶段构建docker镜像

多阶段构建的修改不会保留到下一阶段，只有COPY和ADD命令会保留到下一阶段

usages：
- 第一阶段：编译/打包程序依赖

多阶段用途：
- 缩小镜像体积
-