---
title: Git-11-untrackFile
date: 2018-09-01 11:21:01
categories: Git
tags:
- untrack
- file

---

# 停止追踪新文件

修改.gitignore文件来实现忽略对未add的文件的追踪。

# 取消对已添加的文件的追踪

对于已经add的文件，停止对其追踪需要使用`git rm --cached <file>`来实现。