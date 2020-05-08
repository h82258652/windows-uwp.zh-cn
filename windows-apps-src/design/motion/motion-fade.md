---
Description: 使用淡化动画将项目引入或引出视图。 两个常见淡化动画是淡入和淡出。
title: 淡化动画
ms.assetid: 975E5EE3-EFBE-4159-8D10-3C94143DD07F
label: Motion--fades
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6b937ba4174d01096a09a98f7efd4e7e3dce9632
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970302"
---
# <a name="fade-animations"></a>淡化动画



使用淡化动画将项目引入或引出视图。 两个常见淡化动画是淡入和淡出。

> **重要 API**：[**FadeInThemeAnimation 类**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FadeInThemeAnimation)、[**FadeOutThemeAnimation 类**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FadeOutThemeAnimation)


## <a name="dos-and-donts"></a>应做事项和禁止事项


-   当你的应用在不相关的元素或有大量文本的元素之间过渡时，先使用淡出，然后使用淡入。 这可让传出对象在传入对象显示之前完全消失。
-   如果元素的大小保持不变并且你希望用户感觉他们在查看相同的项，则在传出元素之上淡入一个或多个传入元素。 淡入完成后，可以删除传出项目。 这只是在传出项被传入项完全覆盖时的一个可行选项。
-   避免使用淡化动画添加或删除列表中的项。 相反，请使用为该目的创建的列表动画。
-   避免使用淡化动画更改整个页面内容。 相反，请使用为该目的创建的页面过渡动画。
-   淡出是用于删除元素的不明显的方法。
## <a name="related-articles"></a>相关文章

* [动画概述](https://docs.microsoft.com/windows/uwp/graphics/animations-overview)
* [创建淡化动画](https://docs.microsoft.com/previous-versions/windows/apps/jj649429(v=win.10))
* [快速入门：使用库动画创建 UI 动画](https://docs.microsoft.com/previous-versions/windows/apps/hh452703(v=win.10))
* [**FadeInThemeAnimation 类**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FadeInThemeAnimation)
* [**FadeOutThemeAnimation 类**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FadeOutThemeAnimation)

 

 




