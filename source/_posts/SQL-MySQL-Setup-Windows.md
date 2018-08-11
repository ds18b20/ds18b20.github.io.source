---
title: SQL-MySQL-Setup-Windows
date: 2018-08-01 20:59:25
categories: SQL
tags:
- MySQL
- setup
- Windows

---

# Windows非安装版MySQL

下面介绍Windows系统下非安装版的MySQL的配置方法。

## 下载zip版文件

去官方网站下载zip版的压缩文件，解压缩到指定位置，此位置即MySQL的安装目录。这里我的安装目录是`C:\Program Files\mysql-5.6`。

## 配置设置文件

默认在安装目录下有名为`my-default.ini`的配置文件。将此文件复制一份，然后更名为`my.ini`。

打开`my.ini`，做如下修改：

```
[mysql]
# 设置mysql客户端默认字符集
default-character-set=utf8
[mysqld]
#设置3306端口
port=3306
# 设置mysql的安装目录
basedir=C:\Program Files\mysql-5.6.30-winx64
# 设置mysql数据库的数据的存放目录
datadir=C:\Program Files\mysql-5.6.30-winx64\data
# 允许最大连接数
max_connections=200
# 服务端使用的字符集默认为8比特编码的latin1字符集
character-set-server=utf8
# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB
```

注意，

- basedir和datadir根据自身情况设置
- 文件以ANSI形式保存，所以上述配置文件中的#中文部分应该删除。

## 修改系统环境变量

在系统环境变量PATH里添加MySQL的安装目录。

## 安装

1. 以Administrtor身份启动CMD；
2. cd到上述安装目录，然后键入`mysqld install`。（注意是mysqld）如果没有管理员权限会提示`Install/Remove of the Service Denied!`，正常安装后提示Service `successfully installed.`

## 启动MySQL服务

启动服务命令：`net start mysql`

关闭服务命令：`net stop mysql`

## 运行mysql

键入`mysql -u root -p`，默认密码为空，所以提示输入密码时，直接Enter。

# 参考

[MySQL服务器安装配置-非安装版、windows版](https://www.cnblogs.com/alunchen/p/5526903.html)

