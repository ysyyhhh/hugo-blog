<<<<<<< HEAD

# docker Usage

=======
---
title: docker Usage
date: 2023-10-11
lastmod: 2023-10-24
author: ['Ysyy']
categories: ['tools']
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
>>>>>>> 058a5313c3c0d23db51c00c464baee19dff9ec56
## 多阶段构建docker镜像

多阶段构建的修改不会保留到下一阶段，只有COPY和ADD命令会保留到下一阶段

usages：
- 第一阶段：编译/打包程序依赖

多阶段用途：
- 缩小镜像体积
- 

