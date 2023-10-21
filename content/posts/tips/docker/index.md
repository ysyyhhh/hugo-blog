---
title: docker相关技巧
date: 2023-10-21
lastmod: 2023-10-21
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
## 记把深度学习项目装入docker

安装时出现选项

```Dockerfile
# RUN apt-get install libglib2.0-dev -y
# 由于安装libglib2.0-dev的时候，bash会有交互操作叫你选择对应的时区，在docker build的时候没有交互的，所以需要加上DEBIAN_FRONTEND="noninteractive"
RUN DEBIAN_FRONTEND="noninteractive" apt -y install libglib2.0-dev


```

## docker清理

在win10下，docker是基于wsl2的，所以docker的镜像和容器都是在wsl2的文件系统中。
所以在清理完docker的镜像和容器后，需要对wsl的盘进行压缩。

```bash
# 停止所有的容器
docker stop $(docker ps -aq)

# 删除所有未使用的容器
docker volume prune

# 删除所有未使用的镜像
docker image prune -a

# 查看当前占用的空间
docker system df
```

对wsl2的盘进行压缩

```bash
wsl --shutdown

# 查看wsl2的盘
wsl --list -v

# 使用diskpart压缩
diskpart
# open window Diskpart
select vdisk file="C:\Users\<你的用户名>\AppData\Local\Docker\wsl\data\ext4.vhdx"
attach vdisk readonly
compact vdisk
detach vdisk
exit
```