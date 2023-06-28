---
title: SQL条件语句
date: 2021-10-25
tags: SQL
author: 映雪
---

红颜远，相思苦。几番意，难相负。十年情思百年渡，不斩相思不忍顾。

<!--more-->

![截屏2021-12-24 下午3.47.04.png](/images/2021/12/24/Y9AhKsw7D368dtH.png)

### IF

- 表达式：IF( expr1 , expr2 , expr3 )
- expr1 条件，条件为 true，则值是 expr2 ，false，值就是 expr3

```sql
IF(字段='某一值', yes就为xxx或另一字段的值,no就为xxx或另一字段的值)
```

- 统计各科成绩各分数段人数：课程编号,课程名称,[100-85],[85-70],[70-60],[0-60]及所占人数

```sql
select cid,
count(score) as '总',
count(if(80<score and score<100,sid,NULL)) as '80-100',
count(if(70<score and score<85 ,sid,NULL)) as '70-85',
count(if(score between 70 and 60,sid,null))as '60-70',
count(if(score between 0 and 60,sid,null)) as '0-60'
from SC group by cid ;
```

###  CASE WHEN

- 表达式：

```sql
CASE 列名
    WHEN 条件 THEN 结果 
    ELSE 其他结果
    END 别名
```

```sql
select *, case cid when '01' then '课程01'  when '02' then '课程02' else '课程03' end '课程' from SC;
```

### IFNULL

- 表达式：

```sql
IFNULL( expr1 , expr2)
```

- 在 expr1 的值不为 NULL的情况下都返回 expr1，否则返回 expr2