---
author: payzer
title: "如何将 UI 绘制到屏幕的边缘"
description: 
translationtype: Human Translation
ms.sourcegitcommit: b5961d3266a031ab09a9da63319e9883cf050789
ms.openlocfilehash: cddde27a17e897ab8a68bbed099e532a8cd48f07

---

# 如何将 UI 绘制到屏幕的边缘   
考虑到电视安全区域，应用程序默认会将边框置于视口的边缘（有关详细信息，请参阅[针对 Xbox 和电视进行设计](../input-and-devices/designing-for-tv.md#tv-safe-area)）。 

我们建议关闭此功能并绘制到屏幕的边缘。 通过添加以下代码，可以在你的应用程序启动时绘制到屏幕的边缘：
   
```
Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetDesiredBoundsMode(Windows.UI.ViewManagement.ApplicationViewBoundsMode.UseCoreWindow);
```
   
> [!NOTE]
> C++/DirectX 应用程序无需担心这一问题。 系统始终会将你的应用程序呈现到屏幕的边缘。

## 另请参阅
- [适用于 Xbox 的最佳做法](tailoring-for-xbox.md)
- [Xbox One 上的 UWP](index.md)



<!--HONumber=Aug16_HO3-->


