---
title: mongoDB
date: 2023-11-28
lastmod: 2023-11-30
author: ['Ysyy']
categories: ['']
tags: ['sql']
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
## 常用命令

```bash
# 连接
mongosh ip[:port]/database -u username -p password

# 查看数据库
show dbs

# 切换数据库
use database

# 查看集合
show collections
# file

# 查看集合数据
db.{collection}.find()

# 按条件查看集合数据
## pid=1
db.{collection}.find({pid:1})

```