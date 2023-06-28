---
title: SQL执行机制
date: 2021-10-5
tags: SQL
author: 映雪
---

流云千丈堪醉卧，是谁月下独酌。浮生谁能一笑过，明灭楼台上灯火。

<!--more-->

![截屏2021-12-13 下午6.44.59.png](/images/2021/12/13/E46yQiNhMRYct5B.png)


### 找出除会记专业 其他不同专业年龄最小 并且最小超过18 ，并且按年龄正序排列

```sql
SELECT major_info.major_type AS '专业类型' ,min(student_info_table.age) AS '最小年龄'
FROM student_info_table 
INNER JOIN major_info ON student_info_table.major_id = major_info.major_id 
WHERE major_info.major_type != '会记'
GROUP BY major_type 
HAVING min(student_info_table.age)>18
ORDER BY min(student_info_table.age) ASC;

```


### 执行顺序

1. FROM:执行顺序为从后往前、从右到左。数据量较大的表尽量放在后面。

2. WHERE：执行顺序为自下而上、从右到左。将能过滤掉最大数量记录的条件写在WHERE字句的最右。

3. GROUP BY:执行顺序从右往左分组，最好在GROUP BY前使用WHERE将不需要的记录在GROUP BY之前过滤掉。

4. HAVING：消耗资源。尽量避免使用，HAVING会在检索出所有记录之后才对结果进行过滤，需要排序等操作。

5. SELECT：少用 `*` 号，尽量使用字段名称，oracle在解析的过程中，通过查询数据字典将`*`号依次转换成所有列名，消耗时间。

6. ORDER BY：执行顺序从左到右，消耗资源。


### SQL优化

1. WHERE子句后面的条件顺序对大数据量表的查询会产生直接的影响,数据量较大的表尽量放在后面。
2. 在IN后面值的列表中，将出现最频繁的值放在最前面，出现得最少的放在最后面，减少判断的次数。
3. 注意UNION和UNION ALL的区别。--允许重复数据用UNION ALL好。
4. 注意使用DISTINCT，在没有必要时不要用。
5. 如果语句很复杂，连接太多，可以考虑用临时表和表变量分步完成。
6. NOT IN、NOT EXISTS的相关子查询可以改用LEFT JOIN代替写法。
7. 不要用COUNT(`*`)的子查询判断是否存在记录，最好用LEFT JOIN或者EXISTS。
8. 尽量使用索引。
9. 多表连接的时候，连接条件必须写全，宁可重复，不要缺漏。
10. 不要写SELECT `*`的语句，而是选择你需要的字段。
11. 当在SQL语句中连接多个表时, 请使用表的别名并把别名前缀于每个Column上。这样一来,就可以减少解析的时间并减少那些由Column歧义引起的语法错误。
12. 减少多次的数据转换，也许需要数据转换是设计的问题，减少次数。
13. 杜绝不必要的子查询和连接表。
14. exists 和 in 的效率 当两个表大小差不多的时候，效率差不多，但是当一个表大一个表小的时候，子查询表大的用exists，子查询表小的用in。