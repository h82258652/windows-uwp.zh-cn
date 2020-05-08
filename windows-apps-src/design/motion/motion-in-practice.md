---
Description: 了解你的应用中的流畅运动基础知识。
title: 练习中的动作-Windows 应用中的动画
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
ms.openlocfilehash: 45ab6c593b9e20f778e4b352a8b284cefe57c9a8
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970322"
---
# <a name="bringing-it-together"></a>综合运用

计时、缓动、方向性和引力协同工作，构成了 Fluent 运动的基础。 必须对上述要素进行综合考虑，然后在应用上下文中适当应用。

以下是在应用中应用 Fluent 运动基础的 3 种方法。

:::row:::
    :::column:::
**隐式动画**参数更改中的值之间的自动补间和计时将更改为使用标准化值实现非常简单的流畅运动。
    :::column-end:::
    :::column:::
**内置动画**系统组件（如公用控件和共享动作）为 "默认情况下处于活动状态"。 基础知识的应用方式与其隐含用法一致。
    :::column-end:::
    :::column:::
**遵循指南建议的自定义动画**有时，系统还没有为方案提供精确的运动解决方案。 在这些情况下，请使用基线基本建议作为你的体验的起点。
    :::column-end:::
:::row-end:::

**切换示例**

![功能性动画](images/pageRefresh.gif)

:::row:::
    :::column:::
<b>前进方向：</b><br>
淡出： 150m;缓动：默认加速<b>方向：</b><br>
Slide 150： 300ms;缓动：默认减速
    :::column-end:::
    :::column:::
<b>反向方向：</b><br>
向下滑动150： 150ms;缓动：默认<b>的加速方向：</b><br>
淡入： 300ms;缓动：默认减速
    :::column-end:::
:::row-end:::

**对象示例**

 ![300 ms 运动](images/control.gif)

:::row:::
    :::column:::
<b>方向展开：</b><br>
增长： 300ms;缓动：标准
    :::column-end:::
    :::column:::
<b>方向协定：</b><br>
增长： 150ms;缓动：默认加速
    :::column-end:::
:::row-end:::

## <a name="examples"></a>示例

<table>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>如果安装了<strong style="font-weight: semi-bold">XAML 控件库</strong>应用，请单击此处<a href="xamlcontrolsgallery:/item/ImplicitTransition">打开应用，并查看操作中的隐式转换</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="implicit-animations"></a>隐式动画

> 隐式动画需要 Windows 10 版本1809（[SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)）或更高版本。

隐式动画是一种简单的方法，通过在参数更改期间自动在新值和新值之间进行插值来实现流畅的运动。

您可以隐式地对以下属性的更改进行动画处理：

- [UIElement](/uwp/api/windows.ui.xaml.uielement)
  - **不透明度**
  - **旋转**
  - **缩放**
  - **翻译**

- [Border](/uwp/api/windows.ui.xaml.controls.border)、 [system.windows.controls.contentpresenter>](/uwp/api/windows.ui.xaml.controls.contentpresenter)或[Panel](/uwp/api/windows.ui.xaml.controls.panel)
  - **背景**

可以具有隐式动画的更改的每个属性都有相应的_转换_属性。 若要对属性进行动画处理，请将转换类型分配给相应的_转换_属性。 此表显示了要用于每个的_转换_属性和过渡类型。

| 动画属性 | 转换属性 | 隐式转换类型 |
| -- | -- | -- |
| [UIElement。不透明度](/uwp/api/windows.ui.xaml.uielement.opacity) | [OpacityTransition](/uwp/api/windows.ui.xaml.uielement.opacitytransition) | [ScalarTransition](/uwp/api/windows.ui.xaml.scalartransition) |
| [UIElement.Rotation](/uwp/api/windows.ui.xaml.uielement.rotation) | [RotationTransition](/uwp/api/windows.ui.xaml.uielement.rotationtransition) | [ScalarTransition](/uwp/api/windows.ui.xaml.scalartransition) |
| [UIElement.Scale](/uwp/api/windows.ui.xaml.uielement.scale) | [ScaleTransition](/uwp/api/windows.ui.xaml.uielement.scaletransition) | [Vector3Transition](/uwp/api/windows.ui.xaml.vector3transition) |
| [UIElement.Translation](/uwp/api/windows.ui.xaml.uielement.translation) | [TranslationTransition](/uwp/api/windows.ui.xaml.uielement.translationtransition) | [Vector3Transition](/uwp/api/windows.ui.xaml.vector3transition) |
| [Border. 背景](/uwp/api/windows.ui.xaml.controls.border.background) | [BackgroundTransition](/uwp/api/windows.ui.xaml.controls.border.backgroundtransition) | [BrushTransition](//uwp/api/windows.ui.xaml.uielement.brushtransition) |
| [System.windows.controls.contentpresenter>](/uwp/api/windows.ui.xaml.controls.contentpresenter.background) | [BackgroundTransition](/uwp/api/windows.ui.xaml.controls.contentpresenter.backgroundtransition) | [BrushTransition](//uwp/api/windows.ui.xaml.uielement.brushtransition) |
| [Panel. 背景](/uwp/api/windows.ui.xaml.controls.panel.background) | [BackgroundTransition](/uwp/api/windows.ui.xaml.controls.panel.backgroundtransition)  | [BrushTransition](//uwp/api/windows.ui.xaml.uielement.brushtransition) |

此示例演示如何使用 Opacity 属性和过渡使按钮在控件启用时淡入，并在禁用时淡出。

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

- [运动概述](index.md)
- [计时和缓动](timing-and-easing.md)
- [方向性和重力](directionality-and-gravity.md)
