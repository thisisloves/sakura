---
title: SQL子查询
date: 2021-7-18
tags: SQL
author: 映雪
---

可是我爱你，如旅人于刀山剑阵中披满身荆棘，在辽远的戈壁，携浓过乡愁的希冀，朝圣般软下了双膝，脸上挂有浅笑和未干的泪迹。

<!--more-->

![截屏2021-12-01 下午3.16.32.png](/images/2021/12/01/6L5jZdDhbtQoIHr.png)
![截屏2021-12-01 下午3.17.13.png](/images/2021/12/01/mGZ7dvKTPcDj31l.png)

### 子查询

- 一个 select 语句中包含另一个完整的 select 语句。

- 子查询就是嵌套查询，即 SELECT 中包含 SELECT，如果一条语句中存在两个，或两个以上 SELECT，那么就是子查询语句了。

#### where 型子查询

- 查询除小明之外所有同学院的同学的姓名

1. 查询小明学院 id

```sql
select major_id from student_info_table where name="小明";
```

2. 查询学院所属学生

```sql
select `name` from student_info_table where major_id = '?';
```

3. 组合

```sql
select `name` from student_info_table where major_id =
(select major_id from student_info_table where `name`='小明')
and `name` !='小明';
```

#### from 型子查询

- 查询在公司代码为 222333 的同学并且学院 id 为 203 的学生名

1. 查询公司代码为 222333 的同学

```sql
select * from student_info_table where `internship_code`='222333' ORDER BY `id` asc ;
```

2. 查询学院 id 为 203 的学生

```sql
select `name` as '姓名' from  ？  where major_id ="203";
```

3. 组合

```sql
select `name` as '姓名' from (select * from student_info_table where `internship_code`='222333' ORDER BY `id` asc )  as student_detail
where major_id ='203' ;
```

#### in 子查询

- 查询大于 21 岁的学院名

1. 查询大于 21 岁的学院 id

```sql
SELECT major_id from student_info_table where age>21;
```

2. in 查询多个学院 id 的学院名

```sql
select major_name from major_info WHERE major_id in (SELECT major_id from student_info_table where age>21);
```

### exists 子查询

- 查询学生年龄大于22岁的学院是否存在

1. 查询大于22岁学生学院id

```sql
SELECT major_id from student_info_table where age>22;
```

2.组合

```sql
select * from major_info WHERE  EXISTS   (SELECT major_id from student_info_table where major_info.major_id = student_info_table.major_id  and age>22);

```

- **not exitst 为不存在**