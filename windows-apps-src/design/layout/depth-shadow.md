---
author: knicholasa
description: 使用 Z 深度（也称为相对深度）和阴影，是将深度整合到应用中以帮助用户以自然高效的方式聚焦的两种做法。
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
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/21/2020
ms.locfileid: "80081390"
---
# <a name="z-depth-and-shadow"></a>Z 深度和阴影

![一个 gif 图形，显示沿对角线方向堆叠的四个灰色矩形（一个堆叠于另一个之上）。 该 gif 图形采用了动画效果，以便使阴影出现和消失。](images/elevation-shadow/shadow.gif)

在 UI 中创建元素的视觉层次结构可以使 UI 易于扫描，并可传达需要重点关注的信息。 提升（使所选 UI 元素前进的动作）通常用于在软件中实现此类层次结构。 本文介绍如何使用 Z 深度和阴影在 UWP 应用中创建提升效果。

Z 深度是在 3D 应用创建者中使用的一个术语，用于表示在 Z 轴方向上两个图面之间的距离。 它演示对象与观看者的靠近程度。 可将其概念视为类似于 X/Y 坐标，但方向为 Z。

## <a name="why-use-z-depth"></a>为何使用 Z 深度？

在现实生活中，我们往往会将视线聚焦于离我们较近的对象。 对于数字 UI，我们也可以运用这种空间本能。 例如，如果使某个元素越来越靠近用户，则用户会本能地将视线聚焦于该元素。 沿 Z 轴方向移近 UI 元素时，可以在对象之间建立视觉层次结构，从而帮助用户在应用中自然高效地完成任务。

## <a name="what-is-shadow"></a>什么是阴影？

阴影是用户感受提升的一种方式。 提升的对象上面的光线会在下方图面上产生阴影。 对象越高，阴影就越大且越柔和。 UI 中提升的对象不需要有阴影，但阴影有助于表现提升。

在 UWP 应用中，应该有目的性地使用阴影，而不能只是出于美观。 使用过多的阴影会减少或消除阴影吸引用户注意力的能力。

如果使用标准控件，ThemeShadow 阴影将自动整合到 UI 中。 但是，可以使用 ThemeShadow 或 DropShadow API 在 UI 中手动包含阴影。 

## <a name="themeshadow"></a>ThemeShadow

可将 [ThemeShadow](/uwp/api/windows.ui.xaml.media.themeshadow) 类型应用到任何 XAML 元素，以根据 X、Y、Z 坐标适当绘制阴影。 ThemeShadow 还会根据其他环境规范自动进行调整：

- 根据光线、用户主题、应用环境和外壳方面的变化进行调整。
- 根据元素的 Z 深度自动对元素应用阴影。 
- 在元素移动和更改高度时使其保持同步。
- 使阴影在整个应用程序中和不同的应用程序之间保持一致。

下面介绍如何在 MenuFlyout 上实现 ThemeShadow。 MenuFlyout 提供一个内置的体验，其中的主图面已提升至 32 像素处，每个附加的级联菜单在其上级菜单上方的 +8 像素处打开。

![应用到 MenuFlyout 的 ThemeShadow 屏幕截图，其中显示了三个打开的嵌套菜单。 第一个菜单已提升 32 像素，从上一菜单打开的每个后续菜单进一步提升 8 像素，因此在背景上留下了一个不同的阴影。](images/elevation-shadow/themeshadow-menuflyout.png)

### <a name="themeshadow-in-common-controls"></a>常用控件中的 ThemeShadow

除非另有说明，否则以下常用控件将自动使用 ThemeShadow 从 32 像素深度处投射阴影：

- [上下文菜单](../controls-and-patterns/menus.md)、[命令栏](../controls-and-patterns/app-bars.md)、[命令栏浮出控件](../controls-and-patterns/command-bar-flyout.md)、[菜单栏](../controls-and-patterns/menus.md#create-a-menu-bar)
- [对话框和浮出控件](../controls-and-patterns/dialogs.md)（64 像素的对话框）
- [NavigationView](../controls-and-patterns/navigationview.md)
- [ComboBox](../controls-and-patterns/combo-box.md)、[DropDownButton、SplitButton、ToggleSplitButton](../controls-and-patterns/buttons.md)
- [TeachingTip](../controls-and-patterns/dialogs-and-flyouts/teaching-tip.md)
- [AutoSuggestBox](../controls-and-patterns/auto-suggest-box.md) 
- [日历/日期/时间选取器](../controls-and-patterns/date-and-time.md)
- [工具提示](../controls-and-patterns/tooltips.md)（16 像素）
- [媒体传输控件](../controls-and-patterns/media-playback.md#media-transport-controls)、[InkToolbar](../controls-and-patterns/inking-controls.md)
- [连贯的动画](../motion/connected-animation.md)

注意：浮出控件仅在针对 Windows 10 版本 1903 或更高版本的 SDK 进行编译时才应用 ThemeShadow。

### <a name="themeshadow-in-popups"></a>弹出窗口中的 ThemeShadow

如果你需要吸引用户的注意并让其采取快速操作，则经常会在应用的 UI 中使用弹出窗口。 这种情况就是借助阴影在应用 UI 中创建层次结构的极好例子。

在应用到[弹出窗口](/uwp/api/windows.ui.xaml.controls.primitives.popup)中的任何 XAML 元素时，ThemeShadow 会自动投射阴影。 它会在其后面的应用背景内容上以及在其下面的任何其他已打开弹出窗口上投射阴影。

若要对弹出窗口使用 ThemeShadow，请使用 `Shadow` 属性将 ThemeShadow 应用到 XAML 元素。 然后，相对于该元素后面的其他元素提升该元素（例如，使用 `Translation` 属性的 Z 组件）。
对于大多数弹出窗口 UI，相对于应用背景内容的建议默认提升值为 32 个有效像素。

本示例在弹出窗口中显示一个矩形，它将阴影投射到应用背景内容及其后面的任何其他弹出窗口：

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

![带有阴影的单个矩形弹出窗口。](images/elevation-shadow/PopupRectangle.png)

### <a name="disabling-default-themeshadow-on-custom-flyout-controls"></a>在自定义浮出控件上禁用默认的 ThemeShadow

基于 [Flyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.flyout)、[DatePickerFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.datepickerflyout)、[MenuFlyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.menuflyout) 或 [TimePickerFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.timepickerflyout) 的控件会自动使用 ThemeShadow 来投射阴影。

如果控件内容上的默认阴影看起来不正确，你可以通过将关联 FlyoutPresenter 上的 [IsDefaultShadowEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.flyoutpresenter.isdefaultshadowenabled) 属性设置为 `false` 来禁用默认阴影：

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

一般情况下，我们建议仔细考虑是否要使用阴影，并只在阴影能够带来有意义的视觉层次结构时，才使用阴影。 但是，如果你的高级方案中有必要用到阴影，可以通过我们提供的方式从任何 UI 元素投射阴影。

若要从不在弹出窗口中的 XAML 元素投射阴影，必须在 `ThemeShadow.Receivers` 集合中显式指定可接收阴影的其他元素。 接收器不能是视觉树中投射器的上级。

本示例演示两个矩形，这些矩形在其后面的网格上投射阴影：

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

![两个青绿色的矩形彼此相邻，都带有阴影。](images/elevation-shadow/SharedShadow.png)

### <a name="performance-best-practices-for-themeshadow"></a>ThemeShadow 的性能最佳做法

1. 系统将投射器-接收器对的数量限制设置为 5 个，如果超过此限制，则会关闭阴影。 请遵守系统实施的投射器-接收器对的数量限制（5 个）。

2. 请将自定义接收器元素的数量限制为所需的最小值。

3. 如果多个接收器元素处于相同的高度，请尝试通过将目标限定为单个父元素，来合并这些元素。

4. 如果多个元素将相同类型的阴影投射到相同的接收器元素，请将阴影添加为共享资源并重用它。

## <a name="drop-shadow"></a>投影

DropShadow 不会自动对其环境做出响应，且不使用光源。 有关示例实现，请参阅 [DropShadow 类](https://docs.microsoft.com/uwp/api/windows.ui.composition.dropshadow)。

## <a name="which-shadow-should-i-use"></a>我应该使用哪种阴影？

| 属性 | ThemeShadow | DropShadow |
| - | - | - |
| **Min SDK** | Windows 10 版本 1903 | 14393 |
| **适应性** | 是 | 否 |
| **自定义** | 否 | 是 |
| **光源** | 自动（默认为全局，但可按应用重写） | 无 |
| **在3D 环境中受支持** | 是 | 否 |

- 请记住，阴影的作用是提供有意义的层次结构，而不是简单的视觉处理。
- 一般情况下，我们建议使用可根据环境自动调整的 ThemeShadow。
- 如果有性能方面的忧虑，请限制阴影数目，使用其他视觉处理方法，或使用 DropShadow。
- 如果通过更高级的方案实现视觉层次结构，请考虑使用其他视觉处理方法（例如颜色）。 如果需要阴影，请使用 DropShadow。
