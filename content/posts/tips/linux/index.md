---
title: Linux
date: 2023-11-16
lastmod: 2023-11-16
author: ['Ysyy']
categories: ['']
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
## 用户相关

```shell
# 创建用户
useradd -m -s /bin/bash -d /home/username username
## 解释: -m 创建用户目录, -s 指定shell, -d 指定用户目录

# 设置密码
passwd username

# 删除用户
userdel -r username
```