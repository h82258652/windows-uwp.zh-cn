---
author: mijacobs
Description: "基于边缘的动画显示或隐藏源自屏幕边缘的 UI。"
title: "UWP 应用中基于边缘的 UI 动画"
ms.assetid: 5A8F73B1-F4F6-424b-9EDF-A9766C5DEAE8
label: Motion--edge-based UI
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 1be51a8ff4a63f32834c7eb04b70d17dc41de13b

---

# 基于边缘的 UI 动画




基于边缘的动画显示或隐藏源自屏幕边缘的 UI。 显示和隐藏动画可以由用户或应用启动。 此 UI 可以覆盖应用，也可以作为主屏表面的一部分。 如果此 UI 是应用图面的一部分，那么应用的其余部分可能需要相应调整大小。

**重要的 API**

-   [**EdgeUIThemeTransition 类**](https://msdn.microsoft.com/library/windows/apps/hh702324)


## 应做事项和禁止事项


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

## 相关文章


**对于开发人员 (XAML)**
* [动画概述](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [创建基于边缘的 UI 动画](https://msdn.microsoft.com/library/windows/apps/xaml/jj649428)
* [快速入门：使用库动画创建 UI 动画](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**EdgeUIThemeTransition 类**](https://msdn.microsoft.com/library/windows/apps/hh702324)
* [**PaneThemeTransition 类**](https://msdn.microsoft.com/library/windows/apps/hh969160)
* [创建淡化动画](https://msdn.microsoft.com/library/windows/apps/xaml/jj649429)
* [创建重新定位动画](https://msdn.microsoft.com/library/windows/apps/xaml/jj649434)

 

 







<!--HONumber=Jun16_HO4-->


