---
title: Git-04-Add
date: 2017-12-19 00:34:11
categories: Git
tags:
- Git
- Add
---

# Git Add

## git add .

他会监控工作区的状态树，使用它会把工作时的所有变化提交到暂存区，包括文件内容修改(modified)以及新文件(new)，但不包括被删除的文件。

## git add -u

他仅监控已经被add的文件（即tracked file），他会将被修改的文件提交到暂存区。add -u 不会提交新文件（untracked file）。（git add --update的缩写）

## git add -A

是上面两个功能的合集（git add --all的缩写）



[https://www.cnblogs.com/skura23/p/5859243.html](https://www.cnblogs.com/skura23/p/5859243.html)