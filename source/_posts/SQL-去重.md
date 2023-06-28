---
title: SQL去重
date: 2021-9-25
tags: SQL
author: 映雪
---

东风夜放花千树，更吹落、星如雨。宝马雕车香满路。凤箫声动，玉壶光转，一夜鱼龙舞。

<!--more-->

![截屏2021-12-13 下午6.44.59.png](/images/2021/12/13/E46yQiNhMRYct5B.png)


### DISTINCT
- 在表中，一个列可能会包含多个重复值，希望仅仅列出不同（distinct）的值。
- DISTINCT 关键词用于返回唯一不同的值。

- 用法注意：

1. distinct 【查询字段】，必须放在要查询字段的开头，即放在第一个参数；
2. 只能在SELECT 语句中使用，不能在 INSERT, DELETE, UPDATE 中使用；
3. DISTINCT 表示对后面的所有参数的拼接取 不重复的记录，即查出的参数拼接每行记录都是唯一的
4. 不能与all同时使用，默认情况下，查询时返回的就是所有的结果。


- 一个参数
- 对一个字段查重，表示选取该字段一列不重复的数据。

```sql
SELECT distinct name from staff_salary_info;
```

- 两个参数 
- 对多个字段去重，表示选取多个字段拼接的一条记录，不重复的所有记录

```sql
SELECT distinct name,year from staff_salary_info;
```