---
title: 服务端
date: 2023-07-23
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
jar包启动

```
nohup java -jar -Xms128M -Xmx128M -XX:PermSize=128M -XX:MaxPermSize=128M jar包名.jar
```

nacos启动

```
sh /home/tmp/nacos/bin/startup.sh -m standalone
```

服务器启动

```
cd /home/mind_wings
nohup java -jar -Xms128M -Xmx128M -XX:PermSize=128M -XX:MaxPermSize=128M service-user-1.0-SNAPSHOT.jar &
nohup java -jar -Xms128M -Xmx128M -XX:PermSize=128M -XX:MaxPermSize=128M service-timetable-1.0-SNAPSHOT.jar &
nohup java -jar -Xms128M -Xmx128M -XX:PermSize=128M -XX:MaxPermSize=128M -noverify api-gateway-1.0-SNAPSHOT.jar &
```

出现过的问题

```
api-gateway 启动失败
https://blog.csdn.net/crxk_/article/details/103196146
```