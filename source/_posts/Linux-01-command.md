---
title: Git-07-RM
date: 2018-3-7 22:43:00
categories: Linux
tags:
- Linux
- Ubuntu
- Basic Command
---

# 删除文件夹

rm

```
# 直接rm 文件夹
rm foldername
```
以上命令仅限于文件夹为空的情况。

当文件夹不为空时，加上参数-rf即可。

```
# 加上参数-rf
rm -rf foldername
```

-r表示向下递归，无论有多少下级目录，一并删除。

-f表示强行删除，没有提示信息。



# 新建文件

```
# add a new file
touch filename
# new as root
sudo touch filename
```

# 编辑文件

```
# edit by gedit
sudo gedit filename
```

# 移动文件

```
# mv 源 目的地
mv source destination
```

# 拷贝文件

```
# cp [选项] 源文件或目录 目标文件或目录 
cp -i abc.c /usr/ds18b20 
```

-i参数，当目标目录存在同名文件时，询问是否覆盖。

-r参数，当源是目录时，向下复制整个目录。

# 链接

链接分为**软链接**和**硬链接**两种

软链接类似windows下的快捷方式。

```
# ln -s 源文件 目标文件
ln -s filename /usr/local/bin/less
```

硬链接完整地复制一份源文件到目标地址。

去掉-s参数得到的就是硬链接。

无论是软链接还是硬链接，源地址的文件和目标地址的文件都能保持同步。

