---
description: 显示重点是用户将游戏板或键盘焦点移到它们时可获得焦点的元素的边框之间进行动画处理的光照效果。
title: 显示焦点
template: detail.hbs
ms.date: 03/01/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: chphilip
design-contact: ''
dev-contact: stevenki
ms.localizationpriority: medium
ms.openlocfilehash: 7bcceb8d44b6d92cab05a9c077531b3fe1b05c79
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651752"
---
# <a name="reveal-focus"></a>显示焦点

![主图](images/header-reveal-focus.svg)

显示焦点位于的光照效果[10 英尺体验](/windows/uwp/design/devices/designing-for-tv)，如 Xbox One 和电视屏幕。 当用户将游戏板或键盘焦点移向可聚焦元素（如按钮）时，它会将这些元素的边框进行动画处理。 默认情况下，它是关闭状态，但启用很简单。 

(突出显示交互元素照明影响显示突出显示效果，请参阅[显示突出显示文章](/windows/uwp/design/style/reveal)。)


> **重要的 Api**:[Application.FocusVisualKind 属性](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.FocusVisualKind)， [FocusVisualKind 枚举](https://docs.microsoft.com/uwp/api/windows.ui.xaml.focusvisualkind)， [Control.UseSystemFocusVisuals 属性](/uwp/api/Windows.UI.Xaml.Controls.Control.UseSystemFocusVisuals)

## <a name="how-it-works"></a>工作原理
通过添加元素的边框四周动画的发光显示将调用注意力集中到已设定焦点的元素：

![显示视觉](images/traveling-focus-fullscreen-light-rf.gif)

这是在其中用户可能不会关注完整整个电视屏幕 10 英尺方案中尤其有用。 

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果有<strong style="font-weight: semi-bold">XAML 控件库</strong>应用程序安装，请单击此处<a href="xamlcontrolsgallery:/item/RevealFocus">打开应用并在操作中看到显示焦点</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用程序 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="how-to-use-it"></a>如何使用

显示焦点默认处于关闭状态。 如何启用：
1. 在应用的构造函数中，调用 [AnalyticsInfo.VersionInfo.DeviceFamily](/uwp/api/windows.system.profile.analyticsversioninfo.DeviceFamily) 属性，并检查当前设备系列是否是 `Windows.Xbox`。
2. 如果设备系列是 `Windows.Xbox`，将 [Application.FocusVisualKind](/uwp/api/windows.ui.xaml.application.FocusVisualKind) 属性设置为 `FocusVisualKind.Reveal`。 

```csharp
    if(AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Xbox")
    {
        this.FocusVisualKind = FocusVisualKind.Reveal;
    }
```

在设置后**FocusVisualKind**属性，系统会自动显示焦点将效果应用到所有控件[UseSystemFocusVisuals](/uwp/api/Windows.UI.Xaml.Controls.Control.UseSystemFocusVisuals)属性设置为**True**（大多数控件默认值）。 

## <a name="why-isnt-reveal-focus-on-by-default"></a>为什么默认情况下不上的显示焦点？ 
正如您所看到的它是相当容易，若要打开显示焦点时该应用程序检测到它在 Xbox 上运行。 那么，系统为何不为你打开它？ 由于显示焦点会增加的焦点视觉对象的大小，这可能导致问题，与 UI 布局。 在某些情况下，你将想要自定义显示焦点效果来优化你的应用。

## <a name="customizing-reveal-focus"></a>自定义显示焦点

可以通过修改每个控件的焦点视觉属性自定义显示焦点效果：[FocusVisualPrimaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryThickness)， [FocusVisualSecondaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryThickness)， [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush)，并且[FocusVisualSecondaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryBrush)。 这些属性让你可以自定义焦点矩形的颜色和粗细。 （它们与你用于创建[高可见性焦点视觉](https://docs.microsoft.com/windows/uwp/design/input/guidelines-for-visualfeedback#high-visibility-focus-visuals)的属性相同。） 

但在开始阅读之前，最好先稍微了解构成显示焦点的组件。

有三个部分默认显示焦点视觉对象： 主边框、 辅助边框和显示发光。 主边框为 **2px** 粗，在辅助边框的*外部*周围运行。 辅助边框为 **1px** 粗，在主边框的*内部*周围运行。 显示焦点发光具有与主边框的粗细成比例的粗细和运行*外部*主边框。

除了静态元素，显示焦点视觉对象功能在悬停时 pulsates 和移动焦点时焦点的方向移动动画的光。

![显示焦点层](images/reveal-breakdown.svg)

## <a name="customize-the-border-thickness"></a>自定义边框粗细

若要更改控件边框类型的粗细，请使用这些属性：

| 边框类型 | 属性 |
| --- | --- |
| 主边框、明亮辉光   | [FocusVisualPrimaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryThickness)<br/> （更改主边框将按比例改变明亮辉光的粗细。）   |
| 辅助   | [FocusVisualSecondaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryThickness)   |


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

若要更改显示焦点视觉对象的颜色，请使用[FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush)并[FocusVisualSecondaryBrush](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryBrush)属性。

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

自定义显示焦点另一种方法是通过绘制您自己使用的视觉状态来选择退出系统提供的焦点视觉对象。 若要了解详细信息，请参阅[焦点视觉示例](https://go.microsoft.com/fwlink/p/?LinkID=619895)。


## <a name="reveal-focus-and-the-fluent-design-system"></a>显示焦点和 Fluent 设计系统

显示焦点是将光添加到您的应用程序的 Fluent 设计系统组件。 若要了解有关 Fluent Design 系统及其其他组件的详细信息，请参阅 [UWP 的 Fluent Design 概述](../fluent-design-system/index.md)。

## <a name="related-articles"></a>相关文章

- [显示突出显示](https://docs.microsoft.com/windows/uwp/design/style/reveal)
- [针对 Xbox 和电视进行设计](/windows/uwp/design/devices/designing-for-tv)
- [游戏手柄和遥控器之间的交互](https://docs.microsoft.com/windows/uwp/design/input/gamepad-and-remote-interactions)
- [焦点视觉对象示例](https://go.microsoft.com/fwlink/p/?LinkID=619895)
- [组合效果](https://msdn.microsoft.com/windows/uwp/graphics/composition-effects)
- [在系统中的科学：Fluent 设计和深度](https://medium.com/microsoft-design/science-in-the-system-fluent-design-and-depth-fb6d0f23a53f)
- [在系统中的科学：Fluent 设计和轻型这类](https://medium.com/microsoft-design/the-science-in-the-system-fluent-design-and-light-94a17e0b3a4f)
