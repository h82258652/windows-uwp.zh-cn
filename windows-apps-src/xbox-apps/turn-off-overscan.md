---
title: 如何将 UI 绘制到屏幕的边缘
description: 如何关闭标题安全区域自动缩放。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 1adb221f-6f70-4255-9329-2046a486ca45
ms.localizationpriority: medium
ms.openlocfilehash: 1ac49d80f1d99a56eff565a0daa8f2f3e9289636
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "8328618"
---
# <a name="how-to-draw-ui-to-the-edge-of-the-screen"></a>如何将 UI 绘制到屏幕的边缘   
考虑到电视安全区域，应用程序默认会将边框置于视口的边缘（有关详细信息，请参阅[针对 Xbox 和电视进行设计](../design/devices/designing-for-tv.md#tv-safe-area)）。 

我们建议关闭此功能并绘制到屏幕的边缘。 通过添加以下代码，可以在你的应用程序启动时绘制到屏幕的边缘：
   
```
Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetDesiredBoundsMode(Windows.UI.ViewManagement.ApplicationViewBoundsMode.UseCoreWindow);
```
   
> [!NOTE]
> C++/DirectX 应用程序无需担心这一问题。 系统始终会将你的应用程序呈现到屏幕的边缘。

## <a name="see-also"></a>另请参阅
- [适用于 Xbox 的最佳做法](tailoring-for-xbox.md)
- [Xbox One 上的 UWP](index.md)
