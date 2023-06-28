---
title: SQL数据库
date: 2019-4-15
tags: SQL
author: 映雪
---

青鲤来时遥闻春溪声声碎，嗅得手植棠梨初发轻黄蕊。山栀茶千张纸，对坐草蒲黄，无名子半夏曲，汉宫秋莲房。

<!--more-->

### 数据库

- 数据库是按照数据结构来组织、存储和管理数据的仓库，每个数据库都有一个或多个不同的 API 用于创建，访问，管理，搜索和复制所保存的数据。我们也可以将数据存储在文件中，但是在文件中读写数据速度相对较慢。所以我们使用关系型数据库管理系统，来存储和管理大量的数据。

- 所谓关系型数据库，是建立在关系模型基础上的数据库，借助与集合代数等数学概念和方法来处理数据库中的数据。
- 其特点为：

1. 数据以表格形式出现；
2. 每行为各种记录的名称；
3. 每列为记录名称所对应的数据域；
4. 许多行和列组成一张表单；
5. 若干的表单组成 database

- 数据库：数据库是一些关联表的集合；

- 数据表：表是数据的矩阵，在一个数据库中的表看起来像是一个简单的电子表格；

- 列：一列数据元素，包含了相同的数据；字段

- 行：一行记录，是一组相关的数据；记录

- 主键：是唯一的，一个数据表只能包含一个主键，可以用主键查询数据；

- MySQL 是一个开源的关系型数据库管理系统。目前属于 Oracle 公司。

### MySQL 语句的规范

1. 关键字与函数名称全部大写
2. 数据库名称，表名称，字段名称全部小写
3. SQL 语句必须以分号结尾

### MySQL 基础操作

- 启动 mysql 服务器

```
> net start mysql
```

- 关闭 mysql 服务器

```
> net stop mysql
```

- mysql 登录

```
> mysql -hloacalhost -uroot -p
```

- 展示数据库

```
> show databases;
```

- 创建数据库

```sql
> create database db_name;
> character set charset_name;   //创建数据库同时设置编码方式
```

- 查看错误信息

```
> show warnings;
```

- 查看编码格式

```sql
> show create database db_name;
```

- 修改编码格式

```sql
>  alter database db_name character set utf8;
```

- 使用数据库

```
> use mysql;
```

- 退出

```
> mysql > exit;
```

- nginx 开启

```
start nginx nginx
```

- nginx 关闭

```
nginx -s quit
```
