---
author: payzer
title: "如何禁用鼠标模式"
description: 
area: Xbox
ms.sourcegitcommit: bdf7a32d2f0673ab6c176a775b805eff2b7cf437
ms.openlocfilehash: bd7780f105f86d7d292c5db2535ad40af07d6e4f

---

# 如何禁用鼠标模式
默认情况下，所有应用程序的鼠标模式均处于打开状态。 这意味着未选择退出的所有应用程序将收到鼠标指针（类似于该主机的 Edge 浏览器中的情形）。 我们强烈建议你关闭此模式，并针对方向控制器导航进行优化。   
   
## HTML   
若要关闭鼠标模式，请将以下内容添加到你的应用程序的 JavaScript 文件中：   
   
```code
navigator.gamepadInputEmulation = "keyboard";
```   

## XAML    
若要关闭鼠标模式，请将以下内容添加到你的应用程序的 JavaScript 文件中：   
   
```code
public App() {
        this.InitializeComponent();
        this.RequiresPointerMode = Windows.UI.Xaml.ApplicationRequiresPointerMode.WhenRequested;
        this.Suspending += OnSuspending;
        }
```

## C++/DirectX   
如果你要编写 C++/DirectX 应用，则无需执行任何操作。 鼠标模式仅适用于 HTML 和 XAML 应用程序。



<!--HONumber=Jun16_HO5-->


