---
Description: 良好的图标会与版式和设计语言的其余部分相协调。 它们不会混合隐喻，并且会尽快且尽量简单地仅交流所需内容。
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
ms.openlocfilehash: 5e464251200812e79474d05d9d0a680b49167871
ms.sourcegitcommit: 7da28cf4f4e8390bc9a21a9488b03af39271cbbe
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2019
ms.locfileid: "64564545"
---
# <a name="icons-for-uwp-apps"></a>适用于 UWP 应用的图标

![图标标题图像](images/icons/header-icons.png)

图标提供操作、概念或产品的视觉速记。 通过将含义压缩到符号图像中，图标可以跨越语言障碍，帮助节省非常宝贵的资源：屏幕空间。 

图标可以显示在应用中，也可以显示在应用外： 

:::row:::
    :::column:::
        **Icons inside the app**

        ![icons inside the app](images/icons/inside-icons.png)
在您的应用程序，您可以使用图标来表示操作，例如将文本复制或导航到设置页。
    :::column-end:::
    :::column:::
**此应用外部的图标**

        ![icons outside the app](images/icons/outside-icons.jpg)
外部应用程序，Windows 使用图标来表示您在开始菜单和任务栏中的应用程序。 如果用户选择将应用固定到开始菜单，您的应用程序启动磁贴可以功能应用程序的图标。 应用程序的图标将出现在标题栏，您可以选择创建初始屏幕的应用程序的徽标。
    :::column-end:::
:::row-end:::

本文介绍应用内的图标。 若要了解应用外的图标（应用图标），请参阅[应用和磁贴图标文章](/windows/uwp/design/shell/tiles-and-notifications/app-assets)。

## <a name="when-to-use-icons"></a>何时使用图标

图标可以节省空间，但应在何时使用？ 

:::row:::
    :::column:::
        ![do](images/do.svg)
        ![icons standard image](images/icons/icons-standard.svg)<br>

使用执行操作，如剪切、 复制、 粘贴、 和保存或导航菜单中的导航项的图标。
    :::column-end:::
    :::column:::
        ![don't](images/dont.svg)
        ![icons concept image](images/icons/icons-concept.svg)<br>

如果你想要表示的概念已有，使用一个图标。 （若要查看是否存在一个图标，检查 Segoe 图标列表。）
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![do](images/do.svg)
        ![icon shopping cart](images/icons/icon-shopping-cart.svg)<br>

如果是便于用户了解图标的含义，并且很轻松地清除在小尺寸为，使用一个图标。
    :::column-end:::
    :::column:::
        ![dont](images/dont.svg)
        ![icons concept image](images/icons/icon-bad-example.png)<br>

不要使用一个图标，如果其含义不是很清晰，或者使用户清楚地知道需要复杂的形状。
    :::column-end:::
:::row-end:::



## <a name="using-the-right-type-of-icon"></a>使用正确的图标类型

可通过多种方式创建图标。 你可以使用诸如 Segoe MDL2 Assets 这样的符号字体。 您可以创建您自己的基于矢量的图像。 你甚至可以使用位图图像，不过我们不建议这样做。 下面是可以将图标添加到应用的不同方法的摘要。 

### <a name="use-a-predefined-icon"></a>使用预定义的图标。
:::row:::
    :::column:::
Microsoft 提供了 Segoe MDL2 资产字体的窗体中的 1000 个以上图标。 从字体获取图标可能不直观，但我们的字体显示技术意味着这些图标在任何显示、任何分辨率、任何尺寸下都能够有简洁、清晰的外观。 有关说明，请参阅[Segoe MDL2 图标](segoe-ui-symbol-font.md)。
    :::column-end:::
    :::column:::
        ![pre-defined icon image](images/icons/predefined-icon.png)
    :::column-end:::
:::row-end:::

### <a name="use-a-font"></a>使用字体。
:::row:::
    :::column:::
无需使用 Segoe MDL2 资产字体，即可以使用任何用户已安装在其系统上，如 Wingdings 或 Webdings 字体。
    :::column-end:::
    :::column:::
        ![wingdings image](images/icons/wingdings.png)
    :::column-end:::
:::row-end:::

### <a name="use-a-scalable-vector-graphics-svg-file"></a>使用可缩放的向量图形 (SVG) 文件。
:::row:::
    :::column:::
SVG 资源非常适用于图标，因为它们始终显示在任意大小或分辨率清晰。 大多数绘图应用程序都可以导出到 SVG。 有关说明，请参阅[SVGImageSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.svgimagesource)。
    :::column-end:::
    :::column:::
        ![SVG image](images/icons/icon-scale.gif)
    :::column-end:::
:::row-end:::

### <a name="use-geometry-objects"></a>使用几何图形对象。
:::row:::
    :::column:::
SVG 像文件一样，几何图形是基于矢量的资源，因此它们始终看起来清晰。 不过，创建几何图形比较复杂，因为必须单独指定每个点和曲线。 如果你需要在应用运行时修改图标（例如，动画处理），这确实是唯一的好选择。 有关说明，请参阅[移动和绘制几何图形的命令](../../xaml-platform/move-draw-commands-syntax.md)。 
    :::column-end:::
    :::column:::
        ![Geometry objects image](images/icons/geometry-objects.png)
    :::column-end:::
:::row-end:::

### <a name="you-can-also-use-a-bitmap-image-such-as-png-gif-or-jpeg-although-we-dont-recommend-it"></a>你也可以使用位图图像（如 PNG、GIF 或 JPEG），不过我们不建议这样做。
:::row:::
    :::column:::
位图图像创建特定大小，因此他们需要进行增加或减少根据大你想要的图标和屏幕的分辨率。 当图像缩小（收缩）时，它可能显示得比较模糊；当放大时，它可能显示得斑驳且像素化。 如果你必须使用位图图像，建议使用 PNG 或 GIF 而不是 JPEG。 
    :::column-end:::
    :::column:::
        ![don't](images/dont.svg)
        ![Bitmap image](images/icons/bitmap-image.png)
    :::column-end:::
:::row-end:::

## <a name="make-the-icon-do-something"></a>让图标发挥作用

有一个图标下, 一步是以使其执行某些操作通过将它与命令或导航操作相关联。 若要执行此操作的最佳方法是将图标添加到按钮或命令栏。 

![命令栏图像](images/icons/app-bar-desktop.svg)

## <a name="create-an-icon-button"></a>创建图标按钮

你可以在标准按钮中放置一个图标。 因为你可以在更广泛的位置使用按钮，这让你在选择操作图标显示位置时有了更多一些的灵活性。 

将图标添加到按钮有几种方法：

:::row:::
    :::column span="2":::
        <b>Step 1</b><br>
将按钮的字体系列设置为`Segoe MDL2 Assets`，其内容属性设为你想要使用的标志符号的 unicode 值：
    :::column-end:::
    :::column:::
        ![Create an icon button step 1](images/icons/create-icon-step-1.svg)
    :::column-end:::
:::row-end:::

```xaml 
<Button FontFamily="Segoe MDL2 Assets" Content="&#xE102;" />
```

:::row:::
    :::column span="2":::
        <b>Step 2</b><br>
可以使用图标元素对象之一：[BitmapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.bitmapicon)， [FontIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.fonticon)， [PathIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pathicon)，或[SymbolIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.symbolicon)。 这使您更多类型的图标可供选择，并使您可以将图标和其他类型的内容，例如文本，如果你想要：
    :::column-end:::
    :::column:::
        ![Create an icon button step 2](images/icons/icon-text-step-2.svg)
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
当具有一系列合在一起，将命令，例如剪切/复制/粘贴或一系列绘图命令的照片编辑程序，将其放在一起在[命令栏](../controls-and-patterns/app-bars.md)。 命令栏采用一个或多个应用栏按钮或应用栏切换按钮，每个按钮表示一项操作。 每个按钮有一个[图标](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.appbarbutton#Windows_UI_Xaml_Controls_AppBarButton_Icon)属性，你用它来控制要显示哪个图标。 可以通过多种方式指定图标。 
    :::column-end:::
    :::column:::
        ![Example of a command bar with icons](images/icons/create-icon-command-bar.svg)
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

为命令栏中的按钮提供图标还有其他一些方法：

+ [FontIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.fonticon) - 此图标基于指定的字体系列的字形。
+ [BitmapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.bitmapicon) - 此图标基于具有指定 **Uri** 的位图图像文件。
+ [PathIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pathicon) - 此图标基于[路径](/uwp/api/windows.ui.xaml.shapes.path)数据。

若要了解有关命令栏的详细信息，请参阅[命令栏文章](../controls-and-patterns/app-bars.md)。 



## <a name="related-articles"></a>相关文章

* [磁贴和图标资产的指导原则](../shell/tiles-and-notifications/app-assets.md)
