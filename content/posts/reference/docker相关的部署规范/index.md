---
title: docker 相关的部署规范
date: 2024-01-10
lastmod: 2024-02-22
author: ['Ysyy']
categories: ['']
tags: ['reference']
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
## 1.命名规范

### 1.1.镜像命名规范

镜像命名规范：`<小组名>/<项目名>/<镜像名>:<版本号>`

版本号：`<主版本号>.<次版本号>.<修订号>` eg: `1.0.0`

### 1.2.容器命名规范

容器命名规范：`<小组名>-<项目名>-<容器名>-<版本号>`