---
title: SQL-01-Basic
date: 2018-04-14 08:44:34
categories: SQL
tags:
- database
- SQL
---

# 基础知识总结

`为什么需要数据库`

这应该属于数据存储和查询下的一个子问题，数据存储到介质上之后还需要考虑如何快速精准地查询。这时候一般的存储方式，比如csv就会带来以下问题：

1. 读取和解析数据格式，需要编写查询用的代码
2. 不同程序访问数据的借口不同，代码复用性不高
3. 数据量增大之后，查询出目标数据需要复杂的逻辑处理
4. 大规模数据在普通存取过程中需要先把所有数据读入内存，这就客观上限制了文件的大小

为了解决这个问题，一中标准化的数据存储和查询方式就出现了。他提供了一套标准化的访问接口，编写代码时候，只要提供符合规定的语句即可。数据库（具体说，应该是数据库管理软件）内部会解析语句并返回结果。

`数据模型`

目前有三种常见数据模型：

- 层次模型
- 网状模型
- 关系模型

其中，关系模型应用最为广泛，是目前的主流。

`SQL`

SQL是Structured Query Language的简写，也就是上文中提到的标准化的查询数据库的语句。

# 数据库的存储方式

## 表(table)

存储某类数据的集合，比如用户信息、订单详情、产品目录等。

`表名`，同一个数据库内表的名称是独一无二的，不同数据库中可以包含相同的表名。

`模式(schema)`，定义了数据库/表的特性，比如存储了什么数据，数据如何分解等等。

## 列(column)

列又称为字段，用来存储某一种数据类型，可以类比为Excel里的列。值得注意的是，SQL查询数据是按列查询的，这一点可以在下一篇select的文章中看到。

数据存储类型

数据库表中的每个列都要求有名称和数据类型。Each column in a database table is required to have a name and a data type.

下面的表格列出了 SQL 中通用的数据类型：

| 数据类型                          | 描述                                                         |
| --------------------------------- | ------------------------------------------------------------ |
| CHARACTER(n)                      | 字符/字符串。固定长度 n。                                    |
| VARCHAR(n) 或CHARACTER VARYING(n) | 字符/字符串。可变长度。最大长度 n。                          |
| BINARY(n)                         | 二进制串。固定长度 n。                                       |
| BOOLEAN                           | 存储 TRUE 或 FALSE 值                                        |
| VARBINARY(n) 或BINARY VARYING(n)  | 二进制串。可变长度。最大长度 n。                             |
| INTEGER(p)                        | 整数值（没有小数点）。精度 p。                               |
| SMALLINT                          | 整数值（没有小数点）。精度 5。                               |
| INTEGER                           | 整数值（没有小数点）。精度 10。                              |
| BIGINT                            | 整数值（没有小数点）。精度 19。                              |
| DECIMAL(p,s)                      | 精确数值，精度 p，小数点后位数 s。例如：decimal(5,2) 是一个小数点前有 3 位数小数点后有 2 位数的数字。 |
| NUMERIC(p,s)                      | 精确数值，精度 p，小数点后位数 s。（与 DECIMAL 相同）        |
| FLOAT(p)                          | 近似数值，尾数精度 p。一个采用以 10 为基数的指数计数法的浮点数。该类型的 size 参数由一个指定最小精度的单一数字组成。 |
| REAL                              | 近似数值，尾数精度 7。                                       |
| FLOAT                             | 近似数值，尾数精度 16。                                      |
| DOUBLE PRECISION                  | 近似数值，尾数精度 16。                                      |
| DATE                              | 存储年、月、日的值。                                         |
| TIME                              | 存储小时、分、秒的值。                                       |
| TIMESTAMP                         | 存储年、月、日、小时、分、秒的值。                           |
| INTERVAL                          | 由一些整数字段组成，代表一段时间，取决于区间的类型。         |
| ARRAY                             | 元素的固定长度的有序集合                                     |
| MULTISET                          | 元素的可变长度的无序集合                                     |
| XML                               | 存储 XML 数据                                                |

------

## SQL 数据类型快速参考

然而，不同的数据库对数据类型的定义有所不同。

下面的表格显示了各种不同的数据库平台上一些数据类型的通用名称：

| 数据类型            | Access                 | SQLServer                                          | Oracle          | MySQL      | PostgreSQL      |
| ------------------- | ---------------------- | -------------------------------------------------- | --------------- | ---------- | --------------- |
| *boolean*           | Yes/No                 | Bit                                                | Byte            | N/A        | Boolean         |
| *integer*           | Number (integer)       | Int                                                | Number          | IntInteger | IntInteger      |
| *float*             | Number (single)        | FloatReal                                          | Number          | Float      | Numeric         |
| *currency*          | Currency               | Money                                              | N/A             | N/A        | Money           |
| *string (fixed)*    | N/A                    | Char                                               | Char            | Char       | Char            |
| *string (variable)* | Text (<256)Memo (65k+) | Varchar                                            | VarcharVarchar2 | Varchar    | Varchar         |
| *binary object*     | OLE Object Memo        | Binary (fixed up to 8K)Varbinary (<8K)Image (<2GB) | LongRaw         | BlobText   | BinaryVarbinary |

值得注意的是，即使数据类型名称相同，在不同数据库系统中的尺寸和细节也有所区别。

## 行(row)

SQL数据是按行存储的，类似于Excel的一行数据。**(查询的时候是按列查询的)**

行有时候也被称作记录(record)，但是标准叫法还是行(row)。

## 主键(primary key)

表的每一行必须有一列或者多列可以唯一标识自己，不然也就无法快速精准定位到目标位置。常见的方法是设计一个id列唯一标识某一行，或者主id加从id的方式来复合定位。此时主id可以作为类别(可以重复)，从id细分到每行数据。

# 参考

SQL 通用数据类型

http://www.runoob.com/sql/sql-datatypes-general.html