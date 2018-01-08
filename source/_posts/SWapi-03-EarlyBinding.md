---
title: SWapi-03-EarlyBinding
date: 2017-10-07 12:22:23
categories: SolidWorks API
tags:
- SolidWorks
- API

---

# Early Binding实现自动代码提示

## 自动代码提示

默认录制的Macro代码中可以看到：

```VB
Dim swApp As Object
Dim Part As Object
```

默认swApp和Part被定义为最基础的Object类，因此在函数中键入swApp.之后没有任何相关提示。

可以通过Early Binding实现自动提示。

```VB
Dim swApp As SldWorks.SldWorks
Dim Part As SldWorks.ModelDoc2
```

这样定义之后，再次输入swApp.之后则会自动提示类的内置函数和属性。

另外可以查看API Help文件来确定Early Binding为哪个类。

<!--more-->

关于I class和普通class的区别，可以查看：[https://forum.solidworks.com/thread/15144](https://forum.solidworks.com/thread/15144)

## 函数*SendMsgToUser*

函数定义如下：参考API Help文件

```
Dim instance As ISldWorks
Dim Message As String
 
instance.SendMsgToUser(Message)
```

具体使用时，

```VB
Dim swApp As SldWorks.SldWorks

Sub main()
	Set swApp = Application.SldWorks
	swApp.SendMsgToUser ("Hello")
End sub
```

