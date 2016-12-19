---
author: mijacobs
description: "颜色提供查找应用的各种级别的信息的直观方法，并用作强化交互模型的重要工具。"
title: "颜色"
ms.assetid: 3ba7176f-ac47-498c-80ed-4448edade8ad
template: detail.hbs
extraBodyClass: style-color
translationtype: Human Translation
ms.sourcegitcommit: d7236006f2c620a4ff0de4e0f413f32a2eaf5687
ms.openlocfilehash: 8e253c93f932e04b825478cf0801e4c8c0d43b9d

---

# 颜色

颜色提供查找应用的各种级别的信息的直观方法，并用作强化交互模型的重要工具。

在 Windows 中，颜色也可进行个性化。 用户可以选择要在其整个体验中彰显的颜色和浅色或深色主题。

## 主题色

用户可以从“设置”&gt;“个性化”&gt;“颜色”**中选取一种称为主题色的颜色。 他们可以从一组 48 种颜色样本中进行选择，但 Xbox 除外，因为它的调色板只有 21 种电视安全颜色。

<!-- Alternate version for the dev center. Need to add hex values. -->
![默认主题色](images/accentcolorswatch.png) 默认主题色

![Xbox 主题色](images/accentcolorswatch_xbox.png) Xbox 主题色


当用户选择了某种主题色时，它会显示为系统主题的一部分。 受影响的区域有“开始”菜单、任务栏、窗口镶边、选择的交互状态以及[常用控件](https://dev.windows.com/design/controls-patterns)中的超链接。 每个应用可以通过版式、背景和交互进一步合并主题色，或覆盖它以保留它们的特定品牌。

## 颜色调色板构建基块

选定主题色后，会根据颜色亮度的 HSB 值创建浅色和深色的主题色。 应用可使用阴影变量创建可视化层次结构和提供交互指示。

默认情况下，超链接将使用用户的主题色。 如果页面背景颜色相似，你可以选择将较浅（或较深）色调的主题色分配给超链接，以获取更好的对比度。

![内含 6 种色调的单个主题色](images/shades.png) 默认主题色的各种浅色调/深色调。

![色彩操作中心的规范](images/action_center_redline_zoom.png) 颜色逻辑如何应用于设计规范的示例。

**注意**  在 XAML 中，主要主题色显示为名为 `SystemAccentColor` 的[主题资源](https://msdn.microsoft.com/library/windows/apps/Mt187274.aspx)。 这些色调可用作 `SystemAccentColorLight3`、`SystemAccentColorLight2`、`SystemAccentColorLight1`、`SystemAccentColorDark1`、`SystemAccentColorDark2` 和 `SystemAccentColorDark3`。 也可以通过 [UISettings.GetColorValue](https://msdn.microsoft.com/library/windows/apps/windows.ui.viewmanagement.uisettings.getcolorvalue.aspx) 和 [UIColorType](https://msdn.microsoft.com/library/windows/apps/windows.ui.viewmanagement.uicolortype.aspx) 枚举，以编程方式获得。

## 颜色主题

用户也可以为系统选择浅色或深色主题。 某些应用选择根据用户喜好更改主题，而其他应用则不会。

使用浅色主题的应用适用于涉及生产性应用的方案。 Microsoft Office 提供的应用套件即是例子。 浅色主题支持在延长的任务时间内轻松阅读长篇大论。

深色主题更能够看到媒体中心式的应用的内容对比或提供给用户丰富的视频或图像的方案对比。 在这些方案中，虽然看电影的体验可能是主要任务，但阅读不一定是，并且它在光线较暗的环境条件下显示。

如果你的应用不完全符合这些描述中的任意描述，请考虑遵循系统主题，让用户决定适合他们的主题。

为使主题设计变得更简单，Windows 提供了自动适应主题的颜色调色板。

<!-- OP version -->
### 浅色主题
#### 基本
![基本浅色主题](images/themes-light-base.png)
#### 备用
![备用浅色主题](images/themes-light-alt.png)
#### 列表
![列表浅色主题](images/themes-light-list.png)
#### 镶边
![镶边浅色主题](images/themes-light-chrome.png)
### 深色主题
#### 基本
![基本深色主题](images/themes-dark-base.png)
#### 备用
![备用深色主题](images/themes-dark-alt.png)
#### 列表
![列表深色主题](images/themes-dark-list.png)
#### 镶边
![镶边深色主题](images/themes-dark-chrome.png)


## 更改主题

可通过更改 App.xaml 中的 **RequestedTheme** 属性轻松更改主题：

```XAML
<Application
    x:Class="App9.App"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:App9"
    RequestedTheme="Dark">

</Application>
```

删除 **RequestedTheme** 意味着应用程序将沿用用户的应用模式设置，可以将这些设置选择为以深色或浅色主题查看你的应用。 

确保在创建应用时考虑主题，因为主题对应用外观的影响较大。

## 辅助功能

我们的调色板已经过优化，可在屏幕上使用。 我们建议文本与背景保持 4.5:1 的对比率，以达到最佳可读性。 有许多免费工具可用于测试你的颜色是否可行，如[对比率](http://leaverou.github.io/contrast-ratio/)。

## 相关文章

* [XAML 样式](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/xaml-theme-resources)
* [XAML 主题资源](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/xaml-theme-resources)



<!--HONumber=Aug16_HO3-->


