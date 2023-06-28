---
title: SQL事务
date: 2021-11-20
tags: SQL
author: 映雪
---

取次花丛懒回顾，半缘修道半缘君。

<!--more-->

![截屏2021-12-01 下午3.16.32.png](/images/2021/12/01/6L5jZdDhbtQoIHr.png)

### SQL 事务

- 事务（ Transaction）由一次或者多次基本操作构成，事务由一条或者多条 SQL 语句构成。事务中的所有 SQL 语句是一个整体，共同进退，不可分割，要么全部执行成功，要么全部执行失败。

### SQL 事务属性

1. 原子性

- 一个事务中的所有 SQL 语句，要么全部执行成功，要么全部执行失败，不会结束在中间的某个环节。事务在执行过程中发生错误，会被回滚（Rollback）到事务开始前的状态，就像这个事务从来没有执行过一样。

2. 一致性

- 在事务开始之前和事务结束以后，数据库的完整性没有被破坏。这表示写入的数据必须完全符合所有的预设规则，其中包含数据的精确度、串联性以及后续数据库可以自发性地完成预定的工作。

3. 隔离性

- 数据库允许多个并发事务同时对其数据进行读写和修改的能力，隔离性可以防止多个事务并发执行时由于交叉执行而导致数据的不一致。事务隔离分为不同级别，包括读未提交（Read uncommitted）、读提交（read committed）、可重复读（repeatable read）和串行化（Serializable）。

4. 持久性

- 事务处理结束后，对数据的修改就是永久的，即便系统故障也不会丢失。

### SQL 事务命令

1. BEGIN 或 START TRANSACTION：开始事务
2. COMMIT：提交事务
3. ROLLBACK：回滚事务
4. SAVEPOINT：在事务内部设置回滚标记点
5. RELEASE SAVEPOINT：删除回滚标记点
6. ROLLBACK TO：将事务回滚到标记点（ROLLBACK 命令的变形写法）

### SQL 事务实现

1. sql 事务开始提交

```sql
SELECT * from student_info_table;

BEGIN;
INSERT INTO student_info_table (`id`,`student_id`,`name`,`age`,`sex`,`birth`,`address`,`major_id`,`detail`,`like`,`internship_code`)VALUES (7,1602012007,'小莎',24,"女","1998-4-1","云南普洱",208,'喜欢宅家','测试',222333);
UPDATE  student_info_table SET id = 8 where `like` = '温柔';
COMMIT;

SELECT * from student_info_table;
```

2. sql 事务回滚

```sql
SELECT * from student_info_table;

BEGIN;
DELETE FROM student_info_table WHERE id = 7;
DELETE FROM student_info_table WHERE id = 8;
ROLLBACK;

SELECT * from student_info_table;
```

- 回滚事务以后，物理数据库中的数据并没有发生改变。

3. sql 事务回滚标记点

```sql
SELECT * from student_info_table;

BEGIN;
DELETE FROM student_info_table WHERE id = 7;
SAVEPOINT s;
DELETE FROM student_info_table WHERE id = 8;
ROLLBACK TO s;

SELECT * from student_info_table;
```

- 回滚到标记点 s，只有 ID 为 7 的用户被删除，ID 为 8 的用户依然留在数据库中。
