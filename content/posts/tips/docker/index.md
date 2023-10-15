---
title: 记把深度学习项目装入docker
date: 2023-10-11
lastmod: 2023-10-15
author: ['Ysyy']
categories: ['tips']
tags: ['tips']
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
安装时出现选项

```Dockerfile
# RUN apt-get install libglib2.0-dev -y
# 由于安装libglib2.0-dev的时候，bash会有交互操作叫你选择对应的时区，在docker build的时候没有交互的，所以需要加上DEBIAN_FRONTEND="noninteractive"
RUN DEBIAN_FRONTEND="noninteractive" apt -y install libglib2.0-dev


```