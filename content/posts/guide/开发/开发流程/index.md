---
title: 开发流程
date: 2023-09-26
lastmod: 2024-03-02
author: ['Ysyy']
categories: ['']
tags: ['开发']
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
测试环境

- 自动化部署
- 一键转移到生成环境



## 部署





## 项目代码

- 测试代码
  - 编码风格测试&修正
  - 功能测试
- Dockerfile
- docker-compose
- 测试脚本

- 部署脚本



## 前端代码结构

### 优点

接口可以根据环境自动替换:

- 开发环境的接口
- 生产环境



### 1.docker-run.sh

描述: 部署脚本在git clone之后运行的脚本

内容: 包括docker的构建和运行

示例

```shell
docker-compose down
docker rmi digitalmapadmin-frontend
docker build -t digitalmapadmin-frontend .
docker-compose pull
docker-compose up -d
```



### 2.docker-compose.yaml

描述: docker-run.sh 会用到docker-compose指令

内容: 

- Dockerfile 路径
- 端口
- 环境变量
  - **生产环境下的后端接口** # 这样可以在部署时 自动替换成生产环境的后端接口.

```dockerfile
version: '3.0'
services:
  frontend:
    build:
      context: .
      dockerfile: ./Dockerfile
    ports:
      - 8004:80
    environment:
      - NODE_ENV=production
      - VITE_APP_TITLE=数据资源管理平台
      - VITE_APP_BASE_API=/api
      - VITE_SERVE=http://121.40.252.139:8089/
```

### 3.Dockerfile

描述: 根据项目构建docker 镜像

内容:

- 获取dist: 
  - 安装 npm: 并进行npm install
  - npm run build
- 用nginx运行项目
  - 配置nginx
  - 启动项目

```
FROM node:lts-alpine

WORKDIR /app

# 先将package.json和package-lock.json拷贝到工作目录中
COPY package*.json ./

RUN npm install

# 将当前目录下的所有文件拷贝到工作目录中

COPY . .

RUN npm run build

FROM nginx:alpine

# 将打包后的dist目录下全部文件拷贝到nginx的html/目录下
# COPY ./dist/ /usr/share/nginx/html/
COPY --from=0 /app/dist /usr/share/nginx/html

# 删除nginx中之前的配置
RUN rm /etc/nginx/conf.d/default.conf
# 拷贝当前的文件到nginx中
COPY nginx.conf /etc/nginx/nginx.conf
COPY default.conf.template /etc/nginx/conf.d

# 启动nginx
CMD /bin/sh -c "envsubst '80' < /etc/nginx/conf.d/default.conf.template > /etc/nginx/conf.d/default.conf" && nginx -g 'daemon off;'
```







-