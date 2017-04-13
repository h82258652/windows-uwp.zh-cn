---
author: Xansky
Description: "了解如何开发辅助 Windows 10 UWP 应用，其中包含键盘导航、颜色和对比度设置以及对辅助技术的支持。"
ms.assetid: 9311D23A-B340-42F0-BEFE-9261442AF108
title: "开发非独占 Windows 10 应用"
label: Developing inclusive Windows 10 apps
template: detail.hbs
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 5e4668a185e003150cf9d72aeb5c55c2e092d549
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="developing-inclusive-windows-apps"></a>开发非独占 Windows 应用  

本文讨论了如何开发辅助通用 Windows 平台 (UWP) 应用。 具体而言，它假定你了解如何为应用设计逻辑层次结构。 了解如何开发辅助 Windows 10 UWP 应用，该应用包含键盘导航、颜色和对比度设置以及对辅助技术的支持。

如果你尚未了解，请首先阅读[设计非独占软件](designing-inclusive-software.md)。

为了确保你的应用是辅助应用，你应执行以下三项操作：

1. 向[编程访问](#programmatic-access)公开你的 UI 元素。
2. 确保你的应用针对无法使用鼠标或触摸屏的用户支持[键盘导航](#keyboard-navigation)。
3. 确保你的应用支持辅助[颜色和对比度](#color-and-contrast)设置。

## <a name="programmatic-access"></a>编程访问  
在应用中创建辅助功能时，编程访问至关重要。 通过为应用中的内容和交互式 UI 元素设置辅助名称（必选）和描述（可选）来实现此操作。 这可确保将 UI 控件公开给屏幕阅读器（例如讲述人）或替代输出设备（如盲文屏幕）等辅助技术 (AT)。 如果没有编程访问，辅助技术的 API 将无法正确解释信息，从而导致用户无法充分使用产品，或者迫使辅助技术使用从未旨在用作辅助功能接口的未记录编程接口或技术。 当将 UI 控件公开给辅助技术时，辅助技术可以确定用户可以使用哪些操作和选项。  

有关使应用 UI 元素可用于辅助技术 (AT) 的详细信息，请参阅[公开基本的辅助功能信息](basic-accessibility-information.md)。

## <a name="keyboard-navigation"></a>键盘导航  
对于失明或行动不便的用户，能够使用键盘导航 UI 极其重要。 但是，应仅向需要用户交互才能运作的 UI 控件提供键盘焦点。 不需要操作的组件（如静态图像）不需要键盘焦点。  

请务必记住，和使用鼠标或触摸导航不同，键盘导航是线性的。 在考虑键盘导航时，请考虑你的用户将如何与你的产品交互以及逻辑导航是什么。 在西方文化中，人们以从左到右、从上到下的顺序阅读。 因此，常见做法是针对键盘导航遵循此模式。  

在设计键盘导航时，检查你的 UI，并思考以下问题：
* 控件在 UI 中如何布局或分组？
* 是否存在一些重要的控件组？
    * 如果是，这些组是否包含其他级别的组？
*     在对等控件之间，应通过按 Tab 键、通过特殊导航（如箭头键）还是同时使用两者来进行导航？

目标是帮助用户了解 UI 的布局方式并识别可操作的控件。 如果你发现在用户完成导航循环前有太多制表位，请考虑将相关的控件分组在一起。 某些相关的控件（如混合控件）可能需要在此早期探索阶段进行处理。 在开始开发产品后，很难重新制定键盘导航，因此请尽早进行周密计划！  

若要了解有关 UI 元素之间的键盘导航的详细信息，请参阅[键盘辅助功能](keyboard-accessibility.md)。  

此外，[针对辅助功能设计软件](https://www.microsoft.com/download/details.aspx?id=19262)电子书包含有关此主题的出色章节，标题为_设计逻辑层次结构_。

## <a name="color-and-contrast"></a>颜色和对比度  
Windows 中的内置辅助功能之一是高对比度模式，该模式可增强计算机屏幕上的文本和图像的颜色对比度。 对于某些人来说，增加颜色对比度可以降低眼睛疲劳并且更易于阅读。 当你在高对比度下验证 UI 时，你要检查是否已使用系统颜色（而不是硬编码颜色）为控件一致编码，以确保他们能够在屏幕上看到不使用高对比度的用户所看到的所有控件。  

XAML
```xaml
<Button Background="{ThemeResource ButtonBackgroundThemeBrush}">OK</Button>
```
有关使用系统颜色和资源的详细信息，请参阅 [XAML 主题资源](../controls-and-patterns/xaml-theme-resources.md)。

只要你未替代系统颜色，UWP 应用在默认情况下就支持高对比度主题。 如果用户已经选择他们希望系统使用系统设置或辅助功能工具中的高对比度主题，则框架将自动使用为 UI 中的控件和组件生成高对比度布局和呈现的颜色和样式设置。   

有关详细信息，请参阅[高对比度主题](high-contrast-themes.md)。  

如果你已决定使用你自己的颜色主题而不是系统主题，请考虑以下指南：  

**颜色对比度比率** - 更新后的美国残疾人法案第 508 节以及其他法规要求文本与其背景之间的默认颜色对比度必须为 5:1。 对于较大的文本（18 点字体大小或 14 点加粗），所需的默认对比度为 3:1。  

**颜色组合** - 大约 7% 的男性（和小于 1 % 的女性）具有某种形式的色觉障碍。 色盲用户难以区分某些颜色，因此在应用程序中永远不单独使用颜色来传达状态或意义，这一点很重要。 对于装饰性图像（如图标或背景），应尽量选择能够使色盲用户识别图像的颜色组合。  

## <a name="accessibility-checklist"></a>辅助功能清单  
下面是简化版的辅助功能清单：

1. 为应用中的内容和交互式 UI 元素设置辅助名称（必选）和描述（可选）。
2. 实现键盘辅助功能。
3. 以直观方式验证你的 UI 以确保文本对比度足够大、元素以高对比度主题正确呈现以及使用了正确的颜色。
4. 运行辅助功能工具、解决报告的问题并验证屏幕阅读体验。 （请参阅辅助功能测试主题。）
5. 确保你的应用清单设置遵循辅助功能指南。
6. 在 Windows 应用商店中将你的应用声明为辅助应用。 （请参阅[应用商店中的辅助功能](accessibility-in-the-store.md)主题。）

有关更多详细信息，请参阅完整的[辅助功能清单](accessibility-checklist.md)主题。

## <a name="related-topics"></a>相关主题  
* [设计非独占软件](designing-inclusive-software.md)  
* [非独占设计](http://design.microsoft.com/inclusive)
* [要避免的辅助功能做法](practices-to-avoid.md)
* [针对辅助功能设计软件](https://www.microsoft.com/download/details.aspx?id=19262)
* [Microsoft 辅助功能开发人员中心](https://msdn.microsoft.com/enable)
* [辅助功能](accessibility.md)
