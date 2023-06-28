---
title: SQL窗口函数
date: 2021-10-18
tags: SQL
author: 映雪
---

心微动奈何情己远。物也非,人也非,事事非,往日不可追。

<!--more-->

![截屏2021-12-24 下午3.47.04.png](/images/2021/12/24/Y9AhKsw7D368dtH.png)

### OVER

- OVER 用于为行定义一个窗口，它对一组值进行操作，不需要使用 GROUP BY 子句对数据进行分组，能够在同一行中同时返回基础行的列和聚合列。

### OVER 的语法

```
OVER ( [ PARTITION BY column ] [ ORDER BY culumn ] )
```

- PARTITION BY 子句进行分组；
- ORDER BY 子句进行排序。

- 窗口函数 OVER()指定一组行，开窗函数计算从窗口函数输出的结果集中各行的值。
- 开窗函数不需要使用 GROUP BY 就可以对数据进行分组，还可以同时返回基础行的列和聚合列。

### OVER 的用法

- OVER 开窗函数必须与聚合函数或排序函数一起使用，聚合函数一般指 SUM(),MAX(),MIN,COUNT(),AVG()等常见函数。排序函数一般指 RANK(),ROW_NUMBER(),DENSE_RANK(),NTILE()等。


### 例子

(1).查询各科成绩最高分、最低分和平均分：以如下形式显示：课程 ID，课程 name，最高分，最低分，平均分，及格率

1. 先查出 课程 ID，课程 name，最高分，最低分，平均分 , 总人数

```sql
 SELECT distinct cid as '课程ID',
       min(score) Over(partition by cid ) as '最低分' ,
       max(score) Over(partition by cid ) as '最高分',
       avg(score) Over(partition by cid ) as '平均分',
       count( score) Over(partition by cid ) as '总人数'
    from SC;
```

2. 查出每科合格人数

```sql
SELECT count(cid) as '及格人数' ,cid from SC where score > 60 GROUP BY cid;
```

3. 连接组合

```sql
   SELECT  t1.课程ID,t1.最低分,t1.最高分,t1.平均分,t1.总人数,t2.及格人数 ,concat(ROUND(t2.及格人数 *100/t1.总人数,0),'%')  as '及格率' from ( SELECT distinct cid as '课程ID',
       min(score) Over(partition by cid ) as '最低分' ,
       max(score) Over(partition by cid ) as '最高分',
       avg(score) Over(partition by cid ) as '平均分',
       count( score) Over(partition by cid ) as '总人数'
    from SC) as t1 inner join (SELECT count(cid) as '及格人数' ,cid from SC where score > 60 GROUP BY cid) as t2 WHERE
  t1.课程ID = t2.cid;
```

(2).查询学生的总成绩并进行排名

```sql
   SELECT t.sid,t.总成绩 ,ROW_NUMBER() Over(ORDER BY t.总成绩 desc) as '排名' from   (
   SELECT distinct sid  ,sum(score) Over(partition by sid  ) as '总成绩'  from SC
) as t ;
```

(3).查询每个学生每门成绩排名以及总平均成绩

```sql
    SELECT sid,cid,score,
       ROW_NUMBER() Over(PARTITION BY sid ORDER BY score asc ) as sort,
       avg(score) Over(PARTITION BY sid ) as avg from SC ;
```

### group by 和 partition by 区别

- group by 和 partition by 都有分组统计的功能，但是partition by并不具有group by的汇总功能。
- partition by统计的每一条记录都存在，而group by将所有的记录汇总成一条记录(类似于distinct EmpDepartment 去重)。
- partition by可以和聚合函数结合使用，同时具有其他高级功能。
- 执行顺序上 partition by 实际上就是在执行完select之后，在所得结果集之上进行partition。
- partition by相比较于group by，能够在保留全部数据的基础上，只对其中某些字段做分组排序。