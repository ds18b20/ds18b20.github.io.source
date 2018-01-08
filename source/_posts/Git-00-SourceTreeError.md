---
title: Git-00-SourceTreeError
date: 2017-12-19 00:27:49
categories: Git
tags:
- Git
- SourceTree
---

# SourceTree error

Mark some bug/error when using Git soft SourceTree.

## "Authentication via SSH keys failed"

When using SourceTree to clone a project from github, you may get this error of "通过SSH密钥认证失败", or "Authentication via SSH keys failed". 

Just change the setting like:

`Tool -> Option -> SSH client Setting` and change SSH client to `OpenSSH`.

`工具->选项->SSH客户端配置` ,将SSH客户端选择OpenSSH, 再确认.