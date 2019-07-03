---
Description: 了解如何 Fluent 动作基础知识结合应用程序中。
title: 运动练习 - UWP 应用中的动画
label: Motion in practice
template: detail.hbs
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 8cf010533d2d62559bb8dc0d214e04ab917e62bd
ms.sourcegitcommit: d534f81590d881a18d677a648c59913029837a84
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2019
ms.locfileid: "67535439"
---
# <a name="bringing-it-together"></a>综合运用

计时、缓动、方向性和引力协同工作，构成了 Fluent 运动的基础。 必须对上述要素进行综合考虑，然后在应用上下文中适当应用。

以下是在应用中应用 Fluent 运动基础的 3 种方法。

:::row:::
    :::column:::
**隐式动画**自动 tween 和更换参数来实现非常简单的 Fluent motion 使用标准化的值中的值之间的时间。
    :::column-end:::
    :::column:::
**内置动画**系统组件，如公共控件和共享的动作，将"默认情况下 Fluent"。 已将其隐式使用一致的方式应用基础知识。
    :::column-end:::
    :::column:::
**按照指南建议的自定义动画**可能的有时当系统尚未提供完全的运动解决方案为你的方案。 在这些情况下，为你的体验使用作为起始点的基准基本建议。
    :::column-end:::
:::row-end:::

**转换示例**

![功能性动画](images/pageRefresh.gif)

:::row:::
    :::column:::
<b>方向向前扩展：</b><br>
淡出：150 m;缓动：默认加速<b>中的前进方向：</b><br>
幻灯片向上 150px:300ms;缓动：默认减速
    :::column-end:::
    :::column:::
<b>向扩展后的方向：</b><br>
滑下 150px:150ms;缓动：默认加速<b>中向后方向：</b><br>
淡入：300ms;缓动：默认减速
    :::column-end:::
:::row-end:::

**对象示例**

 ![300 ms 运动](images/control.gif)

:::row:::
    :::column:::
<b>展开方向：</b><br>
增长：300ms;缓动：标准
    :::column-end:::
    :::column:::
<b>方向协定：</b><br>
增长：150ms;缓动：默认加速
    :::column-end:::
:::row-end:::

## <a name="examples"></a>示例

<table>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>如果有<strong style="font-weight: semi-bold">XAML 控件库</strong>应用程序安装，请单击此处<a href="xamlcontrolsgallery:/item/ImplicitTransition">打开应用并在操作中看到隐式转换</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="implicit-animations"></a>隐式动画

> 隐式动画需要 Windows 10，版本 1809年 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 或更高版本。

隐式动画是一种来实现 Fluent 动作自动属性在参数更改新旧值之间插入值的简单方法。

您可以隐式动态显示以下属性的更改：

- [UIElement](/uwp/api/windows.ui.xaml.uielement)
  - **不透明度**
  - **旋转**
  - **缩放**
  - **翻译**

- [边框](/uwp/api/windows.ui.xaml.controls.border)， [ContentPresenter](/uwp/api/windows.ui.xaml.controls.contentpresenter)，或[面板](/uwp/api/windows.ui.xaml.controls.panel)
  - **背景**

可以具有隐式进行动画处理的更改每个属性都有一个相应_过渡_属性。 若要对属性进行动画处理，应将过渡类型分配给相应_过渡_属性。 下表显示了_过渡_属性和要用于每个的过渡类型。

| 动画处理的属性 | 转换属性 | 隐式转换类型 |
| -- | -- | -- |
| [UIElement.Opacity](/uwp/api/windows.ui.xaml.uielement.opacity) | [OpacityTransition](/uwp/api/windows.ui.xaml.uielement.opacitytransition) | [ScalarTransition](/uwp/api/windows.ui.xaml.scalartransition) |
| [UIElement.Rotation](/uwp/api/windows.ui.xaml.uielement.rotation) | [RotationTransition](/uwp/api/windows.ui.xaml.uielement.rotationtransition) | [ScalarTransition](/uwp/api/windows.ui.xaml.scalartransition) |
| [UIElement.Scale](/uwp/api/windows.ui.xaml.uielement.scale) | [ScaleTransition](/uwp/api/windows.ui.xaml.uielement.scaletransition) | [Vector3Transition](/uwp/api/windows.ui.xaml.vector3transition) |
| [UIElement.Translation](/uwp/api/windows.ui.xaml.uielement.translation) | [TranslationTransition](/uwp/api/windows.ui.xaml.uielement.translationtransition) | [Vector3Transition](/uwp/api/windows.ui.xaml.vector3transition) |
| [Border.Background](/uwp/api/windows.ui.xaml.controls.border.background) | [BackgroundTransition](/uwp/api/windows.ui.xaml.controls.border.backgroundtransition) | [BrushTransition](//uwp/api/windows.ui.xaml.uielement.brushtransition) |
| [ContentPresenter.Background](/uwp/api/windows.ui.xaml.controls.contentpresenter.background) | [BackgroundTransition](/uwp/api/windows.ui.xaml.controls.contentpresenter.backgroundtransition) | [BrushTransition](//uwp/api/windows.ui.xaml.uielement.brushtransition) |
| [Panel.Background](/uwp/api/windows.ui.xaml.controls.panel.background) | [BackgroundTransition](/uwp/api/windows.ui.xaml.controls.panel.backgroundtransition)  | [BrushTransition](//uwp/api/windows.ui.xaml.uielement.brushtransition) |

此示例演示如何使用 Opacity 属性和转换进行时启用该控件淡入和淡出，它处于禁用状态时的按钮。

```xaml
<Button x:Name="SubmitButton"
        Content="Submit"
        Opacity="{x:Bind OpaqueIfEnabled(SubmitButton.IsEnabled), Mode=OneWay}">
    <Button.OpacityTransition>
        <ScalarTransition />
    </Button.OpacityTransition>
</Button>
```

```csharp
public double OpaqueIfEnabled(bool IsEnabled)
{
    return IsEnabled ? 1.0 : 0.2;
}
```

## <a name="related-articles"></a>相关文章

- [Motion 概述](index.md)
- [计时和缓动](timing-and-easing.md)
- [方向性和重力](directionality-and-gravity.md)
