---
title: nginx
date: 2024-03-01
lastmod: 2024-03-02
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


## nginx 命令

```shell
# 启动
nginx

# 重启, 重新加载配置文件
nginx -s reload

# 停止
nginx -s stop

# 测试配置文件是否正确
nginx -t
```