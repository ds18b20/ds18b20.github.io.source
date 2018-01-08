---
title: SWapi-04-SketchManager&FeatureManager
date: 2017-10-07 13:21:34
categories: SolidWorks API
tags:
- SolidWorks
- API

---

# 如何使用SketchManager和FeatureManager

## API Class构成

`Level 1:SldWorks(swApp)`

* `Level 2:ModelDoc2(Part)`
  * `Level 3:SketchManager(swSketchManager)`
  * `Level 3:FeatureManager(swFeatureManager)`

<!--more-->

## SketchManager Class

* Insert/Close Sketch
* Creates:
  - Lines
  - Centerlines
  - Arcs
  - Circle
  - Splines--样条曲线
  - Offsets

手动操作时，在SW操作界面下切换到“草图”功能选项卡，可以进行草图绘制。

使用API时，SketchManager Class可以实现除了“尺寸”之外所有的功能。

API Help文件中有定义：

```VB
Dim instance As IModelDoc2
Dim value As SketchManager

value = instance.SketchManager
```

### API绘制草图

下面这段代码实现了**新建零件--选择视图--绘制Circle**

```VB
Option Explicit
' ******************************************************************************
' 09/27/17 by ds18b20
' ******************************************************************************
Dim swApp As SldWorks.SldWorks
Dim Part As SldWorks.ModelDoc2

Dim boolstatus As Boolean
Dim longstatus As Long, longwarnings As Long

Sub main()
    Set swApp = Application.SldWorks
    Set Part = swApp.NewDocument("C:\ProgramData\SolidWorks\SOLIDWORKS 2016\templates\gb_part.prtdot", 0, 0, 0)
    swApp.ActivateDoc2 "零件2", False, longstatus
    Set Part = swApp.ActiveDoc
    Dim myModelView As Object
    Set myModelView = Part.ActiveView
    myModelView.FrameState = swWindowState_e.swWindowMaximized
    boolstatus = Part.Extension.SelectByID2("前视基准面", "PLANE", 0, 0, 0, False, 0, Nothing, 0)
    Part.SketchManager.InsertSketch True

    'Define Variables
    Dim CylinderLength As Double
    Dim CylinderDia As Double
   
    'Inputs User Values
    CylinderLength = 12 / 1000
    CylinderDia = 10 / 1000
   
    'Creating the SketchManager
    Dim swSketchManager As SketchManager
    Set swSketchManager = Part.SketchManager
    
    'Create Circle
    Dim TheCircle As SketchSegment
    Set TheCircle = swSketchManager.CreateCircle(0, 0, 0, CylinderDia / 2, 0, 0)
    
    'Part.ClearSelection2 True
    Part.SketchManager.InsertSketch True
    
End Sub
```

## FeatureManager Class

类似地，使用API时FeatureManager Class可以实现“特征”选项卡下的各种功能。

API Help文件中有定义：

```VB
Dim instance As IModelDoc2
Dim value As FeatureManager

value = instance.FeatureManager
```

### 拉伸实体

下面这段代码实现了新建零件--选择视图--绘制Circle--**拉伸**

```VB
Option Explicit
' ******************************************************************************
' 09/27/17 by ds18b20
' ******************************************************************************
Dim swApp As SldWorks.SldWorks
Dim Part As SldWorks.ModelDoc2

Dim boolstatus As Boolean
Dim longstatus As Long, longwarnings As Long

Sub main()
    Set swApp = Application.SldWorks
    Set Part = swApp.NewDocument("C:\ProgramData\SolidWorks\SOLIDWORKS 2016\templates\gb_part.prtdot", 0, 0, 0)
    swApp.ActivateDoc2 "零件2", False, longstatus
    Set Part = swApp.ActiveDoc
    Dim myModelView As Object
    Set myModelView = Part.ActiveView
    myModelView.FrameState = swWindowState_e.swWindowMaximized
    boolstatus = Part.Extension.SelectByID2("前视基准面", "PLANE", 0, 0, 0, False, 0, Nothing, 0)
    Part.SketchManager.InsertSketch True

    '=====================================
    'Define Variables
    Dim CylinderLength As Double
    Dim CylinderDia As Double
    
    '=====================================
    'Inputs User Values
    CylinderLength = 12 / 1000
    CylinderDia = 10 / 1000
    '=====================================
    'Creating the SketchManager
    Dim swSketchManager As SketchManager
    Set swSketchManager = Part.SketchManager
    
    '=====================================
    'Create Circle
    Dim TheCircle As SketchSegment
    Set TheCircle = swSketchManager.CreateCircle(0, 0, 0, CylinderDia / 2, 0, 0)
    Part.SketchManager.InsertSketch True
    
    '=====================================
    'create feature manager instance
    Dim swFeatureManager As FeatureManager
    Set swFeatureManager = Part.FeatureManager

    'extrude the circle to a boss
    Dim Boss As Feature
    Set Boss = swFeatureManager.FeatureExtrusion2(True, False, False, swEndCondBlind, swEndCondBlind, CylinderLength, 0, False, False, False, False, 0, 0, False, False, False, False, False, False, False, False, False, False)
    
End Sub
```

下一篇介绍如何使用UserForm来提高API应用的灵活性。

