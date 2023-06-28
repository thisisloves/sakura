---
title: SQL锁
date: 2021-11-23
tags: SQL
author: 映雪
---

不问缘由知底里，何如静默待流莺。

<!--more-->

### SQL 锁

- 当有事务操作时，数据库引擎会要求不同类型的锁定，如相关数据行、数据页或是整个数据表，当锁定运行时，会阻止其他事务对已经锁定的数据行、数据页或数据表进行操作。只有在当前事务对于自己锁定的资源不在需要时，才会释放其锁定的资源，供其他事务使用。

### SQL 颗粒划分锁类

1. 表锁

- 一次对整个表加锁，MyISAM 存储引擎使用表锁。开销小、加锁块、无死锁。容易发生锁冲突，并发度低。

```sql
-- 表锁
LOCK TABLE product_comment READ;
-- 解表锁
UNLOCK TABLE product_comment;
```

3. 行锁

- 一次对一条数据加锁，InnoDB 使用行锁，开销大、加锁慢、容易出现死锁。锁范围小，并发度高，不易发生锁冲突。

```sql
SELECT comment_id, product_id, comment_text, user_id FROM product_comment
WHERE user id = 912178 LOCK IN SHARE MODE;

SELECT comment_id, product_id, comment_text, user_id FROM product_comment
WHERE user id = 912178 FOR UPDATE;
```

### SQL 数据库管理划分锁类

1. 共享锁

- 共享锁锁定的资源可以被其他用户读取，但不能修改。在进行 SELECT 的时候，会将对象进行共享锁锁定，当数据读取完毕之后，就会释放共享锁，这样就可以保证数据在读取时不被修改。

```sql
-- 加共享锁
LOCK TABLE product_comment READ;
-- 解共享锁
UNLOCK TABLE product_comment;
-- 加行锁
SELECT comment_id, product_id, comment_text, user_id FROM product_comment
WHERE user id = 912178 LOCK IN SHARE MODE;

```

2. 排他锁

- 排它锁锁定的数据只允许进行锁定操作的事务使用，其他事务无法对已锁定的数据进行查询或修改。

```sql
-- 加写锁
LOCK TABLE product_comment WRITE;
-- 解锁
UNLOCK TABLE product_comment;
-- 加行锁
SELECT comment_id, product_id, comment_text, user_id FROM product_comment
WHERE user id = 912178 FOR UPDATE;

```

### SQL 程序员划分锁类

1. 乐观锁

- 乐观锁认为对同一数据的并发操作不会总发生，属于小概率事件，不用每次都对数据上锁，也就是不采用数据库自身的锁机制，而是通过程序来实现。在程序上，我们可以采用版本号机制或者时间戳机制实现。

2. 悲观锁

- 悲观锁也是一种思想，对数据被其他事务的修改持保守态度，会通过数据库自身的锁机制来实现，从而保证数据操作的排它性。
