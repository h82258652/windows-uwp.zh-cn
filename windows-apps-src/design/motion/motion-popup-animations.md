---
Description: 使用弹出动画显示和隐藏浮出控件或自定义弹出 UI 元素的弹出 UI。 弹出元素是显示在应用内容上方的容器，并且将在用户点击或单击弹出元素外部时取消。
title: 弹出 UI 动画
ms.assetid: 4E9025CE-FC90-4d4c-9DE6-EC6B6F2AD9DF
label: Motion--Pop-up animations
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 78adc32dccc66b98e413796d9cfe20b2a158261e
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970532"
---
# <a name="pop-up-ui-animations"></a>弹出 UI 动画



使用弹出动画显示和隐藏浮出控件或自定义弹出 UI 元素的弹出 UI。 弹出元素是显示在应用内容上方的容器，并且将在用户点击或单击弹出元素外部时取消。

> **重要 API**：[**PopInThemeAnimation 类**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PopInThemeAnimation)、[**PopupThemeTransition 类**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PopupThemeTransition)


## <a name="dos-and-donts"></a>应做事项和禁止事项


-   使用弹出动画显示或隐藏不属于应用页本身的自定义弹出 UI 元素。 Windows 提供的常用控件已经内置了这些动画。
-   不要将弹出动画用于工具提示或对话框。
-   不要使用弹出动画显示或隐藏主要应用内容内的 UI：仅使用弹出动画显示或隐藏显示在主要应用内容顶部的弹出容器。

## <a name="related-articles"></a>相关文章

* [动画概述](https://docs.microsoft.com/windows/uwp/graphics/animations-overview)
* [创建弹出 UI 动画](https://docs.microsoft.com/previous-versions/windows/apps/jj649433(v=win.10))
* [快速入门：使用库动画创建 UI 动画](https://docs.microsoft.com/previous-versions/windows/apps/hh452703(v=win.10))
* [**PopInThemeAnimation 类**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PopInThemeAnimation)
* [**PopOutThemeAnimation 类**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PopOutThemeAnimation)
* [**PopupThemeTransition 类**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PopupThemeTransition)

 

 




