---
title: SQL-03-Commit
date: 2018-04-15 16:55:54
categories: SQL
tags:
- SQL
- commit
---

# SQL语言分类

按照COMMIT和ROLLBACK的用法来分类时，SQL语言可以分成DDL，DML和DCL三大类。

1. DDL（Data Definition Language） 数据定义语言

   用于定义和管理 SQL 数据库中的所有对象的语言 。

   CREATE---创建表

   ALTER---修改表

   DROP---删除表

2.  DML（Data Manipulation Language） 数据操纵语言

   SQL中具体操作数据的语言。 

   INSERT---数据的插入

   DELETE---数据的删除

   UPDATE---数据的修改

   SELECT---数据的查询

3. DCL（Data Control Language）数据控制语言

   用来授予或回收访问数据库的某种特权，并控制数据库操纵事务发生的时间及效果，对数据库实行监视等。

   GRANT---授权。

   ROLLBACK---回滚

   COMMIT---提交。 

# 需要Commit的情况

在上述DML语言之后，需要执行COMMIT / ROLLBACK把数据从内存写到硬盘上。具体说来，DML语句执行之后数据转移到回滚段(除了SELECT)，等待用户进行提交(COMMIT)或者回滚(ROLLBACK)后，放在回滚段中的数据会被清除。**Commit可以使用Python的上下文功能来实现，具体见参考2。**

SELECT语句执行后，数据暂存在共享池。当其他人查询相同数据时，可以直接从共享池中提取，而不用重新从数据库中查询，从而提高了查询速度。

   # Commit方式

提交(COMMIT)数据有三种类型：

1. 显式提交

   直接显式的写出COMMIT命令来完成提交

2. 隐式提交

   用SQL命令间接完成的提交为隐式提交。这些命令是：

   ALTER，AUDIT，COMMENT，CONNECT，CREATE，DISCONNECT，DROP，EXIT，GRANT，NOAUDIT，QUIT，REVOKE，RENAME

3. 自动提交

   若把AUTOCOMMIT设置为ON，则在插入、修改、删除语句执行后，

   系统将自动进行提交，这就是自动提交。其格式为： SQL>SET AUTOCOMMIT ON；

# 参考

COMMIT和ROLLBACK的用法

https://blog.csdn.net/yjtgod/article/details/9447379

You do have to commit after inserting:

https://stackoverflow.com/questions/22488763/sqlite-insert-query-not-working-with-python