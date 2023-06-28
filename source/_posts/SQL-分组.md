---
title: SQL分组
date: 2021-9-5
tags: SQL
author: 映雪
---

雪落枝，枝无叶，花似烟，奈何伊人已去，独执觞卧醉，雾里看花。

<!--more-->

![截屏2021-12-01 下午3.16.32.png](/images/2021/12/01/6L5jZdDhbtQoIHr.png)
![截屏2021-12-01 下午3.17.13.png](/images/2021/12/01/mGZ7dvKTPcDj31l.png)
![截屏2021-12-13 下午6.44.59.png](/images/2021/12/13/E46yQiNhMRYct5B.png)

### GROUP BY

- 根据一个或多个列对结果集进行分组，相同属性的数据为一个组.


- 求每个学院的学生年龄的平均数

1. 将学院表和学生表重组

```sql
select major_info.major_id ,major_info.major_name,student_info_table.name ,student_info_table.age 
from major_info 
inner join  student_info_table  
on major_info.major_id = student_info_table.major_id;
```

2. 组合

```sql
SELECT major_name , avg(age)  from (
SELECT  major_info.major_id ,major_info.major_name,student_info_table.name ,student_info_table.age 
from major_info 
inner join    student_info_table  
on major_info.major_id = student_info_table.major_id
) as major_type_avg_age_name GROUP BY major_name;
```


- 找不同的部门的最高工资

```sql
SELECT dept_name, max(salary)  from staff_salary_info GROUP BY dept_name ;
```

- 找出每年最高工资

```sql
SELECT year, max(salary)  from staff_salary_info GROUP BY  year ;
```

- 找出每个人所有年限的工资的总和

```sql
select name ,sum(salary) as '总和' from staff_salary_info GROUP BY name;
```

- 找出2020年不同部门最高工资

```sql
SELECT dept_name, max(salary)  from staff_salary_info where year = 2020 GROUP BY dept_name ;
```


- group by 两个参数
- 找出不同部门不同年份最高工资


```sql
SELECT dept_name, max(salary),year  from staff_salary_info GROUP BY dept_name ,year;
```

### group_concat()函数

- 功能：将group by产生的同一个分组中的值连接起来，返回一个字符串结果。
- 语法：group_concat( [distinct] 要连接的字段 [order by 排序字段 asc/desc ] [separator '分隔符'] )。
- 使用场景：分组后不同组不同条数数据比较。

![截屏2021-12-24 下午3.47.04.png](/images/2021/12/24/Y9AhKsw7D368dtH.png)

- 查询和"01"号的同学学习的课程完全相同的其他同学的学号和姓名

- 01下有多个课程,分组后无法直接比较,将课程拼接后比较

```sql
SELECT sid ,sname from Student 
where sid in 
(SELECT sid from SC group by sid 
HAVING GROUP_CONCAT(cid) =
(SELECT GROUP_CONCAT(cid)  from SC where sid = 01 group by sid )
 );
```


### Having

- 在 SQL 中增加 HAVING 子句原因是，WHERE 关键字无法与聚合函数一起使用。HAVING 子句可以让我们筛选分组后的各组数据。


- 区别：
1. where子句的作用是在对查询结果进行分组前，将不符合where条件的行去掉，即在分组之前过滤数据，where条件中不能包含聚组函数，使用where条件过滤出特定的行。
2. having 子句的作用是筛选满足条件的组，即在分组之后过滤数据，条件中经常包含聚组函数，使用having 条件过滤出特定的组，也可以使用多个分组标准进行分组。

- 找出不同部门不同年份最高工资除去2019年的

```sql
SELECT dept_name, max(salary),year  from staff_salary_info GROUP BY dept_name ,year HAVING year != 2019;
```