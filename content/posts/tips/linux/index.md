---
title: Linux
date: 2023-11-17
lastmod: 2023-11-18
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

# 添加用户的sudo权限
## 编辑sudoers文件
vi /etc/sudoers
## 在root ALL=(ALL) ALL下面添加
username ALL=(ALL) ALL

# 查看用户组
groups username

# 修改用户组
usermod -g groupname username

```

## 工具

### curl

```shell
# 下载文件
curl -o filename url

```