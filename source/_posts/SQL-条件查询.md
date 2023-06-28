---
title: SQL条件查询
date: 2021-7-5
tags: SQL
author: 映雪
---

可是我爱你，如瘦弱的雄鹰穿越过雪原几世纪，在稀薄的空气，肆虐无忌的寒风里，蓦然感到的那一缕，生平未见渺小却蚀骨的暖意。

<!--more-->

![截屏2021-12-01 下午3.16.32.png](/images/2021/12/01/6L5jZdDhbtQoIHr.png)

### 排序

- desc 表示降序排列 asc 表示升序  desceding  ascending

```sql
SELECT * from student_info_table ORDER BY age desc; 
```

### and过滤查询

```sql
select * from student_info_table where sex = '女' and age>22;
```

### or过滤查询

```sql
select * from student_info_table WHERE sex = '女' or age <20; 
```

### 查询前5条

```sql
SELECT  * from student_info_table LIMIT 5;
```

### like 查询

- % : 百分号包含零个或多个字符的任意字符串

1、LIKE'Mc%' 将搜索以字母 Mc 开头的所有字符串（如 McBadden）。
2、LIKE'%inger' 将搜索以字母 inger 结尾的所有字符串（如 Ringer、Stringer）。
3、LIKE'%en%' 将搜索在任何位置包含字母 en 的所有字符串（如 Bennet、Green、McBadden）。

- _ : 下划线代表任何单个字符

1、LIKE'_heryl' 将搜索以字母 heryl 结尾的所有六个字母的名称（如 Cheryl、Sheryl）

```sql
select * from student_info_table where detail LIKE  "%欢%";
```

### in 查询多个

```sql
select * from student_info_table where name in ("小强","小华");
```

### between 查询范围

```sql
select * from student_info_table where age  BETWEEN 19 and 22;
```

