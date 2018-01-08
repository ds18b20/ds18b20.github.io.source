---
title: Git-06-CheckoutReset
date: 2017-12-19 00:36:19
categories: Git
tags:
- Git
- Checkout
- Reset
---

# Git Checkout & Reset

## Modified, not Added

```
git checkout -- <filename>
```

Return: file before modified 

## Added, not Committed

**not modified -> unstage**

```
git reset HEAD <filename>
```

return: file unstaged



**modified -> discard changes**

```
git checkout -- <filename>
```

return: file staged, but not modified

Then you can use `git checkout -- <filename>` to make repository clean.

## Committed

```
git reset --hard HEAD^
```

return: file before last commit



```
$ git log --pretty=oneline
225ac768ec0c079f03f37421038d63010aee734c line 1 commit
641da51982ad0a39c890032e4233af018601c1e8 line 0 commit

git reset --hard 225ac
```

return: file at 225ac...



If Git window is closed, use `reflog` to call history commands.

```
$ git reflog
225ac76 HEAD@{0}: reset: moving to HEAD
225ac76 HEAD@{1}: reset: moving to 225ac
641da51 HEAD@{2}: reset: moving to HEAD
641da51 HEAD@{3}: reset: moving to HEAD^
225ac76 HEAD@{4}: reset: moving to HEAD
225ac76 HEAD@{5}: reset: moving to 225ac
641da51 HEAD@{6}: reset: moving to HEAD^
225ac76 HEAD@{7}: reset: moving to HEAD
225ac76 HEAD@{8}: commit: line 1 commit
641da51 HEAD@{9}: commit (initial): line 0 commit
```

