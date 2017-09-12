---
author: mijacobs
Description: "使用弹出动画显示和隐藏浮出控件或自定义弹出 UI 元素的弹出 UI。 弹出元素是显示在应用内容上方的容器，并且将在用户点击或单击弹出元素外部时取消。"
title: "UWP 应用中的弹出 UI 动画"
ms.assetid: 4E9025CE-FC90-4d4c-9DE6-EC6B6F2AD9DF
label: Motion--Pop-up animations
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10，uwp"
ms.openlocfilehash: c91e5cd3d4bad1b29d070f4750beb3dd95b3c5dc
ms.sourcegitcommit: 10d6736a0827fe813c3c6e8d26d67b20ff110f6c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/22/2017
---
# <a name="pop-up-ui-animations"></a>弹出 UI 动画

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

使用弹出动画显示和隐藏浮出控件或自定义弹出 UI 元素的弹出 UI。 弹出元素是显示在应用内容上方的容器，并且将在用户点击或单击弹出元素外部时取消。

<div class="important-apis" >
<b>重要的 API</b><br/>
<ul>
<li>[**PopInThemeAnimation 类**](https://msdn.microsoft.com/library/windows/apps/br210383)</li>
<li>[**PopupThemeTransition 类**](https://msdn.microsoft.com/library/windows/apps/hh969172)</li>
</ul>
</div>


## <a name="dos-and-donts"></a>应做事项和禁止事项


-   使用弹出动画显示或隐藏不属于应用页本身的自定义弹出 UI 元素。 Windows 提供的常用控件已经内置了这些动画。
-   不要将弹出动画用于工具提示或对话框。
-   不要使用弹出动画显示或隐藏主要应用内容内的 UI：仅使用弹出动画显示或隐藏显示在主要应用内容顶部的弹出容器。

## <a name="related-articles"></a>相关文章

* [动画概述](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [创建弹出 UI 动画](https://msdn.microsoft.com/library/windows/apps/xaml/jj649433)
* [快速入门：使用库动画创建 UI 动画](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**PopInThemeAnimation 类**](https://msdn.microsoft.com/library/windows/apps/br210383)
* [**PopOutThemeAnimation 类**](https://msdn.microsoft.com/library/windows/apps/br210391)
* [**PopupThemeTransition 类**](https://msdn.microsoft.com/library/windows/apps/hh969172)

 

 




