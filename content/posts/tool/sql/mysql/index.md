---
title: MySQL
date: 2024-03-01
lastmod: 2024-03-02
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
## MySql常用命令

##### 修改用户

修改密码
```
alter user 'root'@'localhost' identified with mysql_native_password by '123456';
```

修改用户host

```sql
update mysql.user set host = '%' where user = 'root
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

##### 删除用户

```
drop user 'remote'@'%';
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

#### 数据库

设置数据库的字符集

```sql
alter database 数据库名 character set utf8;
```

#### 表

```sql
# 添加一列
alter table 表名 add column 列名 类型;


```



#### 数据

```sql

# 插入数据
insert into 表名 (字段1,字段2) values (值1,值2);


# 更新数据
update 表名 set 字段1=值1,字段2=值2 where 条件;

# 删除数据
delete from 表名 where 条件;
```
### 时间处理

Date



### 条件语句

CASE

强制转换

CAST