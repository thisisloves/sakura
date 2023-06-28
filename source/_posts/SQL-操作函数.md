---
title: SQL操作函数
date: 2021-9-15
tags: SQL
author: 映雪
---

自来积毁骨能销，何况真红一点臂砂娇。

<!--more-->

![截屏2021-12-15 下午2.19.46.png](/images/2021/12/15/ZvKwhbkHUQSzdeo.png)

### UCASE()

- UCASE() 函数把字段的值转换为大写。

- user 列字母转换大写

```sql
SELECT UCASE(user) from user_data_copy2;
```

### LCASE()

- LCASE() 函数把字段的值转换为小写。

- user 列字母转换小写

```sql
SELECT LCASE(user) from user_data_copy2;
```

### MID()

- MID() 函数用于从文本字段中提取字符。
- MID() 语法

```sql
SELECT MID(column_name,start,length) FROM table_name;
```

- 提取 user 列 2 到 4 位

```sql
SELECT MID(user,2,2) as '用户名分割' FROM user_data_copy2;
```

### LENGTH()

- LENGTH() 函数返回文本字段中值的长度。

```sql
SELECT LENGTH(user) from user_data_copy2;

```

### ROUND()

- ROUND() 函数用于把数值字段舍入为指定的小数位数。

- ROUND() 语法

```sql
SELECT ROUND(column_name,decimals) FROM TABLE_NAME;
```

- 保留 num 列 2 位小数

```sql
SELECT ROUND(num,2) from user_data_copy2 where id <5;

```

### NOW()

- NOW() 函数返回当前系统的日期和时间。

```sql
SELECT NOW(),user,password from user_data_copy2 ;

```

### FORMAT()

- FORMAT() 函数用于对字段的显示进行格式化。

- SQL FORMAT() 语法

```sql
SELECT FORMAT(column_name,format) FROM table_name;
```

- 将 datetime 列转换为 yyyy-mm-dd

```sql
SELECT user, password, DATE_FORMAT(datetime,'%Y-%m-%d') AS date
FROM user_data_copy2;
```
