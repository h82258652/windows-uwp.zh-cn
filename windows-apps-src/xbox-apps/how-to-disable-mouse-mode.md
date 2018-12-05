---
title: 如何禁用鼠标模式
description: 禁用默认鼠标模式说明。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: e57ee4e6-7807-4943-a933-c2b4dc80fc01
ms.localizationpriority: medium
ms.openlocfilehash: 1e4b8868f416494daf978d65d4a4ccde02d6ccf5
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2018
ms.locfileid: "8740608"
---
# <a name="how-to-disable-mouse-mode"></a>如何禁用鼠标模式
默认情况下，所有应用程序的鼠标模式都处于打开状态，这意味着未选择退出的所有应用程序将收到鼠标指针（类似于该主机的 Edge 浏览器中的情形）。 我们强烈建议你关闭此模式，并针对方向控制器导航进行优化。   
   
## <a name="html"></a>HTML   
若要在 JavaScript 通用 Windows 平台 (UWP) 应用中打开方向控制器导航，请使用 [TVHelpers 方向导航](https://github.com/Microsoft/TVHelpers/wiki/Using-DirectionalNavigation) JavaScript 库。 将方向导航 JavaScript 文件包含在你的应用包中，并在需要方向控制器导航的所有 HTML 页面中添加对它的引用。

```code
<script src="directionalnavigation-1.0.0.0.js"></script>
```
有关更多详细信息，请参阅[方向导航 Wiki](https://github.com/Microsoft/TVHelpers/wiki/Using-DirectionalNavigation)。

如果你要关闭鼠标模式并直接使用 DOM 或 WinRT 游戏板 API，请为需要它的每个页面运行以下内容： 
   
```code
navigator.gamepadInputEmulation = "gamepad";
```   

   此属性默认为 `mouse`，可启用鼠标模式。 将它设置为 `keyboard` 可关闭鼠标模式，但游戏板输入将生成 DOM 键盘事件。 将它设置为 `gamepad` 可关闭鼠标模式，也不会生成 DOM 键盘事件，只允许你使用 DOM 或 WinRT 游戏板 API。

## <a name="xaml"></a>XAML    
若要关闭鼠标模式，请将以下内容添加到应用的构造函数：   
   
```code
public App() {
        this.InitializeComponent();
        this.RequiresPointerMode = Windows.UI.Xaml.ApplicationRequiresPointerMode.WhenRequested;
        this.Suspending += OnSuspending;
}
```

## <a name="cdirectx"></a>C++/DirectX   
如果你要编写 C++/DirectX 应用，则无需执行任何操作。 鼠标模式仅适用于 HTML 和 XAML 应用程序。

## <a name="see-also"></a>另请参阅
- [适用于 Xbox 的最佳做法](tailoring-for-xbox.md)
- [Xbox One 上的 UWP](index.md)

