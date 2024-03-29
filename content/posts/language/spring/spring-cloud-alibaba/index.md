---
title: 微服务
date: 2022-03-03
lastmod: 2024-03-02
author: ['Ysyy']
categories: ['']
tags: ['spring']
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
各个springboot

# Nacos注册中心

#### 核心功能

服务注册：

服务心跳：

服务同步：

服务发现：拿到微服务地址

服务调用：

服务健康检查：

## Ribbon 负载均衡

## feign 优雅地调用远程服务

解决的是微服务之间调用问题

## sentinel 服务容错

解决服务雪崩等问题

## 服务网关

### 解决的问题

解决客户端访问微服务的问题：

1. 维护微服务的多个地址
2. 认证 鉴权复杂
3. 跨域问题

所谓的API网关，就是指系统的统一入口。对于客服端来说，它封装了应用程序的内部结构，为客户端提供统一服务，一些**与业务本身功能无关**的**公共逻辑**可以在这里实现，诸如**认证、鉴权、监控、路由转发**等等。

### 目前主流的解决方案

* Ngnix+lua

使用nginx的**反向代理和负载均衡**可实现对api服务器的负载均衡及高可用

lua是一种脚本语言,可以来编写一些简单的逻辑, nginx支持lua脚本

* Kong

  基于Nginx+Lua开发，性能高，稳定，有多个可用的插件(限流、鉴权等等)可以开箱即用。 问题：

只支持Http协议；二次开发，自由扩展困难；提供管理API，缺乏更易用的管控、配置方式。

* Zuul

  springboot1系列用的，Netflix开源的网关，功能丰富，使用JAVA开发，易于二次开发。Zuul 1.0 有问题：缺乏管控，无法动态配置；依赖组件较多；处理Http请求依赖的是Web容器，性能不如Nginx。

  Zuul有2.0
* Spring Cloud Gateway

  Spring公司为了替换Zuul而开发的网关服务，将在下面具体介绍。

Gateway

缺点：

* 其实现依赖Netty与WebFlux，不是传统的Servlet编程模型，学习成本高
* 不能将其部署在Tomcat、Jetty等Servlet容器里，只能打成jar包执行
* 需要Spring Boot 2.0及以上的版本，才支持

### 路由 route

路由(Route) 是 gateway 中最基本的组件之一，表示一个具体的路由信息载体。主要定义了下面的几个信息:

id，路由标识符，区别于其他 Route，默认是一个随机的UID，最好自己起一个###

uri，路由指向的目的地 uri，即客户端请求最终被转发到的微服务。

order，用于多个 Route 之间的排序，数值越小排序越靠前，匹配优先级越高。

predicate，断言的作用是进行条件判断，只有断言都返回真，才会真正的执行路由。

filter，过滤器用于修改请求和响应信息。

### 断言

predicate 用于条件判断，只有全部的断言为真，才实现路由转发。

4\.5.1 内置路由断言工厂

可以自定义断言

### 过滤器

1. 作用：在请求过程中，对请求和响应做手脚
2. 生命周期：PRE  和 POST
3. 分类：局部过滤器（作用在一个路由上），全局过滤器（全部路由是）

PRE生命周期：在被路由之前调用，跨域实现验证身份，集群。

POST生命周期：可以添加标准的Header，收集统计信息。

#### 局部过滤器 GateAway

内置有很多，可以自定义

#### 全局过滤器

#### 网关限流

用sentinel

## MQ消息队列

一般用于请求加快