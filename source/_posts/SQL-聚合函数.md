---
title: SQL聚合函数
date: 2021-7-25
tags: SQL
author: 映雪
---

肠断相思岁岁同，事世情缘梦难通。满山烟雨桔花白，无尽相思夜夜风。

<!--more-->

![截屏2021-12-01 下午3.16.32.png](/images/2021/12/01/6L5jZdDhbtQoIHr.png)


### 聚合函数

- 聚合函数指的是对一组值执行计算并返回单一的值。

### AVG()

- 函数返回数值列的平均值

* 学生表所有人平均

```sql
SELECT avg(age) from student_info_table ;
```

### COUNT()

- 返回匹配指定条件的行数

* 学生表所有人年龄大于22的人数

```sql
Select count(*) from student_info_table where age>22;
```

### MAX() 和 MIN()

- max 函数返回指定列的最大值。min 函数返回指定列的最小值

```sql
select max(age) from student_info_table ;
select min(age) from student_info_table ;
```

### SUM()

- 返回数值列的总数。

* 返回年龄总数

```sql
select sum(age) as '年龄总数' from student_info_table ;
```


**注意：以上函数不能使用在group by之后**


