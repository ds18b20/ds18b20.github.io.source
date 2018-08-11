---
title: SQL-02-Syntax
date: 2018-08-11 09:18:32
categories: SQL
tags:
- SQL
- Syntax

---

# MySQL常用语法

本篇介绍常用的MySQL语法。

## CREATE

创建数据库，创建表

## DROP

删除数据库，删除表

## DELETE

逐行删除记录

## TRUNCATE

一次性删除整张表

## grant

`grant select, insert, update, delete on database_name.* to 'user_name'@'127.0.0.1' identified by 'password';`
英文grant的本意即为：授予，准许
上述代码的解释如下，

- `database_name.*`名为database_name的所有表。用*作数据库名和表名的通配符。
- `'user_name'@'127.0.0.1'`host：'127.0.0.1'，可以使用%作通配符，例如'192.168.0.%'
- `identified by 'password'`通过密码'password'来授权

## A sample

手动创建SQL脚本来创建数据库和表。

```schema.sql
drop database if exists awesome;

create database awesome;

use awesome;

grant select, insert, update, delete on awesome.* to 'www-data'@'localhost' identified by 'www-data';

create table users (
    `id` varchar(50) not null,
    `email` varchar(50) not null,
    `passwd` varchar(50) not null,
    `admin` bool not null,
    `name` varchar(50) not null,
    `image` varchar(500) not null,
    `created_at` real not null,
    unique key `idx_email` (`email`),
    key `idx_created_at` (`created_at`),
    primary key (`id`)
) engine=innodb default charset=utf8;

create table blogs (
    `id` varchar(50) not null,
    `user_id` varchar(50) not null,
    `user_name` varchar(50) not null,
    `user_image` varchar(500) not null,
    `name` varchar(50) not null,
    `summary` varchar(200) not null,
    `content` mediumtext not null,
    `created_at` real not null,
    key `idx_created_at` (`created_at`),
    primary key (`id`)
) engine=innodb default charset=utf8;

create table comments (
    `id` varchar(50) not null,
    `blog_id` varchar(50) not null,
    `user_id` varchar(50) not null,
    `user_name` varchar(50) not null,
    `user_image` varchar(500) not null,
    `content` mediumtext not null,
    `created_at` real not null,
    key `idx_created_at` (`created_at`),
    primary key (`id`)
) engine=innodb default charset=utf8;
```

保存为schema.sql
然后打开Terminal，运行脚本：
`mysql -u root -p < schema.sql`即可按脚本内容自动创建库和表。
当表的数量很多时，可以使用代码自动生成脚本然后运行。

# Reference

http://www.mamicode.com/info-detail-1706157.html
https://blog.csdn.net/lampsunny/article/details/7410657