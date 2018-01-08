---
title: Hexo Basic
date: 2017-09-24 11:40:51
categories: Hexo&Markdown
tags: 
- Hexo
- Markdown
---

**今天整理一下在Hexo中如何新建文章，包括post/page/draft**

## 新建post文章

命令行下输入：

```Hexo
hexo new "new"
```

执行命令后在source/_post目录下，会生成一个名为new.md的markdown文件。

编辑md文件

文件的开头记录着文件属性，采用yaml格式标记。

<!--more-->

打开文件可以看到有如下记录：

---

>  title: Hexo添加新文章
>
>  date: 2017-09-24 11:40:51
>
>  tags: [Hexo,添加，文章]

---

文件主要有以下属性：

---

> layout    Layout    post或page
>
> title    文章的标题
>
> date    创建日期    文件的创建日期
>
> updated    修改日期    文件的修改日期
>
> comments    是否开启评论    true
>
> tags    标签
>
> categories    类别    如果文章没有指定，那么就是`default_category`字段设置的那个
>
> permalink    url中的名字    文件名

---

其中常用到有

**类别：**

>  categories:
>
>  \- 音乐
>
>  \- 天文

**标签：**

> tags:
>
> \- Hexo
>
> \- AI
>

**布局：**

> 如果修改了layout，在scaffolds文件夹里一定要有名字对应的模版文件，否则会采用默认模版

另外，可以设置摘要和草稿模式

**摘要：**

> <!--more-->之上的内容为摘要

## 新建page文章

以page模式下新建的文章不会在文章列表中列出，但是可以通过链接访问。

输入命令

```hexo
hexo new page new-page
```

则会在source/下新建/new-page/index.md

generate之后可以在通过your-domain/new-page浏览到此新建的page文章。

## 新建draft文章

**以草稿模式保存文章**，此模式下文章不会上传至服务器。

> 草稿相当于很多博客都有的“私密文章”功能。
>
> ```hexo
> $ hexo new draft "new draft"
> ```
>
> 此命令会在source/_drafts目录下生成一个名为new-draft.md文件。由于属于draft文章，所以不会发布到主页，也不能通过链接访问到。_
>
> 如果不想发布某篇文章，可以选择不删除，而是把它移动到_drafts目录之中。
>
> 如果你希望强行预览草稿，更改配置文件：
>
> ---
>
> render_drafts: true
>
> ---
>
> 或者，启动server时加上--drafts命令：
>
> ```
> $ hexo server --drafts
> ```
>
> 下面这条命令可以把草稿变成文章，或者页面：
>
> ```
> $ hexo publish [layout] <filename>
> ```