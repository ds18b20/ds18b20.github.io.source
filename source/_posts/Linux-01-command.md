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

