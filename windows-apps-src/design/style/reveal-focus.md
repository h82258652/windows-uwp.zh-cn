---
author: cphilippona
description: 显示焦点是一种灯光效果，当用户将游戏板或键盘焦点移到它们时，可聚焦元素的边框进行动画处理。
title: 显示焦点
template: detail.hbs
ms.author: mijacobs
ms.date: 03/1/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: chphilip
design-contact: ''
dev-contact: stevenki
ms.localizationpriority: medium
ms.openlocfilehash: 7b5fa84efbe20368be55a50ce20c8e6e5d1fe439
ms.sourcegitcommit: 63cef0a7805f1594984da4d4ff2f76894f12d942
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/05/2018
ms.locfileid: "4389769"
---
# <a name="reveal-focus"></a>显示焦点

![主图](images/header-reveal-focus.svg)

显示焦点是[10 英尺体验](/windows/uwp/design/devices/designing-for-tv)，例如 Xbox One 和电视屏幕的照明效果。 当用户将游戏板或键盘焦点移向可聚焦元素（如按钮）时，它会将这些元素的边框进行动画处理。 默认情况下，它是关闭状态，但启用很简单。 

（展示突出显示效果，突出显示交互性元素的灯光请参阅[展示突出显示文章](/windows/uwp/design/style/reveal)。）


> **重要 API**：[Application.FocusVisualKind 属性](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.FocusVisualKind)、[FocusVisualKind 枚举](https://docs.microsoft.com/uwp/api/windows.ui.xaml.focusvisualkind)、[Control.UseSystemFocusVisuals 属性](/uwp/api/Windows.UI.Xaml.Controls.Control.UseSystemFocusVisuals)

## <a name="how-it-works"></a>工作原理
通过添加动画的明亮辉光的元素边框周围显示焦点引起对聚焦元素：

![显示视觉](images/traveling-focus-fullscreen-light-rf.gif)

这是 10 英尺场景中的用户可能不会完全注意整个电视屏幕尤其有用。 

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果你安装了该<strong style="font-weight: semi-bold">XAML 控件库</strong>应用，单击此处<a href="xamlcontrolsgallery:/item/RevealFocus">打开该应用，请参阅展示的实际的焦点</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="how-to-use-it"></a>如何使用

显示焦点是关闭默认情况下。 如何启用：
1. 在应用的构造函数中，调用 [AnalyticsInfo.VersionInfo.DeviceFamily](/uwp/api/windows.system.profile.analyticsversioninfo.DeviceFamily) 属性，并检查当前设备系列是否是 `Windows.Xbox`。
2. 如果设备系列是 `Windows.Xbox`，将 [Application.FocusVisualKind](/uwp/api/windows.ui.xaml.application.FocusVisualKind) 属性设置为 `FocusVisualKind.Reveal`。 

```csharp
    if(AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Xbox")
    {
        this.FocusVisualKind = FocusVisualKind.Reveal;
    }
```

设置**FocusVisualKind**属性后，系统自动将展示焦点效果应用于其[UseSystemFocusVisuals](/uwp/api/Windows.UI.Xaml.Controls.Control.UseSystemFocusVisuals)属性设置为**True** （大多数控件的默认值） 的所有控件。 

## <a name="why-isnt-reveal-focus-on-by-default"></a>为何焦点不是展示在默认情况下？ 
如你所见，很容易启用展示焦点时在应用检测到它正在 Xbox 上运行。 那么，系统为何不为你打开它？ 展示焦点会增加焦点视觉对象的大小，因为这可能导致你的 UI 布局问题。 在某些情况下，你会想要自定义显示焦点效果来优化你的应用。

## <a name="customizing-reveal-focus"></a>自定义显示焦点

你可以通过修改每个控件的焦点视觉属性来自定义显示焦点效果： [FocusVisualPrimaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryThickness)、 [FocusVisualSecondaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryThickness)、 [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush)，以及[FocusVisualSecondaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryBrush)。 这些属性让你可以自定义焦点矩形的颜色和粗细。 （它们与你用于创建[高可见性焦点视觉](https://docs.microsoft.com/windows/uwp/design/input/guidelines-for-visualfeedback#high-visibility-focus-visuals)的属性相同。） 

但在开始对构成之前，最好先稍有多了解构成展示焦点的组件。

有默认的展示焦点视觉的三个部分： 主边框、 辅助边框和显示明亮辉光。 主边框为 **2px** 粗，在辅助边框的*外部*周围运行。 辅助边框为 **1px** 粗，在主边框的*内部*周围运行。 展示焦点发光具有粗细与主边框的粗细成比例，并且*之外*的主边框周围运行。

除了静态元素中，展示焦点视觉功能动画的灯光，跳动时移动焦点时，焦点方向移动。

![显示焦点层](images/reveal-breakdown.svg)

## <a name="customize-the-border-thickness"></a>自定义边框粗细

若要更改控件边框类型的粗细，请使用这些属性：

| 边框类型 | 属性 |
| --- | --- |
| 主边框、明亮辉光   | [FocusVisualPrimaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryThickness)<br/> （更改主边框将按比例改变明亮辉光的粗细。）   |
| 辅助边框   | [FocusVisualSecondaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryThickness)   |


此示例中更改按钮焦点视觉的边框粗细：

```xaml
<Button FocusVisualPrimaryThickness="2" FocusVisualSecondaryThickness="1"/>
```

## <a name="customize-the-margin"></a>自定义边距

边距是控件的视觉边界与焦点视觉辅助边框的开始部分之间的间距。 默认边距与控件边界的距离为 1px。 可以通过更改 [FocusVisualMargin](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualMargin) 属性来基于每个控件编辑此边距：

```xaml
<Button FocusVisualPrimaryThickness="2" FocusVisualSecondaryThickness="1" FocusVisualMargin="-3"/>
```

负边距将边框推离控件的中心，而正边距将边框移向控件的中心。

## <a name="customize-the-color"></a>自定义颜色

若要更改显示焦点视觉的颜色，使用[FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush)和[FocusVisualSecondaryBrush](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryBrush)属性。

| 属性 | 默认资源 | 默认资源值 |
| ---- | ---- | --- | 
| [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush) | SystemControlRevealFocusVisualBrush  | SystemAccentColor |
| [FocusVisualSecondaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryBrush)  | SystemControlFocusVisualSecondaryBrush  | SystemAltMediumColor |

（当 **FocusVisualKind** 设置为**显示**时，FocusPrimaryBrush 属性仅默认使用 **SystemControlRevealFocusVisualBrush** 资源。 否则，它将使用 **SystemControlFocusVisualPrimaryBrush**。）


若要更改单个控件焦点视觉的颜色，请直接在控件上设置属性。 此示例中覆盖按钮的焦点视觉颜色。

```xaml

<!-- Specifying a color directly -->
<Button
    FocusVisualPrimaryBrush="DarkRed"
    FocusVisualSecondaryBrush="Pink"/>

<!-- Using theme resources -->
<Button
    FocusVisualPrimaryBrush="{ThemeResource SystemBaseHighColor}"
    FocusVisualSecondaryBrush="{ThemeResource SystemAccentColor}"/>    
```

若要更改整个应用中所有焦点视觉的颜色，请使用你自己的定义替代 **SystemControlRevealFocusVisualBrush** 和 **SystemControlFocusVisualSecondaryBrush** 资源：

```xaml
<!-- App.xaml -->
<Application.Resources>

    <!-- Override Reveal Focus default resources. -->
    <SolidColorBrush x:Key="SystemControlRevealFocusVisualBrush" Color="{ThemeResource SystemBaseHighColor}"/>
    <SolidColorBrush x:Key="SystemControlFocusVisualSecondaryBrush" Color="{ThemeResource SystemAccentColor}"/>
</Application.Resources>
```

有关修改焦点视觉颜色的详细信息，请参阅[颜色外观方案和自定义](https://docs.microsoft.com/windows/uwp/design/input/guidelines-for-visualfeedback#color-branding--customizing)。


## <a name="show-just-the-glow"></a>只显示明亮辉光

如果你想要只使用明亮辉光，而不要主边框或辅助边框焦点视觉，只需将控件的 [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush) 属性设置为 `Transparent`，并将 [FocusVisualSecondaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryThickness) 设置为 `0`。 在此情况下，明亮辉光将采用控件背景的颜色来提供无边框感觉。 你可以使用 [FocusVisualPrimaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryThickness) 来修改明亮辉光的粗细。

```xaml

<!-- Show just the glow -->
<Button
    FocusVisualPrimaryBrush="Transparent"
    FocusVisualSecondaryThickness="0" />    
```


## <a name="use-your-own-focus-visuals"></a>使用你自己的焦点视觉

若要自定义显示焦点的另一个方法是通过使用视觉状态绘制你自己选择退出系统提供的焦点视觉。 若要了解详细信息，请参阅[焦点视觉示例](http://go.microsoft.com/fwlink/p/?LinkID=619895)。


## <a name="reveal-focus-and-the-fluent-design-system"></a>显示焦点和 Fluent 设计系统

显示焦点是 Fluent 设计系统组件，它将光线添加到你的应用。 若要了解有关 Fluent Design 系统及其其他组件的详细信息，请参阅 [UWP 的 Fluent Design 概述](../fluent-design-system/index.md)。

## <a name="related-articles"></a>相关文章

- [展示突出显示](https://docs.microsoft.com/windows/uwp/design/style/reveal)
- [针对 Xbox 和电视进行设计](/windows/uwp/design/devices/designing-for-tv)
- [游戏板和遥控器交互](https://docs.microsoft.com/windows/uwp/design/input/gamepad-and-remote-interactions)
- [焦点视觉示例](http://go.microsoft.com/fwlink/p/?LinkID=619895)
- [合成效果](https://msdn.microsoft.com/windows/uwp/graphics/composition-effects)
- [系统内的科学：Fluent Design 和深色](https://medium.com/microsoft-design/science-in-the-system-fluent-design-and-depth-fb6d0f23a53f)
- [系统内的科学：Fluent 设计和浅色](https://medium.com/microsoft-design/the-science-in-the-system-fluent-design-and-light-94a17e0b3a4f)
