---
title: nginx
date: 2023-12-02
lastmod: 2024-02-04
author: ['Ysyy']
categories: ['']
tags: ['nginx']
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
## nginx安装

### 1. 安装依赖

```bash
yum install -y gcc gcc-c++ autoconf automake make

yum install -y pcre pcre-devel

yum install -y zlib zlib-devel

yum install -y openssl openssl-devel
```

### 2. 转发后端图片

```bash
# 1. 创建目录
mkdir -p /data/nginx/cache

# 2. 修改目录权限
chown -R nginx:nginx /data/nginx/cache
```