---
title: Git-03-LocalRepository
date: 2017-12-19 00:32:45
categories: Git
tags:
- Git
- Local Repository
---

# Git Local Repository

## Initialize a Git repository

```
# change to the directory where you want to create a repository
cd Your Directory

# create a git repository
git init
```

## Submit a file to your repository

1. add

   ```
   # one file
   add A.txt

   # multi-files(divided by spaces)
   add B.txt C.txt D.txt
   ```


1. commit

   ```
   # -m "log"
   git commit -"Your Comment"
   ```