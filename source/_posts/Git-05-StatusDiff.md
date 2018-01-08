---
title: Git-05-StatusDiff
date: 2017-12-19 00:35:20
categories: Git
tags:
- Git
- Status
- Diff
---

# Git Status & Diff

## git status

Show git repository status.

If nothing changed:

```
$ git status
On branch master
nothing to commit, working tree clean
```

If file is modified and not be staged:

```
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   test.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

If file is modified and staged:

```
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   test.txt


```

## git diff

If file is modified and not be staged:

```
$ git diff
diff --git a/test.txt b/test.txt
index 27a13b8..30d9986 100644
--- a/test.txt
+++ b/test.txt
@@ -1 +1,2 @@
-line 0
\ No newline at end of file
+line 0
+line 1
\ No newline at end of file
```

