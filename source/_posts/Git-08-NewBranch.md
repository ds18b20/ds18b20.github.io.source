---
title: Git-08-NewBranch
date: 2018-01-08 20:20:23
categories: Git
tags:
- Git
- New Branch
---

# Git New Branch

Use `git branch` to show present branches.

Use `git branch <name>` to create a new branch called <name>.

Use `git checkout <name>` to switch to <name> branch.

Use `git checkout -b <name>` to create new and switch to it.

Use `git merge <name>` to merge <name> branch to the present branch. So `checkout` to the present branch at first. By default, Git will merge in **fast-forward** mode, and new branch information will be lost when you delete this new branch. You can use force Git not to use fast-forward mode by `git merge --no-ff -m "comment here" <branchName>`, and you can still see branch by `git branch`.

Use `git branch -d <name>` to delete <name> branch.