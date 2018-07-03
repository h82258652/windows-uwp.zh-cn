---
author: mijacobs
Description: Good icons harmonize with typography and with the rest of the design language. They don’t mix metaphors, and they communicate only what’s needed, as speedily and simply as possible.
title: 图标
ms.assetid: b90ac02d-5467-4304-99bd-292d6272a014
label: Icons
template: detail.hbs
ms.author: mijacobs
ms.date: 05/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
design-contact: Judysa
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 077967c37f76c8f1d0942f365344de65db13b041
ms.sourcegitcommit: ce45a2bc5ca6794e97d188166172f58590e2e434
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2018
ms.locfileid: "1983570"
---
# <a name="icons-for-uwp-apps"></a>适用于 UWP 应用的图标

![图标标题图像](images/icons/header-icons.png)

图标提供操作、概念或产品的视觉速记。 通过将含义压缩到符号图像中，图标可以跨越语言障碍，帮助节省非常宝贵的资源：屏幕空间。 

图标可以显示在应用中，也可以显示在应用外： 

:::行::: :::列::: **应用内的图标**

        ![icons inside the app](images/icons/inside-icons.png)
        Inside your app, you use icons to represent an action, such as copying text or navigating to the settings page.
    :::column-end:::
    :::column:::
        **Icons outside the app**

        ![icons outside the app](images/icons/outside-icons.jpg)
         Outside your app, Windows uses an icon to represent your app in the start menu and in the taskbar. If the user chooses to pin your app to the start menu, your app's start tile can feature your app's icon. Your app's icon appears in the title bar and you can choose to create a splash screen with your app's logo.
    :::column-end:::
:::行末:::

本文介绍应用内的图标。 若要了解应用外的图标（应用图标），请参阅[应用和磁贴图标文章](/windows/uwp/design/shell/tiles-and-notifications/app-assets)。

## <a name="when-to-use-icons"></a>何时使用图标

图标可以节省空间，但应在何时使用？ 

:::行::: :::列::: ![应做事项](images/do.svg) ![标准图标图像](images/icons/icons-standard.svg)<br>

        Use an icon for actions, like cut, copy, paste, and save, or for navigation items in a navigation menu.
    :::column-end:::
    :::column:::
        ![don't](images/dont.svg)
        ![icons concept image](images/icons/icons-concept.svg)<br>

        Use an icon if one already exists for the concept you want to represent. (To see whether an icon exists, check the Segoe icon list.)
    :::column-end:::
:::行末:::

:::行::: :::列::: ![应做事项](images/do.svg) ![购物车图标](images/icons/icon-shopping-cart.svg)<br>

        Use an icon if it's easy for the user to understand what the icon means and it's simple enough to be clear at small sizes.
    :::column-end:::
    :::column:::
        ![dont](images/dont.svg)
        ![icons concept image](images/icons/icon-bad-example.png)<br>

        Don't use an icon if its meaning isn't clear, or if making it clear requires a complex shape.
    :::column-end:::
:::行末:::



## <a name="using-the-right-type-of-icon"></a>使用正确的图标类型

可通过多种方式创建图标。 你可以使用诸如 Segoe MDL2 Assets 这样的符号字体。 你可以创建自己的基于矢量的图像。 你甚至可以使用位图图像，不过我们不建议这样做。 下面是可以将图标添加到应用的不同方法的摘要。 

### <a name="use-a-predefined-icon"></a>使用预定义的图标。
:::行::: :::列::: Microsoft 提供 1000 多个 Segoe MDL2 Assets 字体格式的图标。 从字体获取图标可能不直观，但我们的字体显示技术意味着这些图标在任何显示、任何分辨率、任何尺寸下都能够有简洁、清晰的外观。 :::列末::: :::列::: ![预定义的图标图像](images/icons/predefined-icon.png) :::列末::: :::行末:::

### <a name="use-a-font"></a>使用字体。
:::行::: :::列::: 不必一定使用 Segoe MDL2 Assets 字体 - 你可以使用用户在其系统上安装的任何字体，如 Wingdings 或 Webdings。
:::列末::: :::列::: ![wingdings 图像](images/icons/wingdings.png) :::列末::: :::行末:::

### <a name="use-a-scalable-vector-graphics-svg-file"></a>使用可缩放的向量图形 (SVG) 文件。
::: 行::: :::列::: SVG 资源非常适合图标，因为它们在任何尺寸或分辨率下看起来都很清晰。 大多数绘图应用程序都可以导出到 SVG。 :::列末::: :::列::: ![SVG 图像](images/icons/icon-scale.gif) :::列末::: :::行末:::

### <a name="use-geometry-objects"></a>使用几何图形对象。
:::行::: :::列::: 与 SVG 文件一样，几何图形也是一种基于矢量的资源，所以始终看起来会很清晰。 不过，创建几何图形比较复杂，因为必须单独指定每个点和曲线。 如果你需要在应用运行时修改图标（例如，动画处理），这确实是唯一的好选择。 有关说明，请参阅[移动和绘制几何图形的命令](../../xaml-platform/move-draw-commands-syntax.md)。 :::列末::: :::列::: ![几何图形对象图像](images/icons/geometry-objects.png) :::列末::: :::行末:::

### <a name="you-can-also-use-a-bitmap-image-such-as-png-gif-or-jpeg-although-we-dont-recommend-it"></a>你也可以使用位图图像（如 PNG、GIF 或 JPEG），不过我们不建议这样做。
:::行::: :::列::: 位图图像以特定尺寸创建，因此它们必须根据你需要的图标大小和屏幕分辨率放大或缩小。 当图像缩小（收缩）时，它可能显示得比较模糊；当放大时，它可能显示得斑驳且像素化。 如果你必须使用位图图像，建议使用 PNG 或 GIF 而不是 JPEG。 :::列末::: :::列::: ![禁止事项](images/dont.svg) ![位图图像](images/icons/bitmap-image.png) :::列末::: :::行末:::

## <a name="make-the-icon-do-something"></a>让图标发挥作用

有了图标后，下一步是通过将图标与命令或导航操作关联来让它发挥作用。 实现此目的的最佳方法是将图标添加到按钮或命令栏。 

![命令栏图像](images/icons/app-bar-desktop.svg)

## <a name="create-an-icon-button"></a>创建图标按钮

你可以在标准按钮中放置一个图标。 因为你可以在更广泛的位置使用按钮，这让你在选择操作图标显示位置时有了更多一些的灵活性。 

将图标添加到按钮有几种方法：

:::行::: :::列范围="2"::: <b>步骤 1</b><br>
        将按钮的字体系列设置为 `Segoe MDL2 Assets` 并将其内容属性设置为你想要使用的字形的 unicode 值：:::列末::: :::列::: ![创建图标按钮步骤 1](images/icons/create-icon-step-1.svg) :::列末::: :::行末:::

```xaml 
<Button FontFamily="Segoe MDL2 Assets" Content="&#xE102;" />
```

:::行::: :::列范围="2"::: <b>步骤 2</b><br>
        你可以使用其中一个图标元素对象：[BitmapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.bitmapicon)、[FontIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.fonticon)、[PathIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pathicon) 或 [SymbolIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.symbolicon)。 这为你提供了更多图标类型可供选择，并在你希望执行以下任务时，让你能够将图标和其他内容类型（如文本）组合起来：:::列末::: :::列::: ![创建图标按钮步骤 2](images/icons/icon-text-step-2.svg) :::列末::: :::行末:::

```xaml 
<Button>
    <StackPanel>
        <SymbolIcon Symbol="Play" />
        <TextBlock>Play the movie</TextBlock>
    </StackPanel>
</Button>
```

## <a name="create-a-series-of-icons-in-a-command-bar"></a>在命令栏中创建一系列图标

:::行::: :::列范围::: 当你有一系列要组合使用的命令（如剪切/复制/粘贴）或一组照片编辑程序的绘图命令时，可以在[命令栏](../controls-and-patterns/app-bars.md)上将它们放在一起。 命令栏采用一个或多个应用栏按钮或应用栏切换按钮，每个按钮表示一项操作。 每个按钮有一个[图标](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.appbarbutton#Windows_UI_Xaml_Controls_AppBarButton_Icon)属性，你用它来控制要显示哪个图标。 可以通过多种方式指定图标。 :::列末::: :::列::: ![带图标的命令栏示例](images/icons/create-icon-command-bar.svg) :::列末::: :::行末:::

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

* [磁贴和图标资源指南](../shell/tiles-and-notifications/app-assets.md)
