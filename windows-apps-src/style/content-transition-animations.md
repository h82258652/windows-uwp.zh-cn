---
author: mijacobs
Description: "内容过渡动画可让你更改屏幕区域的内容，同时保持容器或背景不变。 新的内容将淡入。 如果存在要替换的现有内容，则该内容将淡出。"
title: "内容过渡动画指南"
ms.assetid: 0188FDB4-E183-466f-8A03-EE3FF5C474B1
template: detail.hbs
ms.openlocfilehash: ed2d580568b73e787ad7b980981e025652232a83
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="content-transition-animations"></a>内容过渡动画

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

内容过渡动画可让你更改屏幕区域的内容，同时保持容器或背景不变。 新的内容将淡入。 如果存在要替换的现有内容，则该内容将淡出。

<div class="important-apis" >
<b>重要的 API</b><br/>
<ul>
<li>[**ContentThemeTransition 类 (XAML)**](https://msdn.microsoft.com/library/windows/apps/br243104)</li>
</ul>
</div>

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

 

 




