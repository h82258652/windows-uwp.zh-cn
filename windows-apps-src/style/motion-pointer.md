---
author: mijacobs
Description: "使用指针动画向用户提供用户点击某个项目时的视觉反馈。"
title: "UWP 应用中的指针单击动画"
ms.assetid: EEB10A2C-629A-4705-8468-4D019D74DDFF
label: Motion--Pointer animations
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a3924fef520d7ba70873d6838f8e194e5fc96c62
ms.openlocfilehash: c208b67829e2053302ec0cc7c6da013b89cee8b3

---

# <a name="pointer-click-animations"></a>指针单击动画

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

使用指针动画向用户提供用户点击某个项目时的视觉反馈。 当第一次点击某个项目时，指针向下动画会略微缩小和倾斜所按的项目，并且会进行播放。 当用户释放指针时，会播放指针向上动画，这会将该项目还原到其原始位置。


<div class="important-apis" >
<b>重要的 API</b><br/>
<ul>
<li>[**PointerUpThemeAnimation 类**](https://msdn.microsoft.com/library/windows/apps/hh969168)</li>
<li>[**PointerDownThemeAnimation 类**](https://msdn.microsoft.com/library/windows/apps/hh969164)</li>
</ul>
</div>


## <a name="dos-and-donts"></a>应做事项和禁止事项

-   在使用指针向上动画时，如果用户释放指针，将立即触发动画。 这样，即使点击触发的操作（如导航到新页面）的响应速度较慢，也可以为用户提供其操作已被识别的即时反馈。

## <a name="related-articles"></a>相关文章

* [动画概述](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [创建指针单击动画](https://msdn.microsoft.com/library/windows/apps/xaml/jj649432)
* [快速入门：使用库动画创建 UI 动画](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**PointerUpThemeAnimation 类**](https://msdn.microsoft.com/library/windows/apps/hh969168)
* [**PointerDownThemeAnimation 类**](https://msdn.microsoft.com/library/windows/apps/hh969164)

 

 







<!--HONumber=Dec16_HO2-->


