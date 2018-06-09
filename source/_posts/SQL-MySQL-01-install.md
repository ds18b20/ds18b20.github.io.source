---
title: SQL-MySQL-01-install
date: 2018-04-15 19:58:21
categories: SQL
tags:
- MySQL
- install

---

# Install MySQL



## MySQL Installer



## zip安装

下面的Zip安装方法还没安装成功。。。

1. 从官网下载相应版本，https://dev.mysql.com/downloads/mysql/

2. 解压缩到指定目录

3. 管理员权限打开CMD，cd到上述的解压目录，然后执行以下命令：

   ```
   Microsoft Windows [版本 6.1.7601]
   版权所有 (c) 2009 Microsoft Corporation。保留所有权利。

   C:\Windows\system32>cd C:\Program Files\MySQL\bin

   C:\Program Files\MySQL\bin>mysqld -install
   Service successfully installed.

   C:\Program Files\MySQL\bin>net start mysql
   MySQL 服务正在启动 .
   MySQL 服务无法启动。

   服务没有报告任何错误。

   请键入 NET HELPMSG 3534 以获得更多的帮助。
   ```

   出现上述Error时，

   ```
   C:\Program Files\MySQL\bin>mysqld  --initialize

   C:\Program Files\MySQL\bin>net start mysql
   MySQL 服务正在启动 .
   MySQL 服务已经启动成功。
   ```

   ​





# 参考

MySQL安装

https://www.yiibai.com/mysql/install-mysql.html

安装MySQL提示“请键入 NET HELPMSG 3534 以获得更多的帮助”的解决办法

http://www.voidcn.com/article/p-ndtmhdmk-bkq.html