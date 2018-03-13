---
title: Java-Eclipse-02-ShortKeys
date: 2018-03-11 23:27:00
categories: Java
tags:
- Java
- eclipse
- short keys
---

This article shows short keys in eclipse.

# content assist

输入关键字，按`Alt+/`即可显示内容助理(content assist)。双击所需的内容可以自动填充相关代码，非常方便。

比如class定义时，class内部输入main，

```java
public class Sample3_1 {
	main
}
```

然后按下`Alt+/`，按照提示选择`main method`，则会自动填充以下内容：

```java
public class Sample3_1 {
	public static void main(String[] args) {
		...
	}

}
```



set here:

*Window>Preferences>Java>Editor>Content Assist>Advanced* 

reference:

Eclipse自动补全+常用快捷键

http://blog.csdn.net/bd_matto/article/details/50899032

In my case

https://stackoverflow.com/questions/23986750/ctrl-space-not-working-for-content-assist-on-eclipse



# copy to up/below

`Ctrl+Alt+UP/DOWN`

如果此快捷键和Ubuntu的屏幕切换快捷键冲突的话,参考下面的链接修改Ubuntu快捷键：

http://blog.csdn.net/hanshileiai/article/details/46412641

Alt+F2键调出命令框，输入

```
gsettings set org.gnome.desktop.wm.keybindings switch-to-workspace-down "['']"
gsettings set org.gnome.desktop.wm.keybindings switch-to-workspace-up "['']"
```

# 自动format

`Ctrl+Shift+F`将一段未format的代码自动缩进。

或者选择代码段，右键---Source code---Format也可以实现相同的功能。