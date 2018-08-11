---
title: SQL-MySQL-Setup-Mac
date: 2018-08-01 20:59:07
categories: SQL
tags:
- MySQL
- setup
- Mac

---

# MySQL的安装和配置

这篇主要记录在Mac平台上安装MySQL的过程。

## 下载和安装

到MySQL官网下载Community版的Mac安装文件。
我自己下载的是.dmg文件，双击打开可以看到一个pkg文件，双击这个pkg文件打开。
最后一直点下一步知道安装结束。

## 停止MySQL服务

如果已经启动来MySQL服务，需要先打开Mac的`系统偏好设置`。

在最下面找到MySQL的设置按钮，进入设置并关闭MySQL服务。

## 设置密码

安装后默认没有设置密码，Terminal内键入：`/usr/local/mysql/bin/mysqladmin -u root password`
按照提示两次键入root账户的密码。

为防止遗忘，此处设置为root。

## 添加系统环境变量

此时，Terminal内键入`mysql`会提示找不到命令的错误信息：`-bash: mysql: command not found`，因为$PATH系统变量内没有记录MySQL的安装目录。

> 查看 $PATH的方法：
>
> echo $PATH
>
> /anaconda3/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin

解决方法：

1. 在Terminal内`export PATH="/usr/local/mysql/bin:$PATH"`，但是关闭Terminal后设置消失。
2. If you modify the PATH variable in a Terminal session by exporting it (e.g. export PATH="/usr/local/mysql/bin:$PATH" it will be expired in the next session.
   So either edit the file ~/.bash_profile or edit /private/etc/paths if you need this PATH for other users also.
   修改文件的具体操作如下：

```
cd ~
vim .bash_profile
```

添加两行，
`alias mysql=’/usr/local/mysql/bin/mysql’` 
`alias mysqladmin=’/usr/local/mysql/bin/mysqladmin’` 

或者添加`export PATH="/usr/local/mysql/bin:$PATH"`
然后保存。

再次查看 $PATH确认MySQL所在目录已经被添加：
`echo $PATH`

`/usr/local/mysql/bin:/anaconda3/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin`

**重启终端**输入mysql即可显示欢迎界面。

## 修改默认字符集

涉及到多语言问题时，为了防止显示乱码需要修改默认字符集。
首先，检查安装后的默认设置。
Terminal内键入mysql进入mysql环境，然后键入`show variables like '%char%';`
得到如下结果，

```
+--------------------------+-----------------------------------------------------------+
| Variable_name            | Value                                                     |
+--------------------------+-----------------------------------------------------------+
| character_set_client     | utf8                                                      |
| character_set_connection | utf8                                                      |
| character_set_database   | latin1                                                    |
| character_set_filesystem | binary                                                    |
| character_set_results    | utf8                                                      |
| character_set_server     | latin1                                                    |
| character_set_system     | utf8                                                      |
| character_sets_dir       | /usr/local/mysql-5.6.41-macos10.13-x86_64/share/charsets/ |
+--------------------------+-----------------------------------------------------------+
8 rows in set (0.03 sec)
```

可以看到haracter_set_database和character_set_server并不是utf8字符集。接下来要把这两项修改成utf8格式。
首先，在系统便好设置里关闭MySQL服务，然后修改配置文件。
Mac下MySQL的默认配置文件位置在`/etc/my.cnf`或者`/etc/mysql/my.cnf`。
如果没有的话，需要从`/usr/local/mysql/support-files/my-default.cnf`处拷贝过来：
`sudo cp /usr/local/mysql/support-files/my-default.cnf /etc/my.cnf`
然后`sudo vi /etc/my.cnf`，做如下修改：
[client]部分加入：
`default-character-set=utf8`

[mysqld]部分加入：
`character-set-server=utf8`

修改完毕之后记得重启MySQL服务。

这时再次查看设置：`mysql> show variables like '%char%';`
得到，

```
+--------------------------+-----------------------------------------------------------+
| Variable_name            | Value                                                     |
+--------------------------+-----------------------------------------------------------+
| character_set_client     | utf8                                                      |
| character_set_connection | utf8                                                      |
| character_set_database   | utf8                                                      |
| character_set_filesystem | binary                                                    |
| character_set_results    | utf8                                                      |
| character_set_server     | utf8                                                      |
| character_set_system     | utf8                                                      |
| character_sets_dir       | /usr/local/mysql-5.6.41-macos10.13-x86_64/share/charsets/ |
+--------------------------+-----------------------------------------------------------+
8 rows in set (0.02 sec)
```

修改my.conf配置文件时可以同时修改一下内容：
[mysqld]
`default-storage-engine = INNODB`
`collation-server = utf8_general_ci`

## 忘记初始密码时的操作

https://blog.csdn.net/u014410695/article/details/50630233

# Reference

https://apple.stackexchange.com/questions/287476/the-new-created-terminal-can-not-find-the-file-use-which-in-mac
https://www.cnblogs.com/mfmdaoyou/p/7233394.html
https://blog.csdn.net/Waleking/article/details/7620983

