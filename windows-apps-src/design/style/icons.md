---
Description: 良好的图标会与版式和设计语言的其余部分相协调。 它们不会混合隐喻，并且会尽快且尽量简单地仅传达所需内容。
title: 图标
ms.assetid: b90ac02d-5467-4304-99bd-292d6272a014
label: Icons
template: detail.hbs
ms.date: 05/02/2018
ms.topic: article
keywords: windows 10, uwp
design-contact: Judysa
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: fb0101597a38553f56579913a9c40502fcdbf051
ms.sourcegitcommit: 1d6d05d28358e087d9ee8829d76c5fbbac0225cb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/17/2020
ms.locfileid: "79434224"
---
# <a name="icons-for-uwp-apps"></a>适用于 UWP 应用的图标

![图标标题图像](images/icons/header-icons.png)

图标提供操作、概念或产品的视觉速记。 通过将含义压缩到符号图像中，图标可以跨越语言障碍，帮助节省非常宝贵的资源：屏幕空间。 

图标可以显示在应用中，也可以显示在应用外： 

:::row:::
    :::column:::
        应用中的图标 

        ![应用中的图标](images/icons/inside-icons.png)应用内，使用图标表示复制文本或导航到设置页等动作。
    :::column-end:::
    :::column:::
应用外的图标 

        ![应用外的图标](images/icons/outside-icons.jpg)应用外，Windows 在开始菜单和任务栏中使用图标表示应用。 如果用户选择将应用固定到开始菜单，则应用的开始磁贴可以显示应用图标。 应用图标在标题栏中显示，你可以选择创建带有应用徽标的闪屏。
    :::column-end:::
:::row-end:::

本文介绍应用内的图标。 若要了解应用外的图标（应用图标），请参阅[应用和磁贴图标文章](/windows/uwp/design/shell/tiles-and-notifications/app-assets)。

## <a name="when-to-use-icons"></a>何时使用图标

图标可以节省空间，但应在何时使用？ 

:::row:::
    :::column:::
        ![应做事项](images/do.svg)![图标标准图像](images/icons/icons-standard.svg)<br>

将图标用于剪切、复制、粘贴和保存等操作，或用于导航菜单中的导航项。
    :::column-end:::
    :::column:::
        ![禁止事项](images/dont.svg)![图标概念图像](images/icons/icons-concept.svg)<br>

如果要表示的概念已具有图标，则使用图标。 （若要查看图标是否存在，检查 Segoe 图标列表。）
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![应做事项](images/do.svg)![图标购物车](images/icons/icon-shopping-cart.svg)<br>

如果用户可以轻松地理解图标的含义，且以小尺寸表达含义足够简单，则使用图标。
    :::column-end:::
    :::column:::
        ![禁做事项](images/dont.svg)![图标概念图像](images/icons/icon-bad-example.png)<br>

如果图标的含义不清晰，或者需要复杂的外形才能清晰表达，则不要使用图标。
    :::column-end:::
:::row-end:::



## <a name="using-the-right-type-of-icon"></a>使用正确的图标类型

可通过多种方式创建图标。 可以使用 Segoe MDL2 Assets 这样的符号字体。 可以创建自己的矢量图。 甚至可以使用位图，不过我们不建议这样做。 下面简述了向应用添加图标的不同方法。 

### <a name="use-a-predefined-icon"></a>使用预定义的图标。
:::row:::
    :::column:::
Microsoft 提供 1000 多个 Segoe MDL2 Assets 字体格式的图标。 从字体获取图标可能不直观，但我们的字体显示技术意味着这些图标在任何显示器、任何分辨率、任何尺寸下都能够有简洁、清晰的外观。 有关说明，请参阅 [Segoe MDL2 图标](segoe-ui-symbol-font.md)。
    :::column-end:::
    :::column:::
        ![预定义的图标图像](images/icons/predefined-icon.png)
    :::column-end:::
:::row-end:::

### <a name="use-a-font"></a>使用字体。
:::row:::
    :::column:::
不一定要使用 Segoe MDL2 Assets 字体，可以使用用户在其系统上安装的任何字体，如 Wingdings 或 Webdings。
    :::column-end:::
    :::column:::
        ![wingdings 图像](images/icons/wingdings.png)
    :::column-end:::
:::row-end:::

### <a name="use-a-scalable-vector-graphics-svg-file"></a>使用可缩放矢量图形 (SVG) 文件。
:::row:::
    :::column:::
SVG 资源非常适合作为图标，因为它们在任何尺寸或分辨率下看起来都很清晰。 大多数绘图应用程序都可以导出为 SVG。 有关说明，请参阅 [SVGImageSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.svgimagesource)。
    :::column-end:::
    :::column:::
        ![SVG 图像](images/icons/icon-scale.gif)
    :::column-end:::
:::row-end:::

### <a name="use-geometry-objects"></a>使用几何图形对象。
:::row:::
    :::column:::
与 SVG 文件一样，几何图形也是一种基于矢量的资源，所以看起来始终很清晰。 不过，创建几何图形比较复杂，因为必须单独指定每个点和曲线。 如果需要在应用运行时修改图标（以便对其进行动画处理等），它确实是很好的选择。 有关说明，请参阅[用于移动和绘制几何图形的命令](../../xaml-platform/move-draw-commands-syntax.md)。 
    :::column-end:::
    :::column:::
        ![几何图形对象图像](images/icons/geometry-objects.png)
    :::column-end:::
:::row-end:::

### <a name="you-can-also-use-a-bitmap-image-such-as-png-gif-or-jpeg-although-we-dont-recommend-it"></a>也可以使用位图（如 PNG、GIF 或 JPEG），不过我们不建议这样做。
:::row:::
    :::column:::
位图以特定尺寸创建，因此它们必须根据你需要的图标大小和屏幕分辨率放大或缩小。 当图像缩小（收缩）时，它可能显示得比较模糊；当放大时，它可能显示为像素颗粒。 如果必须使用位图，建议使用 PNG 或 GIF 而不是 JPEG。 
    :::column-end:::
    :::column:::
        ![禁止事项](images/dont.svg)![位图图像](images/icons/bitmap-image.png)
    :::column-end:::
:::row-end:::

## <a name="make-the-icon-do-something"></a>让图标发挥作用

有了图标后，下一步是通过将图标与命令或导航操作关联来让它发挥作用。 实现此目的的最佳方法是将图标添加到按钮或命令栏。 

![命令栏图像](images/icons/app-bar-desktop.svg)

## <a name="create-an-icon-button"></a>创建图标按钮

可将图标放到标准按钮中。 因为可以在更广泛的位置使用按钮，所以可以更灵活地选择操作图标显示位置。 

可通过几种方式将图标添加到按钮：

:::row:::
    :::column span="2":::
        <b>步骤 1</b><br>
将按钮的字体系列设置为 `Segoe MDL2 Assets`，并将其内容属性设置为要使用的字形的 Unicode 值：
    :::column-end:::
    :::column:::
        ![创建图标按钮步骤 1](images/icons/create-icon-step-1.svg)
    :::column-end:::
:::row-end:::

```xaml 
<Button FontFamily="Segoe MDL2 Assets" Content="&#xE102;" />
```

:::row:::
    :::column span="2":::
        <b>步骤 2</b><br>
可使用以下图标元素对象之一：[BitmapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.bitmapicon)、[FontIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.fonticon)、[PathIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pathicon) 或 [SymbolIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.symbolicon)。 这提供了更多图标类型供你选择，并让你能够根据需要将图标和其他内容类型（如文本）组合起来：
    :::column-end:::
    :::column:::
        ![创建图标按钮步骤 2](images/icons/icon-text-step-2.svg)
    :::column-end:::
:::row-end:::

```xaml 
<Button>
    <StackPanel>
        <SymbolIcon Symbol="Play" />
        <TextBlock>Play the movie</TextBlock>
    </StackPanel>
</Button>
```

## <a name="create-a-series-of-icons-in-a-command-bar"></a>在命令栏中创建一系列图标

:::row:::
    :::column span:::
当你有一系列要组合使用的命令（如剪切/复制/粘贴）或一组照片编辑程序的绘图命令时，可以在[命令栏](../controls-and-patterns/app-bars.md)上将它们放在一起。 命令栏采用一个或多个应用栏按钮或应用栏切换按钮，每个按钮表示一项操作。 每个按钮都有一个 [Icon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton#Windows_UI_Xaml_Controls_AppBarButton_Icon) 属性，可用于控制要显示的图标。 可以通过多种方式指定图标。 
    :::column-end:::
    :::column:::
        ![带有图标的命令栏示例](images/icons/create-icon-command-bar.svg)
    :::column-end:::
:::row-end:::

最简单的方法是使用我们提供的预定义图标列表 - 只需指定图标名称，如“返回”或“停止”，系统便可以进行绘制： 

``` xaml
<CommandBar>
    <AppBarToggleButton Icon="Shuffle" Label="Shuffle" Click="AppBarButton_Click" />
    <AppBarToggleButton Icon="RepeatAll" Label="Repeat" Click="AppBarButton_Click"/>
    <AppBarSeparator/>
    <AppBarButton Icon="Back" Label="Back" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Stop" Label="Stop" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Play" Label="Play" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Forward" Label="Forward" Click="AppBarButton_Click"/>
</CommandBar>

```
要获得图标名称的完整列表，请参阅 [Symbol 枚举](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.symbol)。 

还可通过其他方式为命令栏中的按钮提供图标：

+ [FontIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.fonticon) - 图标基于指定的字体系列的字形。
+ [BitmapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.bitmapicon) - 图标基于具有指定 URI 的位图文件  。
+ [PathIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pathicon) - 图标基于[路径](/uwp/api/windows.ui.xaml.shapes.path)数据。

若要了解有关命令栏的详细信息，请参阅[命令栏文章](../controls-and-patterns/app-bars.md)。 



## <a name="related-articles"></a>相关文章

* [应用图标和徽标](app-icons-and-logos.md)
