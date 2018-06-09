---
title: SQL-02-Select
date: 2018-04-14 10:26:43
categories: SQL
tags:
- SQL
- select
---

# 查询

select用于从数据库中按条件查询，返回一个二维表，这个二维表包含列名和每一行的数据。

## 基本查询命令

```sql
SELECT * FROM <表名>;
```

上述命令返回`<表名>`的所有列。

实际上，`FROM 表名`部分不是必需的，比如，

`SELECT 1+1;`

返回结果统一是一个二维表，列名：1+1，值：2

故可以使用这个小trick检测数据库是否连接成功。

## 条件查询

```sql
SELECT <列名> FROM <表名> WHERE <条件>;
SELECT <列名> FROM <表名> WHERE <条件1> AND <条件2>;
```

例如`SELECT * FROM students WHERE score >= 80 AND gender = 'M';`

## 投影查询

```sql
SELECT 列1, 列2, 列3 FROM ...
SELECT 列1 别名1, 列2 别名2, 列3 别名3 FROM ...
```

指定列名返回结果；

还可以给列重新命名。

## 排序

```sql
SELECT <列名> FROM <表名> WHERE <条件> ORDER BY <列名>;
SELECT <列名> FROM <表名> WHERE <条件> ORDER BY <列名1> <列名2>;
SELECT <列名> FROM <表名> WHERE <条件> ORDER BY <列名> DESC;
```

在基本查询语句后加上`ORDER BY <列名>`，默认为从低到高排序；

加上多个列名来实现多重排序；

加上`DESC`来从高到低排序。

## 聚合查询

```sql
SELECT COUNT(*) FROM <表名> WHERE ...；
```

除了COUNT()之外还有其他聚合函数，

| 函数 | 说明                                   |
| ---- | -------------------------------------- |
| SUM  | 计算某一列的合计值，该列必须为数值类型 |
| AVG  | 计算某一列的平均值，该列必须为数值类型 |
| MAX  | 计算某一列的最大值                     |
| MIN  | 计算某一列的最小值                     |

注意，MAX和MIN并不局限于数值类型。如果是字符串类型，MAX返回排序最后的字符，MIN相反。

另外，聚合查询的WHERE条件如果没有匹配到任何结果，那么COUNT会返回0，而MAX/MIN/AVG/SUM会返回null。

## 分组

```sql
SELECT class_id, COUNT(*) num FROM students WHERE class_id = 1;
SELECT class_id, COUNT(*) num FROM students WHERE class_id = 2;
...
SELECT class_id, COUNT(*) num FROM students WHERE class_id = n;
```

可以使用GROUP BY分组功能简化上述操作，

```sql
SELECT class_id, COUNT(*) num FROM students GROUP BY class_id;
```

同样，GROUP BY后可以加多个分组列名，实现多重分组。

**注意：聚合查询的<查询列>中，只能放入分组的列，即GROUP BY后面的列。**

## 多表查询

SQL允许同时从多张表中查询数据，语法为，

```sql
SELECT * FROM <表1> <表2>
```

返回结果是一个二维表，**行数为表1和表2行数的乘积，列数为二者的和。**

为了避免各表中的列名重复，可以使用上文中投影查询中提到的方法，给列重命名。

```sql
SELECT
    students.id sid,
    students.name,
    students.gender,
    students.score,
    classes.id cid,
    classes.name cname
FROM students, classes;
```

也可以给表更名，

```sql
SELECT
    s.id sid,
    s.name,
    s.gender,
    s.score,
    c.id cid,
    c.name cname
FROM students s, classes c;
```

加上判断语句，

```sql
SELECT
    s.id sid,
    s.name,
    s.gender,
    s.score,
    c.id cid,
    c.name cname
FROM students s, classes c
WHERE s.gender = 'M' AND c.id = 1;
```

## 连接查询

```sql
SELECT s.id, s.name, s.class_id, c.name class_name, s.gender, s.score
FROM students s
INNER JOIN classes c
ON s.class_id = c.id;
```

INNER JOIN查询的写法是：

1. 先确定主表，仍然使用`FROM <表1>`的语法；
2. 再确定需要连接的表，使用`INNER JOIN <表2>`的语法；
3. 然后确定连接条件，使用`ON <条件...>`，这里的条件是`s.class_id = c.id`，表示`students`表的`class_id`列与`classes`表的`id`列相同的行需要连接；
4. 可选：加上`WHERE`子句、`ORDER BY`等子句。

ing。。。



# 参考

连接查询

https://liaoxuefeng.com/wiki/001508284671805d39d23243d884b8b99f440bfae87b0f4000/001509167103179399448cb200549bdab7651a5e9167597000