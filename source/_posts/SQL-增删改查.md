---
title: SQL增删改查
date: 2021-5-25
tags: SQL
author: 映雪
---

人生如戏，妆颜上戏台，一颦一笑由己更有命；世事如棋，提子入棋局，一纵一横由人更由天。

<!--more-->

![截屏2021-12-01 下午3.16.32.png](/images/2021/12/01/6L5jZdDhbtQoIHr.png)

### 创建表

```sql
CREATE TABLE teacher(id INT,
name VARCHAR(100) NOt NULL,
age INT(10) NOT NULL,
sex VARCHAR(10) NOT NULL,
kaill VARCHAR(20) NOT NULL,
PRIMARY KEY(id)
);
```

### 展示表列

```sql
show columns from table_name;
```

### 查询

```sql
SELECT * from student_info_table;
```

### 插入

```sql
INSERT INTO student_info_table (`id`,`student_id`,`name`,`age`,`sex`,`birth`,`address`,`major_id`,`detail`,`like`,`internship_code`)
VALUES (7,1602012007,'小莎',24,"女","1998-4-1","云南普洱",208,'喜欢宅家','测试',222333);
```

### 修改

```sql
UPDATE  student_info_table SET id = 7 where `like` = '温柔';
```

### 删除

```sql
DELETE FROM student_info_table WHERE id = 7;
```

### 删除表

```sql
drop table table_name;

```
