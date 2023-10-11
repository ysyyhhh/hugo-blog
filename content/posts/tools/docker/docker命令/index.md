---
title: Docker命令
date: 2023-10-11
lastmod: 2023-10-11
author: ['Ysyy']
categories: ['tools']
tags: ['docker']
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
## 1.镜像相关

```shell

# 拉取镜像
docker pull [选项] [Docker Registry 地址[:端口号]/]仓库名[:标签]

# 查看镜像
docker images [选项] [仓库名]

# 删除镜像
docker rmi [选项] <镜像1> [<镜像2> ...]

# 查看镜像历史
docker history [选项] <镜像名>

# 查看镜像详细信息
docker inspect [选项] <镜像名>
```

## 2.容器相关

```shell
# 创建容器
docker run [选项] <镜像名> [命令]
#eg: docker run -d -p 8080:8080 --name tomcat tomcat:8.5.51
#选项
# -d 后台运行容器，并返回容器ID
# -i 以交互模式运行容器，通常与 -t 同时使用
# -t 为容器重新分配一个伪输入终端，通常与 -i 同时使用
# -P 随机端口映射
# -p 指定端口映射，格式为：主机(宿主)端口:容器端口
# --name 指定容器名字
# --link 连接到其它容器
# --rm 容器退出后自动删除容器文件
# --volumes-from 从其它容器或数据卷挂载一些配置或其它文件
# --volume 挂载宿主机目录或文件，格式为：主机目录:容器目录
# --privileged=true 给容器内的root用户赋予最高权限，容器内的root用户就拥有了真正的root权限
# --restart
#   no 容器退出时不重启
#   on-failure[:max-retries] 容器故障退出（返回值非零）时重启，最多重启max-retries次
#   always 容器退出时总是重启
#   unless-stopped 容器退出时总是重启，但是不考虑在Docker守护进程启动时就已经停止了的容器



# 查看容器
docker ps [选项]

# 删除容器
docker rm [选项] <容器名>

# 启动容器
# 启动和创建容器的区别在于，启动容器是针对已经创建好的容器进行启动，而创建容器则是针对镜像进行的操作
docker start [选项] <容器名>

# 停止容器
docker stop [选项] <容器名>

# 查看容器日志
docker logs [选项] <容器名>

# 查看容器内进程
docker top [选项] <容器名>

# 查看容器详细信息
docker inspect [选项] <容器名>

# 进入容器
docker exec [选项] <容器名> [命令]

# 导出容器
docker export [选项] <容器名>

# 导入容器
docker import [选项] <容器名>

# 重命名容器
docker rename [选项] <容器名> <新容器名>

# 查看容器使用的资源
docker stats [选项] <容器名>

# 查看容器端口映射
docker port [选项] <容器名>

```