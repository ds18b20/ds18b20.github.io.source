---
title: Git-09-Push
date: 2018-01-08 20:22:59
categories: Git
tags:
- Git
- Push
---

# Git Push

`git push` command is used to push file from **local repo** to **remote repo**.

Basically, you can use push command by

```
git push <remote repo name> <local branch name> <remote branch name>
# example:
# (1st master is local master branch)
# (2nd master is remote master branch)
git push origin master : refs/for/master
```

There are 2 ways to use this command.

## I.connect local repo with remote repo

1. Create a repo on your Github.

2. Create a local repo on your PC.

3. Name a remote repo as `origin`

under your local repo directory:

```
# origin: remote repo name
git remote add origin git@github.com:yourID/project.git
```

to add a remote repo called `origin`.


4. First push:

```
git push -u origin master
```

Push local repo to remote repo. Here `-u` is to relate **local master branch** with **remote master branch**.

Later, you can just enter

```
git push origin master
```

to push local master branch with the remote branch, that has been related.

If you want to push the present branch to remote, and the present branch has been related, just enter:

```
git push origin
```

If present repo has only one branch, remote repo name can be omitted.

```
# use git branch -r
git push
```

5. `git push` command can be used to delete a branch

```
git push origin : <remote branch name>
```

When push a local repo, if you omit the local branch, remote branch will be deleted.

6. Create new remote branch using `git push`

```
git push origin <local branch name> : <new branch name>
```

## II.clone from remote repo

1. clone

```
git clone <URL>
```

2. push

```
git push origin master
```

## About refs/for



## reference

https://www.cnblogs.com/qianqiannian/p/6008140.html

https://www.cnblogs.com/springbarley/archive/2012/11/03/2752984.html

fetch + merge > pull

http://www.cnblogs.com/flying_bat/p/3408634.html
