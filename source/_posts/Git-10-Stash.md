---
title: Git-08-Stash
date: 2018-01-08 20:25:53
categories: Git
tags:
- Git
- Stash
---

# Git Stash

If you are working on a branch called **Ver1.0**, and you find a bug in file **AAA** on **master branch** that need to be fixed immediately.

You can create a new **bug branch** from master, work on this **bug branch** to fix the bug, commit it and switch to master branch and merge the modification. If you commit them all, you will get too many unuseful commits.

But when you want to add/commit, some modification will be add/commit to **bug branch** if **Ver1.0** is NOT clean. This modification is not what the **bug branch** should have.

Before you switch to another branch, note that if your working directory or staging area has uncommitted changes that conflict with the branch you’re checking out, Git won’t let you switch branches. So, keep it CLEAN.

Or, you can stash working directory to **Ver1.0** branch by

```
git stash
```

If you enter `git status`, you will find the working area is already CLEAN. All modification should be added, but don'n need to be committed.

```
# on branch Ver1.0
git stash

# create branch bug from master
git checkout master
git checkout -b bug

### fix the bug ###
git add AAA
git commit AAA -m "fix bug"

# merge/del bug
git checkout master
git merge bug
git branch -d bug

# switch to branch Ver1.0
git checkout Ver1.0
git stash pop

# or
git stash list
git stash apply stash@{0}
git stash drop
```

Reference:

https://segmentfault.com/q/1010000000156026

https://git-scm.com/book/zh/v1/Git-%E5%88%86%E6%94%AF-%E5%88%86%E6%94%AF%E7%9A%84%E6%96%B0%E5%BB%BA%E4%B8%8E%E5%90%88%E5%B9%B6