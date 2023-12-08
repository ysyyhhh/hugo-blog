---
title: MySql常用命令
date: 2023-10-11
lastmod: 2023-12-08
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
##### 修改用户密码

```
alter user 'root'@'localhost' identified with mysql_native_password by '123456';
```

##### 刷新权限

```
flush privileges;
```

##### 添加一个远程用户

```
create user 'remote'@'%' identified by 'password'
GRANT all ON *.* TO 'remote'@'%';
grant all privileges on *.* to 'remote'@'%' with grant option;

*.*所有数据库下的所有表
```

##### 创建数据库并设定中文编码

```
CREATE DATABASE `db_name` DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;
```

##### 登录格式

```
mysql -h #{数据库IP} -P 3306 -u #{用户名} -p -D #{数据库名}
```

##### 自增id 不连续时

```
SET @auto_id = 0;
UPDATE 表名 SET 自增字段名 = (@auto_id := @auto_id + 1);
ALTER TABLE 表名 AUTO_INCREMENT = 1;

```

#### 文件

```
source
```