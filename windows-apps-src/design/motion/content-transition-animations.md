---
author: mijacobs
Description: Content transition animations let you change the content of an area of the screen while keeping the container or background constant. New content fades in. If there is existing content to be replaced, that content fades out.
title: 内容过渡动画指南
ms.assetid: 0188FDB4-E183-466f-8A03-EE3FF5C474B1
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10，uwp
pm-contact: stmoy
design-contact: conrwi
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 14d5120632833e91e82ed7dd717ba04a9abb0efb
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2018
ms.locfileid: "6645841"
---
# <a name="content-transition-animations"></a>内容转换动画



内容过渡动画可让你更改屏幕区域的内容，同时保持容器或背景不变。 新的内容将淡入。 如果存在要替换的现有内容，则该内容将淡出。

> **重要 API**：[**ContentThemeTransition 类 (XAML)**](https://msdn.microsoft.com/library/windows/apps/br243104)

## <a name="dos-and-donts"></a>应做事项和禁止事项


-   如果要将一组新项目移入某个空容器，请使用进入动画。 例如，开始载入应用后，部分应用内容可能无法立即显示出来。 在该内容准备好显示时，使用内容过渡动画将后续内容移入视图中。
-   使用内容过渡可将一组内容替换为已经驻留在视图内同一容器中的另一组内容。
-   在引入新内容时，根据常规页面流或阅读顺序将该内容向上滑入（从底部到顶部）视图。
-   以合乎逻辑的方式引入新内容，例如，最后引入最重要的内容片段。
-   如果有多个容器的内容需要进行更新，将同时触发所有过渡动画，不出现任何耽搁或延迟。
-   当整个页面都发生变化时不要使用内容过渡动画。 在此情况下应改用页面过渡动画。
-   如果仅刷新内容，不要使用内容过渡动画。 内容过渡动画是为了显示运动。 要进行刷新，请使用淡化动画。



## <a name="related-articles"></a>相关文章

**面向开发人员 (XAML)**
* [动画概述](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [创建内容过渡动画](https://msdn.microsoft.com/library/windows/apps/xaml/jj649426)
* [快速入门：使用库动画创建 UI 动画](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**ContentThemeTransition 类**](https://msdn.microsoft.com/library/windows/apps/br243104)

 

 




