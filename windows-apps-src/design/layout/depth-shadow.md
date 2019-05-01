---
author: knicholasa
description: Z 深度或相对深度和卷影是通过两种方法合并到应用来帮助用户专注于自然和高效的深度。
title: Z 深度和适用于 UWP 应用的卷影
template: detail.hbs
ms.author: nichola
ms.date: 04/19/2019
ms.topic: article
ms.custom: 19H1
keywords: windows 10, uwp
pm-contact: chigy
ms.localizationpriority: medium
ms.openlocfilehash: ab49f13d3938e55750ce523f9e0d4ae241304763
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2019
ms.locfileid: "63817715"
---
# <a name="z-depth-and-shadow"></a>Z 深度和阴影

![为 gif 图像显示四个灰色矩形的对角线，堆积在一起。 以便阴影显示和隐藏的动画 gif。](images/elevation-shadow/shadow.gif)

在你的 UI 中创建的元素的可视层次结构使得 UI 更方便地浏览和传达特别关注。 提升，自带的 UI，选定元素的行为通常用于实现软件中的此类的层次结构。 本文讨论如何使用 z 深度和卷影 UWP 应用中创建提升。

Z 深度是在 3D 应用创建者之间用来表示两个面沿 z 轴之间的距离的术语。 它演示了如何关闭对象是到查看器。 将其视为类似的概念为 x / y 坐标，但在 z 方向。

## <a name="why-use-z-depth"></a>为什么要使用 z 深度？

在物理世界中，我们倾向于关注更接近于我们的对象。 我们可以应用于数字 UI 以及此空间想到。 例如，如果向用户显示元素更接近，然后用户将会本能地重点放在元素上。 通过移动 UI 元素 z 轴更近，可以建立对象，帮助应用程序中自然、 高效地完成任务的用户之间的可视化层次结构。

## <a name="what-is-shadow"></a>阴影是什么？

阴影是一种方法就用户感知提升。 提升对象上的光线下图面上创建阴影。 更高版本的对象、 较大和更柔和阴影变得。 在 UI 中的提升的权限对象不需要具有阴影，但它们可帮助提升的外观。

在 UWP 应用中，应有意而不是美观的方式使用阴影。 使用太多阴影将减少或消除阴影的功能，可以将精力集中用户。

如果使用标准控件，将会自动将 ThemeShadow 阴影合并到您的 UI。 但是，您可以手动将包括阴影在 UI 中使用 ThemeShadow 或 DropShadow Api。 

## <a name="themeshadow"></a>ThemeShadow

类型可以应用于任何 XAML 元素来绘制的 ThemeShadow 遮盖了具有适当地根据的 x、 y、 z 坐标。 有关其他环境规范还自动调节 ThemeShadow:

- 调整照明、 用户主题、 应用程序环境中，和 shell 中的更改。
- 适用于自动基于其 z 深度元素的阴影。 
- 它们移动并更改提升保留元素保持同步。
- 使阴影在整个以及跨应用程序一致。

下面是上 MenuFlyout ThemeShadow 实现已方式。 MenuFlyout 具有内置的体验，其中主要面提升为 32 px 且每个其他的级联菜单打开 + 8px 上面从打开的菜单。

![应用于具有三个开放的嵌套菜单 MenuFlyout ThemeShadow 屏幕截图。 第一个菜单是提升 32 px 和从上一个菜单中打开每个后续菜单是提升的 8px 详细，这样它将不同的卷影留在背景上。](images/elevation-shadow/themeshadow-menuflyout.png)

### <a name="themeshadow-in-common-controls"></a>ThemeShadow 共同控制

以下公共控件将自动使用 ThemeShadow 以转换从 32 px 深度的阴影，除非另有指定：

- [上下文菜单](../controls-and-patterns/menus.md)，[命令栏](../controls-and-patterns/app-bars.md)，[命令栏浮出控件](../controls-and-patterns/command-bar-flyout.md)，[菜单栏](../controls-and-patterns/menus.md#create-a-menu-bar)
- [对话框和浮出控件](../controls-and-patterns/dialogs.md)（64px 对话框）
- [NavigationView](../controls-and-patterns/navigationview.md)
- [ComboBox](../controls-and-patterns/combo-box.md)， [DropDownButton，SplitButton，ToggleSplitButton](../controls-and-patterns/buttons.md)
- [TeachingTip](../controls-and-patterns/dialogs-and-flyouts/teaching-tip.md)
- [AutoSuggestBox](../controls-and-patterns/auto-suggest-box.md) 
- [日历/日期/时间选取器](../controls-and-patterns/date-and-time.md)
- [工具提示](../controls-and-patterns/tooltips.md)(16px)
- [媒体传输控件](../controls-and-patterns/media-playback.md#media-transport-controls)， [InkToolbar](../controls-and-patterns/inking-controls.md)
- [连接的动画](../motion/connected-animation.md)

注意：浮出控件将仅应用 ThemeShadow 编译针对 Windows 10 版本 1903年或较新的 SDK 时。

### <a name="themeshadow-in-popups"></a>在弹出窗口中的 ThemeShadow

它通常是指，应用程序的 UI 使用一个弹出窗口的方案需要用户的关注和快速操作。 卷影应使用以帮助在应用程序的 UI 中创建层次结构时，这些是很好示例。

ThemeShadow 自动强制转换时应用于中的任何 XAML 元素的阴影[弹出窗口](/uwp/api/windows.ui.xaml.controls.primitives.popup)。 它将转换后它和它下面任何其他打开的弹出窗口的应用程序后台内容上的阴影。

若要使用弹出窗口 ThemeShadow，使用`Shadow`ThemeShadow 对 XAML 元素的属性。 然后，该元素从其他元素后面，例如使用提升数的 z 分量`Translation`属性。
对于大多数的弹出 UI，相对于应用的背景内容的推荐的默认提升是 32 有效像素。

此示例显示一个弹出窗口中投影到应用背景内容和它后面的任何其他弹出窗口的矩形：

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

![单个矩形 popup 具有阴影。](images/elevation-shadow/PopupRectangle.png)

### <a name="disabling-default-themeshadow-on-custom-flyout-controls"></a>禁用默认 ThemeShadow 上自定义的浮出控件

控件基于[浮出控件](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.flyout)， [DatePickerFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.datepickerflyout)， [MenuFlyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.menuflyout)或者[TimePickerFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.timepickerflyout)自动使用 ThemeShadow 强制转换卷影。

如果默认卷影看起来不正确上控件的内容，则可以通过设置来禁用[IsDefaultShadowEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.flyoutpresenter.isdefaultshadowenabled)属性设置为`false`关联 FlyoutPresenter 上：

```xaml
<Flyout>
    <Flyout.FlyoutPresenterStyle>
        <Style TargetType="FlyoutPresenter">
            <Setter Property="IsDefaultShadowEnabled" Value="False" />
        </Style>
    </Flyout.FlyoutPresenterStyle>
</Flyout>
```

### <a name="themeshadow-in-other-elements"></a>在其他元素中的 ThemeShadow

一般情况下我们鼓励您仔细考虑你的卷影使用，并限制到其中，它引入了有意义的可视化层次结构的情况下使用。 但是，我们提供一种方法，以在具有高级方案需要它的情况下强制转换任何 UI 元素中的阴影。

若要强制转换不在弹出窗口的 XAML 元素从阴影，则必须显式指定的其他元素可以接收在卷影`ThemeShadow.Receivers`集合。 接收方不能为万向可视化树中的上级。

此示例演示强制转换到背后的网格上的阴影的两个矩形：

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

![两个相邻，它们都具有阴影的蓝绿色矩形。](images/elevation-shadow/SharedShadow.png)

### <a name="performance-best-practices-for-themeshadow"></a>ThemeShadow 的性能最佳实践

1. 系统设置的限制为 5 万向接收器对，并将卷影如果，则关闭超出此限制。 坚持 5 万向接收器对的系统上强制实施限制。

2. 限制为一个小的必要的自定义接收器元素数。

3. 如果多个接收方元素位于相同的提升，请尝试将它们合并通过改为以单个父元素为目标。

4. 如果多个元素将转换到相同的接收方元素上的卷影的相同类型然后将卷影添加为共享资源，并重复使用它。

## <a name="drop-shadow"></a>投影

DropShadow 不是对其环境的自动响应能力，并且不使用光源。 有关示例实现中，请参阅[DropShadow 类](https://docs.microsoft.com/uwp/api/windows.ui.composition.dropshadow)。

## <a name="which-shadow-should-i-use"></a>应使用哪个卷影？

| 属性 | ThemeShadow | DropShadow |
| - | - | - | - |
| **Min SDK** | Windows 10 版本 1903 | 14393 |
| **适应性** | 是 | 否 |
| **自定义项** | 否 | 是 |
| **光源** | 自动 (默认情况下，全局，但可以重写每个应用) | 无 |
| **在 3D 环境中受支持** | 是 | 否 |

- 请记住，阴影的用途是提供有意义的层次结构，而不是作为简单的视觉处理。
- 通常情况下，我们建议使用 ThemeShadow，自动适应其环境。
- 有关性能问题，限制阴影的数目、 使用其他的视觉处理，或使用 DropShadow。
- 如果你具有更高级的方案来实现的可视化层次结构，请考虑使用其他的视觉处理 （例如颜色）。 如果需要卷影，然后使用 DropShadow。
