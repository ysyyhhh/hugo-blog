---
title: 部署手册
date: 2024-02-07
lastmod: 2024-02-26
author: ['Ysyy']
categories: ['']
tags: ['reference']
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
## docker 镜像上传到私有仓库

```shell
docker login -u username -p password registry.cn-hangzhou.aliyuncs.com

docker tag image registry.cn-hangzhou.aliyuncs.com/username/image:tag

docker push registry.cn-hangzhou.aliyuncs.com/username/image:tag
```

## Server
### SpringBoot 

application.yml 必须使用环境变量来进行配置。

```yaml
spring:
  datasource:
    url: jdbc:mysql://${MYSQL_HOST:localhost}:${MYSQL_PORT:3306}/${MYSQL_DATABASE:test}?useUnicode=true&characterEncoding=utf-8&useSSL=false&serverTimezone=Asia/Shanghai
    username: ${MYSQL_USER:test}
    password: ${MYSQL_PASSWORD:123456}
```