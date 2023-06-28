---
title: SQL约束
date: 2021-11-19
tags: SQL
author: 映雪
---

天地为棋盘，众生为棋子，不愿做棋子，当为下棋人。

<!--more-->

### SQL 约束（Constraints）

- SQL 约束用于规定表中的数据规则。
- 如果存在违反约束的数据行为，行为会被约束终止。
- 约束可以在创建表时规定（通过 CREATE TABLE 语句），或者在表创建之后规定（通过 ALTER TABLE 语句）。

### SQL 约束作用

- 为了确保表中的数据的完整性(准确性、正确性)，为表添加一些限制。是数据库中表设计的一个最基本规则。使用约束可以使数据更加准确，从而减少冗余数据（脏数据）。


#### NOT NULL(非空约束)

- 在默认的情况下，表的列接受 NULL 值。NOT NULL 约束强制列不接受 NULL 值。

```sql
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255) NOT NULL,
    Age int
);
```

#### UNIQUE(唯一约束)

- UNIQUE 约束唯一标识数据库表中的每条记录，UNIQUE 和 PRIMARY KEY 约束均为列或列集合提供了唯一性的保证，PRIMARY KEY 约束拥有自动定义的 UNIQUE 约束。

```sql
CREATE TABLE Persons
(
P_Id int NOT NULL,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255),
UNIQUE (P_Id)
)
```

#### PRIMARY KEY(主键约束)
- 理论上来说每一个数据表都必须有一个唯一主键作为数据的唯一标识，设置主键的列不允许为空，主键习惯 id 表示，可以在创建数据时直接指定，也可以通过修改表结构直接添加，设置为主键的列在添加数据时不能重复，既唯一性。
- PRIMARY KEY 约束唯一标识数据库表中的每条记录，主键必须包含唯一的值，主键列不能包含 NULL 值，每个表都应该有一个主键，并且每个表只能有一个主键

```sql
CREATE TABLE Persons
(
P_Id int NOT NULL,
LastName varchar(255) NOT NULL,
FirstName varchar(255),n
Address varchar(255),
City varchar(255),
PRIMARY KEY (P_Id)
)
```

#### FOREIGN KEY(外键约束)

- 一个表中的 FOREIGN KEY 指向另一个表中的 UNIQUE KEY(唯一约束的键)，FOREIGN KEY 约束用于预防破坏表之间连接的行为，FOREIGN KEY 约束也能防止非法数据插入外键列，因为它必须是它指向的那个表中的值之一。

```sql
CREATE TABLE Orders
(
O_Id int NOT NULL,
OrderNo int NOT NULL,
P_Id int,
PRIMARY KEY (O_Id),
FOREIGN KEY (P_Id) REFERENCES Persons(P_Id)
)
```

#### CHECK(列值约束)

- CHECK 约束用于限制列中的值的范围，如果对单个列定义 CHECK 约束，那么该列只允许特定的值，如果对一个表定义 CHECK 约束，那么此约束会基于行中其他列的值在特定的列中对值进行限制。

- 约束 P_Id 的列必须包含大于 0 的整数

```sql
CREATE TABLE Persons
(
P_Id int NOT NULL,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255),
CHECK (P_Id>0)
)
```

#### DEFAULT(默认约束)

- DEFAULT 约束用于向列中插入默认值，如果没有规定其他的值，那么会将默认值添加到所有的新记录。

```sql
CREATE TABLE Persons
(
    P_Id int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Address varchar(255),
    City varchar(255) DEFAULT 'Sandnes'
)
```
