---
author: mijacobs
description: "“展示”是一种新的交互模型，有助于为应用程序增添更多焦点和乐趣。"
title: "展示"
template: detail.hbs
ms.author: mijacobs
ms.date: 08/9/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: kisai
design-contact: conrwi
dev-contact: jevansa
doc-status: Published
ms.openlocfilehash: d50e3f47faad5fff0ef461a4b5312127a0b9ec9c
ms.sourcegitcommit: 0d5b3daddb3ae74f91178c58e35cbab33854cb7f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/09/2017
---
# <a name="reveal"></a>展示

> [!IMPORTANT]
> 本文介绍的功能尚未发布，在商业发行之前可能发生重大修改。 Microsoft 不对此处提供的信息作任何明示或默示的担保。

“展示”是一种灯光效果，有助于重点深入了解应用的交互式元素。

> **重要的 API**：[RevealBrush 类](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrush)、[RevealBackgroundBrush 类](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbackgroundbrush)、[RevealBorderBrush 类](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealborderbrush)、[RevealBrushHelper 类](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrushhelper) 和 [VisualState 类](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.VisualState)

“展示”行为会在鼠标或焦点矩形位于所需区域上方时显示展示（或焦点）内容周围的元素，从而完成此操作。

![展示视觉](images/Nav_Reveal_Animation.gif)

通过显示对象周围的隐藏边框，“展示”可以帮助用户更好地理解他们与之交互的空间以及可用操作。 这一点在列表控件和附带背板的控件方面尤其重要。

## <a name="reveal-and-the-fluent-design-system"></a>展示和 Fluent 设计系统

 Fluent 设计系统可帮助你创建包含光线、深度、动画、材料和比例的现代、粗体 UI。 展示是 Fluent 设计系统组件，它将光线添加到你的应用。 

## <a name="what-is-reveal"></a>什么是“展示”？

“展示”有两大主要视觉组件：**悬停展示**行为和**边框展示**行为。

![“展示”层](images/RevealLayers.png)

“悬停展示”与悬停的内容（通过指针或焦点输入）直接相关联，并且在悬停或聚焦项目周围应用一个柔和的光晕形状，从而让你知道可以与该项目交互。

“边框展示”应用于聚焦项目和邻近项目。 这会向你显示，这些邻近项目可以与当前的聚焦项目采用类似操作。

“展示”方式细分：

- “边框展示”将位于所有内容顶部，同时位于指定边缘中
- 文本和内容将直接在“边框展示”下方显示
- “悬停展示”将位于内容和文本下方
- 背板（打开并启用“悬停展示”）
- 背景（控件背景）

<!--
<div class=”microsoft-internal-note”>
To create your own Reveal lighting effect for static comps or prototype purposes, see the full [uni design guidance](http://uni/DesignDepot.FrontEnd/#/ProductNav/3020/1/dv/?t=Resources%7CToolkit%7CReveal&f=Neon) for this effect in illustrator.
</div>
-->

## <a name="how-to-use-it"></a>如何使用

“展示”可在需要时在光标周围显示出图形，一旦移动鼠标则立即消失。

在已隐示边框和背板的应用主要内容（展示内容）中启用“展示”时，可以最大化发挥“展示”的作用。 “展示”还应用于控件集或控件列表。

## <a name="controls-that-automatically-use-reveal"></a>自动使用“展示”的控件

- [**ListView**](../controls-and-patterns/lists.md)
- [**TreeView**](../controls-and-patterns/tree-view.md)
- [**NavigationView**](../controls-and-patterns/navigationview.md)
- [**AutosuggestBox**](../controls-and-patterns/auto-suggest-box.md)

## <a name="enabling-reveal-on-other-common-controls"></a>在其他常见控件上启用“展示”

如果你在某个场景中需要应用“展示”（此类控件是主要内容并且/或者用于列表或集合中），我们提供了可选资源样式，能帮助你在这类场景中启用“展示”。

默认情况下此类控件没有启用“展示”，因为这些控件属于小型控件，通常是应用程序主要焦点的辅助控件；但是每个应用均各不相同，如果这些控件是应用中使用最多的控件，我们提供了一些样式以在以下方面提供帮助：

| 控件名称   | 资源名称 |
|----------|:-------------:|
| 按钮 |  ButtonRevealStyle |
| ToggleButton | ToggleButtonRevealStyle |
| RepeatButton | RepeatButtonRevealStyle |
| AppBarButton | AppBarButtonRevealStyle |
| SemanticZoom | SemanticZoomRevealStyle |
| ComboBoxItem | ComboxBoxItemRevealStyle |

若要应用这些样式，只需如下更新“样式”属性：

```XAML
<Button Content="Button Content" Style="{StaticResource ButtonRevealStyle}"/>
```

> [!NOTE]
> SDK 的 16190 版本不会自动提供这些样式。 若要获取它们，你需要手动将它们从 generic.xaml 复制到你的应用。 Generic.xaml 通常位于 C:\Program Files (x86)\Windows Kits\10\DesignTime\CommonConfiguration\Neutral\UAP\10.0.16190.0\Generic。 此问题将在后面的版本中得到修复。 

## <a name="enabling-reveal-on-custom-controls"></a>在自定义控件上启用“展示”

若要在自定义控件或重新模板化控件上启用“展示”，可以转到该控件模板的“视觉状态”中该控件的“样式”，然后在根网格中指定“展示”：

```xaml
<VisualState x:Name="PointerOver">
  <VisualState.Setters>
    <Setter Target="RootGrid.(RevealBrushHelper.State)" Value="PointerOver" />
    <Setter Target="RootGrid.Background" Value="{ThemeResource ButtonRevealBackgroundPointerOver}"/>
    <Setter Target="ContentPresenter.BorderBrush" Value="{ThemeResource ButtonRevealBorderBrushPointerOver}"/>
    <Setter Target="ContentPresenter.Foreground" Value="{ThemeResource ButtonForegroundPointerOver}"/>
  </VisualState.Setters>
</VisualState>
```

若要获取“展示”的拉动效应，则还需要将相同的 RevealBrushHelper 添加到 PressedState：

```xaml
<VisualState x:Name="Pressed">
  <VisualState.Setters>
    <Setter Target="RootGrid.(RevealBrushHelper.State)" Value="Pressed" />
    <Setter Target="RootGrid.Background" Value="{ThemeResource ButtonRevealBackgroundPressed}"/>
    <Setter Target="ContentPresenter.BorderBrush" Value="{ThemeResource ButtonRevealBorderBrushPressed}"/>
    <Setter Target="ContentPresenter.Foreground" Value="{ThemeResource ButtonForegroundPressed}"/>
  </VisualState.Setters>
</VisualState>
```


使用我们的系统资源“展示”，则意味着我们将为你处理所有主题更改问题。

## <a name="dos-and-donts"></a>应做事项和禁止事项
- 请在用户可以采取操作的元素中使用“展示” - “展示”不应用于静态内容
- 请在数据列表或数据集中显示“展示”
- 请将“展示”应用于附带背板的明确包含的内容
- 请勿在无法交互的静态背景、文本或图像上显示“展示”
- 请勿将“展示”应用于无关的悬浮内容
- 请勿在内容对话框或通知等一次性隔离情况中使用“展示”
- 请勿在安全决策中使用“展示”，因为这可能会使你不再关注需要向用户提供的消息

## <a name="related-articles"></a>相关文章

- [RevealBrush 类](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrush)
- [**亚克力**](acrylic.md)
- [**合成效果**](https://msdn.microsoft.com/windows/uwp/graphics/composition-effects)
