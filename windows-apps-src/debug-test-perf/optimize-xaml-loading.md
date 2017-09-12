---
author: jwmsft
ms.assetid: 569E8C27-FA01-41D8-80B9-1E3E637D5B99
title: "优化 XAML 标记"
description: "在内存中分析构造对象的 XAML 标记对于复杂的 UI 而言非常耗时。 你可以通过以下措施为你的应用缩短 XAML 标记分析和加载时间及提高内存效率。"
ms.author: jimwalk
ms.date: 08/10/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 007b86717c4bfbbcdc711aa63a7bb45dd06a1132
ms.sourcegitcommit: ec18e10f750f3f59fbca2f6a41bf1892072c3692
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/14/2017
---
# <a name="optimize-your-xaml-markup"></a>优化 XAML 标记

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

在内存中分析构造对象的 XAML 标记对于复杂的 UI 而言非常耗时。 你可以采取以下措施为你的应用缩短 XAML 标记分析和加载时间并提高内存效率。

在应用启动时进行限制，即仅加载初始 UI 所需的 XAML 标记。 检查初始页面（包括页面资源）中的标记，并确认你未加载不立即需要的额外元素。 这些元素可以来自各种来源，如资源字典、最初折叠的元素和在其他元素上绘制的元素。

为了提高效率而优化 XAML 需要进行权衡；并非总是有一个单一的解决方案适用于所有情况。 在这里，我们将查看一些常见的问题并指导你如何为应用进行正确的权衡。

## <a name="minimize-element-count"></a>最大程度减少元素计数

尽管 XAML 平台能够显示大量元素，但你可以使用最少数量的元素实现所需的视觉效果，从而使应用布局设置和呈现速度更快。

你在决定如何布局 UI 控件时所做的选择将影响在启用应用时创建的 UI 元素的数量。 有关优化布局的详细信息，请参阅[优化你的 XAML 布局](optimize-your-xaml-layout.md)。

元素计数在数据模板中非常重要，因为每个元素都对每个数据项再次创建。 有关如何减少列表或网格中的元素计数的信息，请参阅 [ListView 和 GridView UI 优化](optimize-gridview-and-listview.md) 文章中的*按项目减少元素*。

在这里，我们来了解你还通过其他哪些途径减少在启用应用时必须加载的元素数量。

### <a name="defer-item-creation"></a>延迟创建项目

如果你的 XAML 标记包含不立即显示的元素，可以延迟加载这些元素，直到它们显示出来。 例如，可以延迟非可见内容，如类似于选项卡的 UI 上的辅助选项卡。 或者，你可以默认在网格视图中显示项目，但应提供选项，以支持用户改为在列表中查看数据。 你可以延迟到需要时加载列表。

使用 [x:Load 属性](../xaml-platform/x-load-attribute.md) 而不是 [Visibility](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_Visibility) 属性控制显示元素的时间。 当元素的可见性设置为 **Collapsed** 时，在呈现传递过程中会被跳过，但是仍会在内存中付出对象实例成本。 改用 x:Load 时，框架不会在需要它之前创建对象实例，因此内存成本会更低。 缺点是在未加载 UI 时需付出较小的内存开销（大约 600 字节）。

> [!NOTE]
> 可以通过使用 [x:Load](../xaml-platform/x-load-attribute.md) 或 [x:DeferLoadStrategy](../xaml-platform/x-deferloadstrategy-attribute.md) 属性来延迟加载元素。 从 Windows 10 创意者更新（版本 1703，SDK 版本 15063）开始，可使用 x:Load 属性。 要使用 x:Load，Visual Studio 项目所面向的最低版本必须为 *Windows 10 创意者更新（10.0，版本 15063）*。 若要面向较早的版本，请使用 x: DeferLoadStrategy。

下面的示例显示在使用不同的技术来隐藏 UI 元素时，元素计数和内存使用上的差异。 ListView 和 GridView 包含放置在页面根网格中的相同项目。 ListView 不可见，但会显示 GridView。 以上每个示例中的 XAML 在屏幕上产生相同的 UI。 我们将使用 Visual Studio [用于分析和性能的工具](tools-for-profiling-and-performance.md)来查看元素计数和内存使用情况。

#### <a name="option-1---inefficient"></a>选项 1 - 低效

在这里，加载 ListView，但由于它的宽度为 0，因此不可见。 ListView 及其每个子元素均在可视化树中创建并加载到内存。

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <ListView x:Name="List1" Width="0">
        <ListViewItem>Item 1</ListViewItem>
        <ListViewItem>Item 2</ListViewItem>
        <ListViewItem>Item 3</ListViewItem>
        <ListViewItem>Item 4</ListViewItem>
        <ListViewItem>Item 5</ListViewItem>
        <ListViewItem>Item 6</ListViewItem>
        <ListViewItem>Item 7</ListViewItem>
        <ListViewItem>Item 8</ListViewItem>
        <ListViewItem>Item 9</ListViewItem>
        <ListViewItem>Item 10</ListViewItem>
    </ListView>

    <GridView x:Name="Grid1">
        <GridViewItem>Item 1</GridViewItem>
        <GridViewItem>Item 2</GridViewItem>
        <GridViewItem>Item 3</GridViewItem>
        <GridViewItem>Item 4</GridViewItem>
        <GridViewItem>Item 5</GridViewItem>
        <GridViewItem>Item 6</GridViewItem>
        <GridViewItem>Item 7</GridViewItem>
        <GridViewItem>Item 8</GridViewItem>
        <GridViewItem>Item 9</GridViewItem>
        <GridViewItem>Item 10</GridViewItem>
    </GridView>
</Grid>
```

加载 ListView 的实时可视化树。 页面的总元素计数是 89。

![带有列表视图的可视化树](images/visual-tree-1.png)

ListView 及其子元素加载到内存。

![带有列表视图的可视化树](images/memory-use-1.png)

#### <a name="option-2---better"></a>选项 2 - 更好

在这里，ListView 的可见性已设置为折叠（其他 XAML 与原来的相同）。 ListView 在可视化树中创建，但其子元素不是。 但是，它们将加载到内存，以便内存使用与上面的示例相同。

```xaml
    <ListView x:Name="List1" Visibility="Collapsed">
```

折叠 ListView 的实时可视化树。 页面的总元素计数是 46。

![带有折叠的列表视图的可视化树](images/visual-tree-2.png)

ListView 及其子元素加载到内存。

![带有列表视图的可视化树](images/memory-use-1.png)

#### <a name="option-3---most-efficient"></a>选项 3 - 最高效

在这里，ListView 的 x:Load 属性设置为 **False**（其他 XAML 与原来的相同）。 在启动时不在可视化树中创建 ListView，也不加载到内存。

```xaml
    <ListView x:Name="List1" Visibility="Collapsed" x:Load="False">
```

不加载 ListView 的实时可视化树。 页面的总元素计数是 45。

![不加载列表视图的可视化树。](images/visual-tree-3.png)

ListView 及其子元素不加载到内存。

![带有列表视图的可视化树](images/memory-use-3.png)

> [!NOTE]
> 这些示例中的元素计数和内存使用量都很小，且仅为了展示概念才进行显示。 在这些示例中，使用 x:Load 的开销大于节约的内存，因此应用不会获益。 应使用应用上的分析工具来确定你的应用是否会从延迟加载中受益。

### <a name="use-layout-panel-properties"></a>使用布局面板属性

布局面板具有 [Background](https://msdn.microsoft.com/library/windows/apps/BR227512) 属性，因此无需只是为了将面板着色而将 [Rectangle](https://msdn.microsoft.com/library/windows/apps/BR243371) 置于 Panel 前。

**低效**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<Grid>
    <Rectangle Fill="Black"/>
</Grid>
```

**高效**

```xaml
<Grid Background="Black"/>
```

布局面板还有内置边框属性，以便你无需将[边框](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.border) 元素放在布局面板周围。 有关详细信息和示例，请参阅[优化 XAML 布局](optimize-your-xaml-layout.md)。

### <a name="use-images-in-place-of-vector-based-elements"></a>使用图像代替基于矢量的元素

如果重用同一基于矢量的元素的次数足够多，则改为使用[图像](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image) 元素将会更加高效。 基于矢量的元素更加耗费资源，因为 CPU 必须分别创建每个单独的元素。 图像文件仅需要解码一次。

## <a name="optimize-resources-and-resource-dictionaries"></a>优化资源和资源字典

你通常使用[资源字典](../controls-and-patterns/resourcedictionary-and-xaml-resource-references.md)将你要引用的几乎全局级别的资源存储在应用中的多个位置。 例如，样式、画笔、模板等等。

一般情况下，我们优化了 [ResourceDictionary](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary)，以便不实例化资源（除非另有要求）。 但你应避免对资源进行不必要的实例化的情况。

### <a name="resources-with-xname"></a>具有 x:Name 的资源

使用 [x:Key 属性](../xaml-platform/x-key-attribute.md) 引用资源。 具有 [x:Name 属性](../xaml-platform/x-name-attribute.md)的任何资源都不会受益于平台优化；相反，只要创建 ResourceDictionary，就会立即对其进行实例化。 出现这种情况是因为 x:Name 告知平台你的应用需要对此资源的字段访问权限，因此平台需要创建某些内容来创建相关引用。

### <a name="resourcedictionary-in-a-usercontrol"></a>UserControl 中的 ResourceDictionary

在 [UserControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.usercontrol) 内部定义的 ResourceDictionary 会产生负面影响。 平台将为 UserControl 的每个实例创建此类 ResourceDictionary 的副本。 如果你有经常使用的 UserControl，那么请将ResourceDictionary 移出 UserControl，并将其放在页面级别。

### <a name="resource-and-resourcedictionary-scope"></a>资源和 ResourceDictionary 范围

如果页面引用在其他文件中定义的用户控件或资源，则框架还会分析该文件。

在这里，因为 _InitialPage.xaml_ 使用了 _ExampleResourceDictionary.xaml_ 中的一个资源，因此在启动时必须分析整个 _ExampleResourceDictionary.xaml_。

**InitialPage.xaml。**

```xaml
<Page x:Class="ExampleNamespace.InitialPage" ...>
    <Page.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <ResourceDictionary Source="ExampleResourceDictionary.xaml"/>
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </Page.Resources>

    <Grid>
        <TextBox Foreground="{StaticResource TextBrush}"/>
    </Grid>
</Page>
```

**ExampleResourceDictionary.xaml。**

```xaml
<ResourceDictionary>
    <SolidColorBrush x:Key="TextBrush" Color="#FF3F42CC"/>

    <!--This ResourceDictionary contains many other resources that
        are used in the app, but are not needed during startup.-->
</ResourceDictionary>
```

如果在应用中的许多页面上都使用了某个资源，将其存储在 _App.xaml_ 中是一个很好的方法，并且可以避免重复。 但 _App.xaml_ 将在启动时进行分析，因此任何仅在一个页面（除非该页面是初始页面）中使用的资源都应放置于页面的本地资源中。 此示例演示的 _App.xaml_ 包含仅由一个页面（非初始页面）使用的资源。 这会不必要地增加应用启动时间。

**App.xaml**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<Application ...>
     <Application.Resources>
        <SolidColorBrush x:Key="DefaultAppTextBrush" Color="#FF3F42CC"/>
        <SolidColorBrush x:Key="InitialPageTextBrush" Color="#FF3F42CC"/>
        <SolidColorBrush x:Key="SecondPageTextBrush" Color="#FF3F42CC"/>
        <SolidColorBrush x:Key="ThirdPageTextBrush" Color="#FF3F42CC"/>
    </Application.Resources>
</Application>
```

**InitialPage.xaml。**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<Page x:Class="ExampleNamespace.InitialPage" ...>
    <StackPanel>
        <TextBox Foreground="{StaticResource InitialPageTextBrush}"/>
    </StackPanel>
</Page>
```

**SecondPage.xaml。**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<Page x:Class="ExampleNamespace.SecondPage" ...>
    <StackPanel>
        <Button Content="Submit" Foreground="{StaticResource SecondPageTextBrush}" />
    </StackPanel>
</Page>
```

若要使此示例更有效率，可以将 `SecondPageTextBrush` 移动到 _SecondPage.xaml_ 中，然后将 `ThirdPageTextBrush` 移动到 _ThirdPage.xaml_ 中。 `InitialPageTextBrush` 可以保留在 _App.xaml_ 中，因为在任何情况下，应用程序资源都必须在应用启动时进行分析。

### <a name="consolidate-multiple-brushes-that-look-the-same-into-one-resource"></a>将看起来相同的多个画笔整合到一个资源中

XAML 平台将尝试缓存常用对象，这样可以尽可能经常地重用这些对象。 但是，XAML 难以判断在一部分标记中声明的画笔是否与在另一部分的标记中声明的画笔相同。 此处的示例使用 [SolidColorBrush](https://msdn.microsoft.com/library/windows/apps/BR242962) 进行演示，但使用 [GradientBrush](https://msdn.microsoft.com/library/windows/apps/BR210068) 的情况可能性更大也更为重要。 还需检查使用预定义颜色的画笔；例如：`"Orange"` 和 `"#FFFFA500"` 是同一颜色。

**低效。**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<Page ... >
    <StackPanel>
        <TextBlock>
            <TextBlock.Foreground>
                <SolidColorBrush Color="#FFFFA500"/>
            </TextBlock.Foreground>
        </TextBox>
        <Button Content="Submit">
            <Button.Foreground>
                <SolidColorBrush Color="#FFFFA500"/>
            </Button.Foreground>
        </Button>
    </StackPanel>
</Page>
```

若要解决重复问题，请将画笔定义为资源。 如果其他页面中的控件使用了同一画笔，请将该画笔移动到 _App.xaml_ 中。

**高效。**

```xaml
<Page ... >
    <Page.Resources>
        <SolidColorBrush x:Key="BrandBrush" Color="#FFFFA500"/>
    </Page.Resources>

    <StackPanel>
        <TextBlock Foreground="{StaticResource BrandBrush}" />
        <Button Content="Submit" Foreground="{StaticResource BrandBrush}" />
    </StackPanel>
</Page>
```

## <a name="minimize-overdrawing"></a>尽量减少过度绘制

过度绘制是指采用同一屏幕像素绘制多个对象。 请注意，有时本指南与最大程度减少元素数目的需求之间会存在权衡关系。

将 [**DebugSettings.IsOverdrawHeatMapEnabled**](https://msdn.microsoft.com/library/windows/apps/Hh701823) 用作视觉诊断。 你可能会在场景中发现你未注意到的要进行绘制的对象。

### <a name="transparent-or-hidden-elements"></a>透明或隐藏元素

如果元素由于透明或隐藏在其他元素之后的原因而不可见，并且它不会对布局产生影响，请将其删除。 如果元素在初始的视觉状态中不可见，但在其他视觉状态中可见，请使用 x:Load 控制其状态或将元素本身的 [Visibility](https://msdn.microsoft.com/library/windows/apps/BR208992) 设置为 **Collapsed**，并将该值更改为相应状态中的 **Visible**。 此类启发式方法存在例外情况：通常情况下，主要的视觉状态中属性所具有的值最好在元素上本地设置。

### <a name="composite-elements"></a>复合元素

使用合成元素，而不是通过将多个元素分层来创建某个效果。 在此示例中，结果是双色的形状，上半部分是黑色（来自 [Grid](https://msdn.microsoft.com/library/windows/apps/BR242704) 的背景），而下半部分是灰色（来自 **Grid** 黑色背景上的半透明白色 [Rectangle](https://msdn.microsoft.com/library/windows/apps/BR243371) Alpha 混合）。 此处填充了实现效果所需的 150% 的像素。

**低效。**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<Grid Background="Black">
    <Grid.RowDefinitions>
        <RowDefinition Height="*"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <Rectangle Grid.Row="1" Fill="White" Opacity=".5"/>
</Grid>
```

**高效。**

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="*"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <Rectangle Fill="Black"/>
    <Rectangle Grid.Row="1" Fill="#FF7F7F7F"/>
</Grid>
```

### <a name="layout-panels"></a>布局面板

布局面板可以有两个目的：为区域着色，以及设置子元素的布局。 如果 Z 顺序中较为后面的元素已经为区域着色，则前面的布局面板无需再绘制该区域：它可以只专注于设置其子元素的布局。 下面提供了一个示例。

**低效。**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<GridView Background="Blue">
    <GridView.ItemTemplate>
        <DataTemplate>
            <Grid Background="Blue"/>
        </DataTemplate>
    </GridView.ItemTemplate>
</GridView>
```

**高效。**

```xaml
<GridView Background="Blue">
    <GridView.ItemTemplate>
        <DataTemplate>
            <Grid/>
        </DataTemplate>
    </GridView.ItemTemplate>
</GridView>
```

如果 [Grid](https://msdn.microsoft.com/library/windows/apps/BR242704) 必须进行点击测试，请对它设置透明背景值。

### <a name="borders"></a>边框

使用 [Border](https://msdn.microsoft.com/library/windows/apps/BR209253) 元素绘制对象周围的边框。 在此示例中，[Grid](https://msdn.microsoft.com/library/windows/apps/BR242704) 用作 [TextBox](https://msdn.microsoft.com/library/windows/apps/BR209683) 周围的临时边框。 但中心单元的所有像素要进行过度绘制。

**低效。**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<Grid Background="Blue" Width="300" Height="45">
    <Grid.RowDefinitions>
        <RowDefinition Height="5"/>
        <RowDefinition/>
        <RowDefinition Height="5"/>
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="5"/>
        <ColumnDefinition/>
        <ColumnDefinition Width="5"/>
    </Grid.ColumnDefinitions>
    <TextBox Grid.Row="1" Grid.Column="1"></TextBox>
</Grid>
```

**高效。**

```xaml
 <Border BorderBrush="Blue" BorderThickness="5" Width="300" Height="45">
     <TextBox/>
</Border>
```

### <a name="margins"></a>边距

请注意边距。 如果负边距扩展到另一方的呈现边界并引起过度绘制，则两个相邻元素将（可能意外）重叠。

### <a name="cache-static-content"></a>缓存静态内容

过度绘制的另一个来源是由许多重叠元素形成的形状。 如果针对包含合成形状的 [UIElement](https://msdn.microsoft.com/library/windows/apps/BR208911)，将 [CacheMode](https://msdn.microsoft.com/library/windows/apps/BR228084) 设置为 **BitmapCache**，平台会将该元素作为位图呈现一次，然后每帧使用该位图而不是过度绘制。

**低效。**

```xaml
<Canvas Background="White">
    <Ellipse Height="40" Width="40" Fill="Blue"/>
    <Ellipse Canvas.Left="21" Height="40" Width="40" Fill="Blue"/>
    <Ellipse Canvas.Top="13" Canvas.Left="10" Height="40" Width="40" Fill="Blue"/>
</Canvas>
```

![包含三个实心圆的维恩图](images/solidvenn.png)

上面的图像是结果，而此处为过度绘制区域的示意图。 红色越深，指示过度绘制程度越高。

![显示重叠区域的维恩图](images/translucentvenn.png)

**高效。**

```xaml
<Canvas Background="White" CacheMode="BitmapCache">
    <Ellipse Height="40" Width="40" Fill="Blue"/>
    <Ellipse Canvas.Left="21" Height="40" Width="40" Fill="Blue"/>
    <Ellipse Canvas.Top="13" Canvas.Left="10" Height="40" Width="40" Fill="Blue"/>
</Canvas>
```

请注意 [CacheMode](https://msdn.microsoft.com/library/windows/apps/BR228084) 的使用。 如果任意子形状具有动画效果，请不要使用此技术，因为可能需要在每一帧重新生成位图缓存，这违背了原本目的。

## <a name="use-xbf2"></a>使用 XBF2

XBF2 是在运行时避免所有文本分析成本的 XAML 标记的二进制表示形式。 它还为加载和树创建优化了你的二进制文件，并允许适用于 XAML 类型的“快速路径”改进堆和对象创建成本，例如 VSM、ResourceDictionary、Styles 等。 由于它完全由内存映射，因此没有可用于加载和读取 XAML 页面的堆占用。 此外，它可以减少以 appx 格式存储的 XAML 页面的磁盘占用。 XBF2 是更紧凑的表示形式，并且它可以减少多达 50%的比较 XAML/XBF1 文件的磁盘占用。 例如，在转换为 XBF2 后，内置的“照片”应用会减少大约 60%，即从约 ~1MB 的 XBF1 资源下降到约 ~400KB 的 XBF2 资源。 我们还看到了应用在 CPU 方面受益了 15% 到 20%，在 Win32 堆方面受益了 10% 到 15%。

框架提供的 XAML 内置控件和字典已完全支持 XBF2。 对于你自己的应用，请确保你的项目文件可声明 TargetPlatformVersion 8.2 或更高版本。

若要检查你是否具有 XBF2，请在二进制编辑器中打开你的应用；如果有 XBF2，则第 12 个和第 13 个字节为 00 02。

## <a name="related-articles"></a>相关文章

- [应用启动性能的最佳实践](best-practices-for-your-app-s-startup-performance.md)
- [优化 XAML 布局](optimize-your-xaml-layout.md)
- [ListView 和 GridView UI 优化](optimize-gridview-and-listview.md)
- [用于分析和性能的工具](tools-for-profiling-and-performance.md)