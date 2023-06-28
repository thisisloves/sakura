---
title: SQL表单操作
date: 2021-6-25
tags: SQL
author: 映雪
---

可是我爱你，如盲人于火海悬丝之上亦步亦趋，在空濛的静寂，轻哼着某支无名曲，倏忽间睁开了眼睛，有幸初次目睹整片洪荒天地。

<!--more-->

![截屏2021-12-01 下午3.38.28.png](/images/2021/12/01/n45aBZhItzT13pR.png)

### 复制表单

```
create table user_data_copy as select * from user_data;
```

### 单复制表单结构

```
create table user_data_copy2 as SELECT * from user_data where 1=2;
```

### 复制表单数据（表单数据结构一样）

```
insert into  user_data_copy2 select * from  user_data_copy;
```

### 复制表单数据 (两个表单数据结构不一样)

```
insert into user_data_copy2(`password`,`user`,`dept`) SELECT `user`,`password`,`detail`  from user_data_copy;
```