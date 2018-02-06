---
title: GoogleAPI-01-QuickStart
date: 2018-02-05 23:03:07
categories: Google API
tags:
- Google
- API
---

# 基本

Google drive 提供了一套API来读取操作保存在drive里的文件。可以upload/download文件，也可以给文件参加meta data来方便管理。

另外Google suite 中除了Google drive之外，其他应用也提供了各自的API。

# quick start

## Web应用（javascript）

https://developers.google.com/drive/v3/web/quickstart/js?authuser=1

连接测试成功之后，显示文件list：

![/GoogleAPI-01-QuickStart/WebOK.jpg](/GoogleAPI-01-QuickStart/WebOK.jpg)

## Python应用

https://developers.google.com/drive/v3/web/quickstart/python?authuser=1

使用Python连接时注意firewall的屏蔽问题。

![/GoogleAPI-01-QuickStart/blocked.jpg](/GoogleAPI-01-QuickStart/blocked.jpg)

# 其他参考文章

## Javascript获得文件列表

https://qiita.com/comefigo/items/72a254bad14edf6f621a

结果以行列显示

![/GoogleAPI-01-QuickStart/WebOKPre.jpg](/GoogleAPI-01-QuickStart/WebOKPre.jpg)

帐户受限时，会返回错误：

![/GoogleAPI-01-QuickStart/Error.jpg](/GoogleAPI-01-QuickStart/Error.jpg)

## 上传文件的例子

https://qiita.com/comefigo/items/37e0e7668166d494b922

尚未确认

