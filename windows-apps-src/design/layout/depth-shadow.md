---
author: knicholasa
description: Z 深度或相对深度和阴影是将深度合并到应用中的两种方式，可帮助用户自然高效地定位。
title: 适用于 UWP 应用的 Z 深度和阴影
template: detail.hbs
ms.date: 04/19/2019
ms.topic: article
ms.custom: 19H1
keywords: windows 10, uwp
pm-contact: chigy
ms.localizationpriority: medium
ms.openlocfilehash: 216974ba564a192f94473469f3a7a49191ef2192
ms.sourcegitcommit: af4050f69168c15b0afaaa8eea66a5ee38b88fed
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/21/2020
ms.locfileid: "80081390"
---
# <a name="z-depth-and-shadow"></a>Z 深度和阴影

![显示四个灰色矩形的 gif，其中一个沿对角堆积。 Gif 将以动画形式显示，使阴影出现和消失。](images/elevation-shadow/shadow.gif)

在用户界面中创建元素的可视层次结构使 UI 易于扫描并传达重点关注的重要内容。 提升后，将 UI 向前引入 select 元素的操作通常用于在软件中实现此类层次结构。 本文介绍如何使用 z 深度和阴影在 UWP 应用中创建提升。

Z 深度是指3D 应用创建者中使用的一种术语，用于表示两个曲面沿 z 轴的距离。 它说明了如何使对象与查看器接近。 将其视为 x/y 坐标的类似概念，而不是 z 方向。

## <a name="why-use-z-depth"></a>为什么使用 z 深度？

在实际情况下，我们倾向于将重点放在离我们更近的对象上。 我们也可以将此空间 instinct 应用于数字 UI。 例如，如果使某个元素更接近用户，则用户会将焦点自然而然地在该元素上。 通过在 z 轴中移动 UI 元素，可以在对象之间建立可视层次结构，从而帮助用户在应用中自然有效地完成任务。

## <a name="what-is-shadow"></a>什么是阴影？

影子是用户感觉提升的一种方式。 较高的对象在下面的表面上创建阴影。 对象越高，阴影就越大越好。 用户界面中提升的对象不需要阴影，而是有助于创建提升的外观。

在 UWP 应用中，隐藏应在目的性中使用，而不是美观方式。 如果使用太多的阴影，则会减少或消除阴影使用户聚焦的能力。

如果使用标准控件，ThemeShadow 的阴影将自动合并到 UI 中。 但是，可以使用 ThemeShadow 或 DropShadow Api 在 UI 中手动包含阴影。 

## <a name="themeshadow"></a>ThemeShadow

[ThemeShadow](/uwp/api/windows.ui.xaml.media.themeshadow)类型可应用于任何 XAML 元素，以根据 x、y、z 坐标适当绘制阴影。 ThemeShadow 还自动调整其他环境规范：

- 适应灯光、用户主题、应用环境和 shell 中的变化。
- 基于元素的 z 深度，自动对元素应用阴影。 
- 在元素移动和更改提升时使其保持同步。
- 使所有应用程序的阴影保持一致。

下面介绍如何在 MenuFlyout 上实现 ThemeShadow。 MenuFlyout 有一个内置体验，其中主图面提升到32px，而每个附加的级联菜单在其打开的菜单上方打开 + 8px。

![应用于具有三个打开的嵌套菜单的 MenuFlyout 的 ThemeShadow 的屏幕截图。 第一个菜单是提升后的32px，从上一菜单打开的每个后续菜单都将提升8px，使其更多，使其在背景上保持不同的阴影。](images/elevation-shadow/themeshadow-menuflyout.png)

### <a name="themeshadow-in-common-controls"></a>公共控件中的 ThemeShadow

除非另有指定，否则以下公共控件将自动使用 ThemeShadow 从32px 深度转换阴影：

- [上下文菜单](../controls-and-patterns/menus.md)、[命令栏](../controls-and-patterns/app-bars.md)、[命令栏飞出](../controls-and-patterns/command-bar-flyout.md)[菜单](../controls-and-patterns/menus.md#create-a-menu-bar)栏
- [对话框和浮出控件](../controls-and-patterns/dialogs.md)（64px 中的对话框）
- [NavigationView](../controls-and-patterns/navigationview.md)
- [ComboBox](../controls-and-patterns/combo-box.md)、 [DropDownButton、SplitButton、ToggleSplitButton](../controls-and-patterns/buttons.md)
- [TeachingTip](../controls-and-patterns/dialogs-and-flyouts/teaching-tip.md)
- [AutoSuggestBox](../controls-and-patterns/auto-suggest-box.md) 
- [日历/日期/时间选取器](../controls-and-patterns/date-and-time.md)
- [Tooltip](../controls-and-patterns/tooltips.md) （16px）
- [媒体传输控制](../controls-and-patterns/media-playback.md#media-transport-controls)， [InkToolbar](../controls-and-patterns/inking-controls.md)
- [连贯的动画](../motion/connected-animation.md)

注意：浮出控件仅在针对 Windows 10 版本1903或更早版本的 SDK 进行编译时应用 ThemeShadow。

### <a name="themeshadow-in-popups"></a>弹出窗口中的 ThemeShadow

通常情况下，应用的 UI 会在需要用户注意和快速操作的情况下使用弹出窗口。 在应用程序的 UI 中，当应该使用影子来帮助创建层次结构时，这些是很好的示例。

当应用于[弹出窗口](/uwp/api/windows.ui.xaml.controls.primitives.popup)中的任何 XAML 元素时，ThemeShadow 会自动强制转换阴影。 它将在其后面的应用背景内容和它下面的任何其他打开的弹出窗口之间强制转换。

若要将 ThemeShadow 与弹出窗口一起使用，请使用 `Shadow` 属性将 ThemeShadow 应用于 XAML 元素。 然后，将该元素从其后面的其他元素中提升（例如，通过使用 `Translation` 属性的 z 组件）。
对于大多数 Popup UI，相对于应用背景内容的建议默认提升为32有效像素。

此示例在弹出窗口中显示一个将阴影转换到应用背景内容以及它后面的任何其他弹出窗口的矩形：

```xaml
<Popup>
    <Rectangle x:Name="PopupRectangle" Fill="Lavender" Height="48" Width="96">
        <Rectangle.Shadow>
            <ThemeShadow />
        </Rectangle.Shadow>
    </Rectangle>
</Popup>
```

```csharp
// Elevate the rectangle by 32px
PopupRectangle.Translation += new Vector3(0, 0, 32);
```

![带有阴影的单个矩形 popup。](images/elevation-shadow/PopupRectangle.png)

### <a name="disabling-default-themeshadow-on-custom-flyout-controls"></a>禁用自定义飞出控件上的默认 ThemeShadow

基于[浮出](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.flyout)控件、 [DatePickerFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.datepickerflyout)、 [MenuFlyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.menuflyout)或[TimePickerFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.timepickerflyout)的控件自动使用 ThemeShadow 来转换阴影。

如果控件内容的默认阴影不正确，则可以通过将[IsDefaultShadowEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.flyoutpresenter.isdefaultshadowenabled)属性设置为关联的 FlyoutPresenter 上的 `false` 来禁用它：

```xaml
<Flyout>
    <Flyout.FlyoutPresenterStyle>
        <Style TargetType="FlyoutPresenter">
            <Setter Property="IsDefaultShadowEnabled" Value="False" />
        </Style>
    </Flyout.FlyoutPresenterStyle>
</Flyout>
```

### <a name="themeshadow-in-other-elements"></a>其他元素中的 ThemeShadow

一般情况下，我们鼓励你认真考虑使用影子，并将其使用限制为引入有意义的视觉层次结构。 但是，我们提供了一种方法，可从任何 UI 元素转换阴影，以防有必要的高级方案。

若要从不在弹出窗口中的 XAML 元素转换阴影，你必须显式指定可接收 `ThemeShadow.Receivers` 集合中的阴影的其他元素。 接收方不能在可视化树中作为 caster 的上级。

此示例演示两个将阴影投射到其后的网格上的矩形：

```xaml
<Grid>
    <Grid.Resources>
        <ThemeShadow x:Name="SharedShadow" />
    </Grid.Resources>

    <Grid x:Name="BackgroundGrid" Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" />

    <Rectangle x:Name="Rectangle1" Height="100" Width="100" Fill="Turquoise" Shadow="{StaticResource SharedShadow}" />

    <Rectangle x:Name="Rectangle2" Height="100" Width="100" Fill="Turquoise" Shadow="{StaticResource SharedShadow}" />
</Grid>
```

```csharp
/// Add BackgroundGrid as a shadow receiver and elevate the casting buttons above it
SharedShadow.Receivers.Add(BackgroundGrid);

Rectangle1.Translation += new Vector3(0, 0, 16);
Rectangle2.Translation += new Vector3(120, 0, 32);
```

![两个蓝绿色矩形彼此相邻，都带有阴影。](images/elevation-shadow/SharedShadow.png)

### <a name="performance-best-practices-for-themeshadow"></a>ThemeShadow 的性能最佳实践

1. 系统设置的限制为5个 caster 对，如果超过此限制，则将关闭影子。 坚持使用5个 caster 的系统强制限制。

2. 将自定义接收方元素的数目限制为所需的最小值。

3. 如果多个接收方元素处于相同的提升状态，请尝试通过以单个父元素为目标来合并它们。

4. 如果有多个元素将相同类型的阴影强制转换为相同的接收方元素，则将阴影添加为共享资源并重复使用它。

## <a name="drop-shadow"></a>投影

DropShadow 不会自动响应其环境，也不会使用光源。 有关示例实现，请参阅[DropShadow 类](https://docs.microsoft.com/uwp/api/windows.ui.composition.dropshadow)。

## <a name="which-shadow-should-i-use"></a>我应该使用哪种阴影？

| 属性 | ThemeShadow | DropShadow |
| - | - | - |
| **最小 SDK** | Windows 10 版本1903 | 14393 |
| **能力** | 是 | 是 |
| **定义** | 是 | 是 |
| **光源** | 自动（默认情况下为全局，但可以覆盖每个应用） | 无 |
| **在3D 环境中受支持** | 是 | 是 |

- 请记住，影子的用途是提供有意义的层次结构，而不是简单的视觉处理。
- 通常，我们建议使用 ThemeShadow，它可自动适应其环境。
- 有关性能的问题，请限制阴影数量，使用其他视觉对象处理，或使用 DropShadow。
- 如果具有更高级的方案来实现视觉层次结构，请考虑使用其他视觉对象处理方式（例如，颜色）。 如果需要 shadow，请使用 DropShadow。
