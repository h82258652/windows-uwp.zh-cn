---
author: serenaz
description: Z 深度或相对值深度和阴影的深度融入你的应用来帮助用户专注自然、 高效地两种方法。
title: Z 深度和适用于 UWP 应用的阴影
template: detail.hbs
ms.author: sezhen
ms.date: 02/12/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: chigy
design-contact: balrayit
ms.localizationpriority: medium
ms.openlocfilehash: 3a87e7366765b7c8b304e930fed0d3c45900aad9
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/12/2018
ms.locfileid: "6446004"
---
# <a name="z-depth-and-shadow"></a>Z 深度和阴影

![true 深度](images/elevation-shadow/depth.svg)

Fluent 的深度系统使用物理概念，如 3D 定位，光和阴影完全重做数字 UI 可以察觉的更多分层的物理环境中。 Z 深度或相对值深度和阴影，在两种将深度合并到你的 UWP 应用。

## <a name="what-is-z-depth"></a>什么是 z 深度？

Z 深度是沿 z 轴，两个表面之间的距离，但它说明了对象与查看器的接近程度。

![z 深度](images/elevation-shadow/elevation.svg)

### <a name="why-use-z-depth"></a>为什么使用 z 深度？

在物理世界中，我们倾向于重点介绍靠近我们的对象。 我们可以将此空间想到应用于数字 UI。 例如，如果向用户显示更近的元素，然后用户将会自然而然地焦点的元素上。 通过移动的 UI 元素靠近 z 轴中，你可以建立对象，帮助用户在应用中自然、 高效地完成任务之间的视觉层次结构。 

![在内容菜单的 z 深度](images/elevation-shadow/whyelevation.svg)

除了提供有意义的视觉层次结构，z 深度还允许你创建流动的体验无缝地从 2D 到 3D 环境，跨所有设备和外形规格缩放你的应用。 

![2d 到 3d 中的 z 深度](images/elevation-shadow/elevation-2d3d.svg)

### <a name="how-is-z-depth-perceived"></a>我们如何 z 深度，所以认为？

根据我们如何感知现实世界中的深度，下面是可用于显示邻近感应数字 UI 中的多种技术。

- **缩放**更大的对象看起来小于仔细对象的大小相同。 这是方法很难演示有效地在 2D 空间中，因此通常不建议。 但是，你可以使用缩放和[阴影](#what-is-shadow)创建的对象将并拢在 2D 用户有效模拟。

    ![邻近感应与缩放](images/elevation-shadow/elevation-scale.svg)

- **氛围**对象会很远的物体和不带有"烟雾"覆盖或其他大气效果的焦点。

    ![使用氛围邻近感应](images/elevation-shadow/elevation-atmosphere.svg)

- **运动**可以使用相对速度来演示邻近感应： 近的对象比远处的后台对象更快地移动。 若要了解如何实现此效果，请参阅[视差](../motion/parallax.md)。

    ![使用运动的邻近感应](images/elevation-shadow/elevation-motion.svg)

### <a name="recommendations-for-z-depth"></a>Z 深度的建议

减少提升平面提供清晰的视觉焦点的数量。 对于大多数方案，两个平面就足够： 一个用于前台项目 （高邻近感应），另一个用于后台项目 （低邻近感应）。 如果你有多个上没有重叠的提升的项目时，将它们组合同一平面 （即，前景） 来减少平面的数量。

![在应用内的 z 深度](images/elevation-shadow/app-depth.svg)

## <a name="what-is-shadow"></a>什么是卷影？

![shadow](images/elevation-shadow/shadow.svg)

阴影是一种方法识别提升。 当存在时提升对象上方的光时，阴影上没有下面的图面。 更高版本的对象、 更大和柔和变得阴影。 请注意，无需具有阴影提升的对象但阴影执行指示提升。

在 UWP 应用中，阴影应该是有意、 不美观。 如果阴影有损焦点和工作效率，然后限制使用的阴影。

你可以使用 ThemeShadow 或 DropShadow Api 使用阴影。

## <a name="themeshadow"></a>ThemeShadow

可以将类型应用于任何 XAML 元素绘制 ThemeShadow 阴影相应地根据的 x、 y、 z 坐标。 对于其他环境规范还会自动调整 ThemeShadow:

- 适应照明、 用户主题、 应用环境和 shell 中的更改。
- 阴影自动基于其提升权限的元素。
- 保持元素同步，因为它们移动和更改提升。
- 确保阴影保持一致，在整个以及跨应用程序。

下面是在使用浅色和深色主题的不同提升 ThemeShadow 的示例：

![智能阴影浅色主题](images/elevation-shadow/smartshadow-light.svg)

![带有深色主题的智能阴影](images/elevation-shadow/smartshadow-dark.svg)

### <a name="themeshadow-in-common-controls"></a>ThemeShadow 共同点控件

下面的常用控件自动将使用 ThemeShadow 投影：

- [对话框和浮出控件](../controls-and-patterns/dialogs.md)
- [NavigationView](../controls-and-patterns/navigationview.md)
- [媒体传输控件](../controls-and-patterns/media-playback.md)
- [上下文菜单](../controls-and-patterns/menus.md)
- [命令栏](../controls-and-patterns/app-bars.md)
- [自动建议](../controls-and-patterns/auto-suggest-box.md)，[组合框](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox)，[日历日期/时间选取器](../controls-and-patterns/date-and-time.md)，[工具提示](../controls-and-patterns/tooltips.md)
- [访问键](../input/access-keys.md)

### <a name="themeshadow-in-popups"></a>弹出窗口中的 ThemeShadow

ThemeShadow 自动投影时应用中的任何 XAML 元素[弹出窗口](/uwp/api/windows.ui.xaml.controls.primitives.popup)。 它将转换应用背景背后的内容其和其下的任何其他打开弹出窗口上的阴影。

若要使用弹出窗口 ThemeShadow，请使用`Shadow`属性将 ThemeShadow 应用到 XAML 元素。 然后，该元素从其他元素隐藏它，例如使用提升的 z 分量`Translation`属性。
对于大多数弹出窗口 UI，推荐的默认提升相对应用背景内容是 32 有效像素为单位。

此示例显示了一个矩形弹出窗口中投影到应用背景内容以及它后面的任何其他弹出窗口：

```xaml
<Popup>
    <Rectangle x:Name="PopupRectangle" Fill="White" Height="48" Width="96">
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

![阴影相对于代码示例](images/elevation-shadow/smartshadow-example.svg)

### <a name="themeshadow-in-other-elements"></a>在其他元素的 ThemeShadow

转换不在弹出窗口的 XAML 元素从阴影，必须显式指定其他元素可以接收阴影中的`ThemeShadow.Receivers`集合。

此示例演示了投影到隐藏它们网格的两个按钮：

```xaml
<Grid x:Name="BackgroundGrid" Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.Resources>
        <ThemeShadow x:Name="SharedShadow" />
    </Grid.Resources>

    <Button x:Name="Button1" Content="Button 1" Shadow="{StaticResource SharedShadow}" Margin="10" />

    <Button x:Name="Button2" Content="Button 2" Shadow="{StaticResource SharedShadow}" Margin="120" />
</Grid>
```

```csharp
/// Add BackgroundGrid as a shadow receiver and elevate the casting buttons above it
SharedShadow.Receivers.Add(BackgroundGrid);

Button1.Translation += new Vector3(0, 0, 16);
Button2.Translation += new Vector3(0, 0, 32);
```

### <a name="performance-best-practices-for-themeshadow"></a>ThemeShadow 性能最佳做法

1. 限制为小必要的自定义接收器元素的数量。 

2. 如果多个接收器元素位于相同提升，然后尝试将其合并通过改为面向一个父元素。

3. 如果多个元素将转换的相同类型的相同的接收器元素上的卷影然后作为共享资源添加阴影并重复使用它。

## <a name="drop-shadow"></a>投影

DropShadow 不是自动响应其环境，并且不会使用光源。 有关示例实现，请参阅[DropShadow 类](https://docs.microsoft.com/uwp/api/windows.ui.composition.dropshadow)。

## <a name="which-shadow-should-i-use"></a>应使用哪个阴影？

| 属性 | ThemeShadow | DropShadow |
| - | - | - | - |
| **最小值 SDK** | RS5 | 14393 |
| **适应性** | 是 | 否 |
| **自定义项** | 否 | 是 |
| **光源** | 自动 (全局默认情况下，但每个应用可以重写) | 无 |
| **在 3D 环境中受支持** | 是 | 否 |

- 通常情况下，我们建议使用 ThemeShadow，自动适应其环境。
- 如果你具有更高级的自定义阴影方案，然后使用 DropShadow，允许进行更高版本的自定义。
- 有关向后兼容性，使用 DropShadow。
- 有关性能问题，限制的阴影数或使用 DropShadow。
- 在 HMDs true 3D 中，使用 ThemeShadow。 由于 DropShadow 指定偏移量绘制视觉对象它在一侧，从父级，从它看起来具有浮动空间中。 另一方面，ThemeShadow 顶部定义为接收者的视觉对象呈现。
