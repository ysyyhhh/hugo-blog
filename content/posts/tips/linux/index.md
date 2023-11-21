---
title: Linux
date: 2023-11-20
lastmod: 2023-11-21
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

# 查看所有用户
cat /etc/passwd

```

## 目录挂载

```shell

# 查看挂载
df -h

# 挂载目录
mount /dev/sdb1 /home/username/data

# 卸载目录
umount /home/username/data

# 挂载硬盘
## 查看硬盘
fdisk -l

## 格式化硬盘
fdisk /dev/sdb

## 格式化为ext4
mkfs.ext4 /dev/sdb1

```

挂载目录并立即生效

```shell
# 挂载目录
mount /dev/sdb1 /home/username/data

# 立即生效
mount -a

```

## 文件

```shell

# 带权限复制
cp -rp source dest

```

## 工具

### curl

```shell
# 下载文件
curl -o filename url

```

## 系统路径/变量

持久化添加/改变系统路径/变量

```shell
# 添加到系统路径
echo 'export PATH=$PATH:/home/username/bin' >> /etc/profile

# 立即生效
source /etc/profile

```