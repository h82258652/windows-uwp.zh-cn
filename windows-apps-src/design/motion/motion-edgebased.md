---
Description: 基于边缘的动画显示或隐藏源自屏幕边缘的 UI。
title: UWP 应用中基于边缘的 UI 动画
ms.assetid: 5A8F73B1-F4F6-424b-9EDF-A9766C5DEAE8
label: Motion--edge-based UI
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: fd7071092a66f46a81095a5cb6aff8b623a774a5
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66366625"
---
# <a name="edge-based-ui-animations"></a>基于边缘的 UI 动画





基于边缘的动画显示或隐藏源自屏幕边缘的 UI。 显示和隐藏动画可以由用户或应用启动。 此 UI 可以覆盖应用，也可以作为主屏表面的一部分。 如果此 UI 是应用图面的一部分，那么应用的其余部分可能需要相应调整大小。

> **重要的 API**：[**EdgeUIThemeTransition 类**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.EdgeUIThemeTransition)


## <a name="dos-and-donts"></a>应做事项和禁止事项


-   使用边缘 UI 动画可显示或隐藏没有扩展到屏幕的自定义消息或错误栏。
-   使用面板动画可显示将滑动相当长距离进入屏幕的 UI，如任务窗格或自定义软键盘。
-   将 UI 从其要附着的相同边缘滑入。
-   将 UI 从其出发的相同边缘滑出。
-   如果应用的内容需要调整大小以响应 UI 的滑入或滑出，请使用淡化动画来调整大小。
    -   如果在滑入 UI，则在边缘 UI 或页面动画之后使用淡化动画。
    -   如果在滑出 UI，则在边缘 UI 或页面动画的同时使用淡化动画。
-   不要将这些动画用于通知。 通知不应放在基于边缘的 UI 中。
-   不要将边缘 UI 或面板动画用于不在屏幕边缘的任何 UI 容器或控件。 这些动画仅用于显示屏幕边缘的 UI、调整其大小以及对其进行取消。 若要移动其他类型的 UI，请使用重新定位动画。

    ![显示何时使用边缘 UI 或面板动画，以及何时使用重新定位。](images/edgevsreposition.png)

## <a name="related-articles"></a>相关文章


**面向开发人员**
* [动画概述](https://docs.microsoft.com/windows/uwp/graphics/animations-overview)
* [对基于边缘的 UI 进行动画处理](https://docs.microsoft.com/previous-versions/windows/apps/jj649428(v=win.10))
* [快速入门：对 UI 使用库动画进行动画处理](https://docs.microsoft.com/previous-versions/windows/apps/hh452703(v=win.10))
* [**EdgeUIThemeTransition 类**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.EdgeUIThemeTransition)
* [**PaneThemeTransition 类**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PaneThemeTransition)
* [进行淡化动画处理](https://docs.microsoft.com/previous-versions/windows/apps/jj649429(v=win.10))
* [进行重新定位动画处理](https://docs.microsoft.com/previous-versions/windows/apps/jj649434(v=win.10))

 

 




