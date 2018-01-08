---
title: SWapi-05-UserForm
date: 2017-10-07 19:58:42
categories: SolidWorks API
tags:
- SolidWorks
- API

---

# 使用UserForm

添加UserForm以增加程序灵活性。本示例中通过UserForm来指定Cylinder的直径和长度。

## UserForm部分

UserForm主要包括

* 两个文字框（用来接收指定的直径和长度）
* 两个标签来提示输入内容
* 一个Command Button来调用主函数

<!--more-->

负责显示的`UserForm.show`直接在main函数中调用。

其中Command Button部分代码如下：

```vb
Private Sub CommandButton1_Click()
    'create variables for Diameter and Length
    Dim Length As Double
    Dim Diameter As Double
    
    Length = UserForm1.LengthTextBox.Text
    Diameter = UserForm1.DiameterTextBox.Text
    'call sub routine
    Call InsertSketch
    Call CreatCylinder(Length, Diameter)
    
    End

End Sub
```

## 主函数部分

主要用来执行新建草图--绘制草图--拉伸等具体操作。

代码如下：

```vb
'Option Explicit
' ******************************************************************************
' 09/27/17 by ds18b20
' ******************************************************************************
Dim swApp As SldWorks.SldWorks
Dim Part As SldWorks.ModelDoc2

Dim boolstatus As Boolean
Dim longstatus As Long, longwarnings As Long

Sub main()
    'add line to show user form
    UserForm1.Show
End Sub

Public Sub InsertSketch()
    'open new doc and insert sketch
    Set swApp = Application.SldWorks

    Set Part = swApp.NewDocument("C:\ProgramData\SolidWorks\SOLIDWORKS 2016\templates\gb_part.prtdot", 0, 0, 0)
    swApp.ActivateDoc2 "零件2", False, longstatus
    Set Part = swApp.ActiveDoc
    Dim myModelView As Object
    Set myModelView = Part.ActiveView
    myModelView.FrameState = swWindowState_e.swWindowMaximized
    boolstatus = Part.Extension.SelectByID2("前视基准面", "PLANE", 0, 0, 0, False, 0, Nothing, 0)
    Part.SketchManager.InsertSketch True

End Sub
    
Public Sub CreatCylinder(InputLength As Double, InputDiameter As Double)

    '=====================================
    'Define Variables
    Dim CylinderLength As Double
    Dim CylinderDia As Double
    
    '=====================================
    'Inputs from user form
    CylinderLength = InputLength / 1000
    CylinderDia = InputDiameter / 1000
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

