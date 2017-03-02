---
author: payzer
title: "如何将 UI 绘制到屏幕的边缘"
description: 
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 1adb221f-6f70-4255-9329-2046a486ca45
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 9a221672391dfbfb4af664438448307800020c6f
ms.lasthandoff: 02/08/2017

---

# <a name="how-to-draw-ui-to-the-edge-of-the-screen"></a>如何将 UI 绘制到屏幕的边缘   
考虑到电视安全区域，应用程序默认会将边框置于视口的边缘（有关详细信息，请参阅[针对 Xbox 和电视进行设计](../input-and-devices/designing-for-tv.md#tv-safe-area)）。 

我们建议关闭此功能并绘制到屏幕的边缘。 通过添加以下代码，可以在你的应用程序启动时绘制到屏幕的边缘：
   
```
Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetDesiredBoundsMode(Windows.UI.ViewManagement.ApplicationViewBoundsMode.UseCoreWindow);
```
   
> [!NOTE]
> C++/DirectX 应用程序无需担心这一问题。 系统始终会将你的应用程序呈现到屏幕的边缘。

## <a name="see-also"></a>另请参阅
- [适用于 Xbox 的最佳做法](tailoring-for-xbox.md)
- [Xbox One 上的 UWP](index.md)

