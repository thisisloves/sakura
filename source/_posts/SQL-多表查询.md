---
title: SQL多表查询
date: 2021-6-18
tags: SQL
author: 映雪
---

可是我爱你，如跛脚的孤狼穿越过荒漠几万里，在漆黑如墨滴，无边无止境的夜里，看到远方徐徐升起，最初一抹悠远而缱绻的光景。

<!--more-->

![截屏2021-12-01 下午3.17.33.png](/images/2021/12/01/qNXQk8m6PMb5wip.png)
![截屏2021-12-01 下午3.16.32.png](/images/2021/12/01/6L5jZdDhbtQoIHr.png)
![截屏2021-12-01 下午3.17.13.png](/images/2021/12/01/mGZ7dvKTPcDj31l.png)

### 笛卡尔积

- 两个集合X和Y的笛卡尔积（Cartesian product），又称直积，表示为X × Y，第一个对象是X的成员而第二个对象是Y的所有可能有序对的其中一个成员。

![截屏2021-12-15 下午4.58.41.png](/images/2021/12/15/zJDBjboMq9uHfsS.png)


### 联表查询原理

1. 先确定数据要用到哪些表。
2. 将多个表先通过笛卡尔积变成一个表。
3. 然后去除不符合逻辑的数据（根据两个表的关系去掉）。
4. 最后当做是一个虚拟表一样来加上条件即可。

### inner join

- INNER JOIN 关键字在表中存在至少一个匹配时返回行。

![截屏2021-11-25 下午4.24.53.png](/images/2021/11/25/7GaoSTeyfIWxXN8.png)

```sql
SELECT * from student_info_table inner join major_info on student_info_table.major_id = major_info.major_id;
```

### inner join （三表连接）

```sql
SELECT * from student_info_table inner join company_sql_data
on student_info_table.internship_code = company_sql_data.company_code
inner join major_info on student_info_table.major_id = major_info.major_id;
```

### left join

- LEFT JOIN 关键字从左表（table1）返回所有的行，即使右表（table2）中没有匹配。如果右表中没有匹配，则结果为 NULL。

![截屏2021-11-25 下午4.24.31.png](/images/2021/11/25/iVZe1TmbY6DjOtR.png)

```sql
SELECT * from student_info_table left join company_sql_data
on student_info_table.internship_code = company_sql_data.company_code;
```

### right join

- RIGHT JOIN 关键字从右表（table2）返回所有的行，即使左表（table1）中没有匹配。如果左表中没有匹配，则结果为 NULL。

![截屏2021-11-25 下午4.42.06.png](/images/2021/11/25/f2EoXeF7SINbcZl.png)

```sql
select * from student_info_table right join company_sql_data
on student_info_table.internship_code = company_sql_data.company_code;
```

### full outer join (mysql 不支持)

- FULL OUTER JOIN 关键字只要左表（table1）和右表（table2）其中一个表中存在匹配，则返回行 。FULL OUTER JOIN 关键字结合了 LEFT JOIN 和 RIGHT JOIN 的结果。

![截屏2021-11-25 下午4.55.04.png](/images/2021/11/25/l13RE5LgqbDVJUo.png)

```sql
select * from student_info_table FULL OUTER JOIN company_sql_data on student_info_table.internship_code = company_sql_data.company_code;
```

### union

- 操作合并两个或者多个 select 语句的结果

```sql
SELECT _ from student_info_table left join company_sql_data
on student_info_table.internship_code = company_sql_data.company_code union
select _ from student_info_table right join company_sql_data
on student_info_table.internship_code = company_sql_data.company_code;
```
