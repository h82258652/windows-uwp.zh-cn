---
author: payzer
title: "如何禁用鼠标模式"
description: 
area: Xbox
translationtype: Human Translation
ms.sourcegitcommit: 6f4719c98d490cdcac8c799c4c68af55b217cbc5
ms.openlocfilehash: d1ee946693b9f9714b8d570b8ae3718469d2c10d

---

# 如何禁用鼠标模式
默认情况下，所有应用程序的鼠标模式均处于打开状态。 这意味着未选择退出的所有应用程序将收到鼠标指针（类似于该主机的 Edge 浏览器中的情形）。 我们强烈建议你关闭此模式，并针对方向控制器导航进行优化。   
   
## HTML   
若要在 JavaScript UWP 应用中打开方向控制器导航，请使用 [TVHelpers 方向导航](https://github.com/Microsoft/TVHelpers/wiki/Using-DirectionalNavigation) JavaScript 库。 将方向导航 JavaScript 文件包含在你的应用包中，并在需要方向控制器导航的所有 HTML 页面中添加对它的引用。
```code
<script src="directionalnavigation-1.0.0.0.js"></script>
```
有关更多详细信息，请参阅[方向导航 Wiki](https://github.com/Microsoft/TVHelpers/wiki/Using-DirectionalNavigation)。

如果你要关闭鼠标模式并直接使用 DOM 或 WinRT 游戏板 API，请为需要它的每个页面运行以下内容： 
   
```code
navigator.gamepadInputEmulation = "gamepad";
```   

此属性默认为 ```'mouse'```，可启用鼠标模式。 将它设置为 ```'keyboard'``` 可关闭鼠标模式，但游戏板输入将生成 DOM 键盘事件。 将它设置为 ```'gamepad'``` 可关闭鼠标模式，也不会生成 DOM 键盘事件，只允许你使用 DOM 或 WinRT 游戏板 API。

## XAML    
若要关闭鼠标模式，请将以下内容添加到应用的构造函数：   
   
```code
public App() {
        this.InitializeComponent();
        this.RequiresPointerMode = Windows.UI.Xaml.ApplicationRequiresPointerMode.WhenRequested;
        this.Suspending += OnSuspending;
}
```

## C++/DirectX   
如果你要编写 C++/DirectX 应用，则无需执行任何操作。 鼠标模式仅适用于 HTML 和 XAML 应用程序。



<!--HONumber=Jul16_HO1-->


