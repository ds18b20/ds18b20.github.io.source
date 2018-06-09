---
title: Python-SQL-01-SQLite
date: 2018-04-14 22:27:24
categories: SQL
tags:
- SQLite
---

# basic

SQLite是一个开源的嵌入式数据库，由于其数据库就是一个单独的文件，方便嵌入到各种应用程序中。而且SQLite本身是由C编写而成，所以速度快，体积小。

主要特点如下，

- 不需要一个单独的服务器进程或操作的系统（无服务器的）。

- SQLite 不需要配置，这意味着不需要安装或管理。

- 一个完整的 SQLite 数据库是存储在一个单一的跨平台的磁盘文件。

- SQLite 是非常小的，是轻量级的，完全配置时小于 400KiB，省略可选功能配置时小于250KiB。

- SQLite 是自给自足的，这意味着不需要任何外部的依赖。

- SQLite 事务是完全兼容 ACID 的，允许从多个进程或线程安全访问。

- SQLite 支持 SQL92（SQL2）标准的大多数查询语言的功能。

- SQLite 使用 ANSI-C 编写的，并提供了简单和易于使用的 API。

- SQLite 可在 UNIX（Linux, Mac OS-X, Android, iOS）和 Windows（Win32, WinCE, WinRT）中运行。

  https://wizardforcel.gitbooks.io/tutorialspoint-db/content/sqlite/61.html

# SQLite命令

与关系型数据库进行交互的标准SQLite命令类似于SQL命令。主要有CREATE，SELECT，INSERT，UPDATE，DELETE，和DROP。根据它们的操作性质可以分为以下三类：

## DDL - 数据定义语言

| 命令   | 描述                                                   |
| ------ | ------------------------------------------------------ |
| CREATE | 创建一个新的表，一个表的视图，或者数据库中的其他对象。 |
| ALTER  | 修改数据库中的某个已有的数据库对象，比如一个表。       |
| DROP   | 删除整个表，或者表的视图，或者数据库中的其他对象。     |

## DML - 数据操作语言

| 命令   | 描述           |
| ------ | -------------- |
| INSERT | 创建一条记录。 |
| UPDATE | 修改记录。     |
| DELETE | 删除记录。     |

## DQL - 数据查询语言

| 命令   | 描述                           |
| ------ | ------------------------------ |
| SELECT | 从一个或多个表中检索某些记录。 |

# Python操作SQLite

Python发行版本默认内置了操作SQLite的库sqlite3，使用前直接`import sqlite3`即可。

```python
import sqlite3

conn = sqlite3.connect('sqlite_test.db')
cursor = conn.cursor()

com = "SELECT COUNT(*) FROM sqlite_master WHERE type=\'table\' AND name=\'%s\'" % 'shop'
print(com)

cursor.execute(com)
values = cursor.fetchall()
print(values)

cursor.close()
conn.close()
```

操作数据库之前需要连接到数据库文件，这样的一个连接称为一个Connection。

成功连接到数据库之后，要执行SQL语句需要先打开游标(cursor)，通过游标执行语句。

操作完成之后必须关闭cursor和connect。**关于cursor详见参考**

像上面的例子中，查询语句包含参数时，可以使用？通配，并在逗号后按顺序加上参数组成的tuple。

```python
import sqlite3

conn = sqlite3.connect('sqlite_test.db')

cursor = conn.cursor()

cursor.execute("SELECT COUNT(*) FROM sqlite_master WHERE type=? AND name=?", ('table', 'shop'))
values = cursor.fetchall()
print(values)

cursor.close()
conn.close()
```

或者使用python自带的字符串格式化方法format来实现类似效果。

## 判断表是否存在

由于一个数据库文件中可以存储多个表，故有时需要判断某个表时候已经创建，这时候可以使用上面例子中的语句来实现。

`SELECT COUNT(*) FROM sqlite_master WHERE type='table' AND name='<Table Name>'`

返回结果为[(1,)]时，则该表<Table Name>已经存在。

类似地，如果要返回保存表的数目，可以去掉AND后面的name条件，`SELECT COUNT(*) FROM sqlite_master WHERE type='table'`

**注意，出现引号嵌套时，注意是否需要对引号进行转义处理。**

## sqlite_master表

上文中已经出现了sqlite_master这个表名，它储存了数据库中描述表的关键信息。

## 添加表

```sql
CREATE TABLE <表名> (id varchar(20) PRIMARY KEY, name varchar(20))
```

## 删除表

```sql
DROP TABLE <表名>
```

DROP TABLE会删除表的结构和储存的数据，而且属于SQL中的DDL，执行不需要COMMIT。执行后不能恢复，所以此命令要慎用。删除储存的数据时候可以使用TRUNCATE或者DELETE命令。**详见参考部分内容**

## 插入行

语法为`INSERT INTO TABLE_NAME (column1, column2, column3,...columnN)]  VALUES (value1, value2, value3,...valueN)`

或者插入完整的一行，`INSERT INTO TABLE_NAME VALUES (value1,value2,value3,...valueN)`，此时要注意value的顺序。

## 删除行

语法`DELETE FROM 表名称 WHERE 列名称 = 值`

删除所有行时，可以`DELETE FROM table_name`。虽然所有行内容被删除，但是表的结构，属性和索引都是完整的。

# 参考

在SqlServer存储过程中使用Cursor（游标）操作记录

https://blog.csdn.net/yixiaotian1988/article/details/7907431

Sql中truncate，delete以及drop的区别

https://www.cnblogs.com/baibo123/p/7906070.html