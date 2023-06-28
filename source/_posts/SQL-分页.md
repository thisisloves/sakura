---
title: SQL分页
date: 2021-8-5
tags: SQL
author: 映雪
---

但斟酒，明日复何求?今夕还胜往时忧，怕得哪岁空回首，独去遣离愁。

<!--more-->

![截屏2021-12-01 下午3.16.32.png](/images/2021/12/01/6L5jZdDhbtQoIHr.png)


### LIMIT 

- 1. 使用一个参数(查询前n条数据)

```sql
select * from student_info_table  LIMIT 2;
```

- 查询前两条数据等于

```sql
select * from student_info_table  LIMIT 0 ,2;
```


- 2. 使用两个参数 (开始第n条数据，截取m条数据)



```sql
select * from student_info_table  LIMIT 1 ,2;
```

- 查询出第2第3条数据 


### 基础分页 


```sql
select * from student_info_table  LIMIT 0 ,2;
select * from student_info_table  LIMIT 2 ,2;
select * from student_info_table  LIMIT 4 ,2;
```

- 以上为第一页第二页第三页查询sql，每页2条

- n为页数，m为每页多少个

```sql
select * from student_info_table  LIMIT (n-1)*m ,m;
```

- 弊端：如果是limit 100000,100，需要扫描100100行，在一个高并发的应用里，每次查询需要扫描超过10W行，性能肯定大打折扣。


### 优化分页

- 思路：按id先过滤排序出需求的数据，再进行分页，计算偏移量，这样查询扫描数据数量大大下降。

```sql
select * from student_info_table  where id> 4 order by id asc LIMIT 0,2;
```
- 查询结果为第5条第6条

```sql
select * from student_info_table  where id> 4 order by id asc LIMIT 2,2;
```
- 查询结果为第7条第8条