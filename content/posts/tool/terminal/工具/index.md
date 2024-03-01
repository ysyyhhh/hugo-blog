---
title: 工具
date: 2023-08-07
lastmod: 2024-03-01
author: ['Ysyy']
categories: ['']
tags: ['terminal']
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
<https://blog.csdn.net/pyufftj/article/details/83102530>

<https://blog.csdn.net/fragrant_no1/article/details/85986511>

nohup

```
nohup python3  -u tcp_client.py > tcp.log 2>&1 &

nuhup : 不挂起的意思
python3 tcp_client.py : 使用python3环境运行 tcp_client.py文件
-u : 代表程序不启用缓存，也就是把输出直接放到log中，没这个参数的话，log文件的生成会有延迟
> tcp.log : 把程序输出日志保存到tcp.log文件中
2>&1 : 换成2>&1，&与1结合就代表标准输出了，就变成错误重定向到标准输出
& : 最后一个& ，代表该命令在后台执行
```

```
nohup python3  -u main.py > chatbot.log 2>&1 &
nohup ./go-cqhttp > go-cq.log 2>&1 &
```

# curl

# curl