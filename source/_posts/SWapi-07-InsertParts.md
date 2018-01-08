---
title: SWapi-07-InsertParts
date: 2017-10-09 23:04:48
tags:
- SolidWorks
- API

---

# 向Assembly中添加Parts的注意事项

The specified file must be loaded in memory. A file is loaded into memory when you load the file in your SolidWorks session ( **SldWorks::OpenDoc6** ) or open an assembly that already contains the file.

**即在AddComponent之前需要用OpenDoc6把Parts装入Memory**

参考代码：<!--more-->

```vb
Dim swApp As Object
Dim Part As Object
Dim boolstatus As Boolean
Dim longstatus As Long, longwarnings As Long
 
Sub main()
 
Set swApp = Application.SldWorks
 
'open / load part file
Set Part = swApp.OpenDoc6("C:\Solidworks-macros\tubular dowel_pcs2b.sldprt", 1, 0, "Default", longstatus, longwarnings)
swApp.ActivateDoc2 "tubular dowel_pcs2b.sldprt", False, longstatus
 
'start new assembly file
Set Part = swApp.NewDocument("C:\ProgramData\SolidWorks\SolidWorks 2013\templates\Assembly.asmdot", 0, 0, 0)
swApp.ActivateDoc2 "Assem#", False, longstatus
Set Part = swApp.ActiveDoc
 
'insert the part into assembly
boolstatus = Part.AddComponent("C:\Solidworks-macros\tubular dowel_pcs2b.sldprt", 0, 0, 0)
Set Part = swApp.ActiveDoc
 
'close the part file
swApp.ActivateDoc2 "tubular dowel_pcs2b.sldprt", False, longstatus
swApp.CloseDoc "tubular dowel_pcs2b.sldprt"
 
'set assembly as active document
swApp.ActivateDoc2 "Assem9", False, longstatus
Set Part = swApp.ActiveDoc
 
End Sub
```

