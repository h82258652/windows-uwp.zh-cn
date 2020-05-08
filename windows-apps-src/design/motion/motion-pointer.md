---
Description: 使用指针动画向用户提供用户点击某个项目时的视觉反馈。
title: 指针单击动画
ms.assetid: EEB10A2C-629A-4705-8468-4D019D74DDFF
ms.date: 08/09/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 32c3460b1af226adfa969d02a695f1d6436390be
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970542"
---
# <a name="pointer-click-animations"></a>指针单击动画



使用指针动画向用户提供用户点击某个项目时的视觉反馈。 当第一次点击某个项目时，指针向下动画会略微缩小和倾斜所按的项目，并且会进行播放。 当用户释放指针时，会播放指针向上动画，这会将该项目还原到其原始位置。


> **重要 API**：[**PointerUpThemeAnimation 类**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PointerUpThemeAnimation)、[**PointerDownThemeAnimation 类**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PointerDownThemeAnimation)


## <a name="dos-and-donts"></a>应做事项和禁止事项

-   在使用指针向上动画时，如果用户释放指针，将立即触发动画。 这样，即使点击触发的操作（如导航到新页面）的响应速度较慢，也可以为用户提供其操作已被识别的即时反馈。

## <a name="related-articles"></a>相关文章

* [动画概述](https://docs.microsoft.com/windows/uwp/graphics/animations-overview)
* [创建指针单击动画](https://docs.microsoft.com/previous-versions/windows/apps/jj649432(v=win.10))
* [快速入门：使用库动画创建 UI 动画](https://docs.microsoft.com/previous-versions/windows/apps/hh452703(v=win.10))
* [**PointerUpThemeAnimation 类**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PointerUpThemeAnimation)
* [**PointerDownThemeAnimation 类**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PointerDownThemeAnimation)

 

 




