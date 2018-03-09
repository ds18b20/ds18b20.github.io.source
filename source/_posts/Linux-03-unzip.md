---
title: Git-07-RM
date: 2018-3-7 23:09:00
categories: Linux
tags:
- Linux
- Ubuntu
- unzip
---

# zip

**unzip ./FileName.zip**

ubuntu默认安装unzip工具



# rar

安装unrar，sudo apt install unrar



解压缩，unrar x ./FileName.rar

---

$unrar --help

用法: 

```
unrar <command> -<switch 1> -<switch N> <archive> <files...><@listfiles...> <path_to_extract\>
```


<命令>
  e             解压文件到当前目录
  l[t,b]        列出压缩文档信息[technical, bare]
  p             打印文件到标准输出
  t             测试压缩我俄当
  v[t,b]       列出压缩文档的详细信息[technical,bare]
  x             解压文件到完整路径 

---



卸载unrar，sudo apt remove unrar