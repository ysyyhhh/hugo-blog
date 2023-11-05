---
title: docker相关技巧
date: 2023-10-24
lastmod: 2023-11-05
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

# 删除缓存
docker builder prune


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
select vdisk file="D:\ubuntu\wsl\docker-desktop-data\ext4.vhdx"
attach vdisk readonly
compact vdisk
detach vdisk
exit
```

## docker中安装conda

```Dockerfile

# 安装conda

RUN apt-get install -y wget

# yhyu13 : donwload anaconda package & install
RUN wget "https://repo.anaconda.com/archive/Anaconda3-2023.03-1-Linux-x86_64.sh" 

RUN sh Anaconda3-2023.03-1-Linux-x86_64.sh -b -p /opt/conda

# RUN rm /anaconda.sh 

RUN ln -s /opt/conda/etc/profile.d/conda.sh /etc/profile.d/conda.sh

RUN echo ". /opt/conda/etc/profile.d/conda.sh" >> ~/.bashrc

# yhyu13 : add conda to path  
ENV PATH /opt/conda/bin:/opt/conda/condabin:$PATH
```

## docker-compose 使用gpu

```yaml
version: '3.7'
services:
  pytorch:
    build: .
    runtime: nvidia
    environment:
      - NVIDIA_VISIBLE_DEVICES=all
      - NVIDIA_DRIVER_CAPABILITIES=all
    volumes:
      - .:/workspace
    ports:
      - "8888:8888"
      - "6006:6006"
    command: bash -c "jupyter notebook --ip
```

## wsl 盘迁移到非系统盘

一般情况下 wsl盘的位置在
`C:\Users\<用户名>\AppData\Local\Docker\wsl`

docker的盘在
`C:\Users\<用户名>\AppData\Local\Docker\wsl\data`

```bash

# 1. 停止wsl
wsl --shutdown

# 2. 查看wsl状态
wsl --list -v
# 可以看到docker有两个wsl，一个是docker-desktop-data，一个是docker-desktop
# 只需要迁移docker-desktop-data即可,另一个很小

# 3. 迁移wsl
wsl --export Ubuntu-20.04 D:\ubuntu\wsl\Ubuntu-20.04.tar

# 4. 删除wsl
wsl --unregister Ubuntu-20.04


# 5. 查看是否删除成功
wsl --list -v

# 6. 导入wsl
wsl --import Ubuntu-20.04 D:\ubuntu\wsl\Ubuntu-20.04 D:\ubuntu\wsl\Ubuntu-20.04.tar --version 2

# 7. 查看是否导入成功
wsl --list -v
```