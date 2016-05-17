---
author: mcleblanc
ms.assetid: 569E8C27-FA01-41D8-80B9-1E3E637D5B99
title: 优化 XAML 标记
description: 在内存中分析构造对象的 XAML 标记对于复杂的 UI 而言非常耗时。 你可以采取以下措施为你的应用提高 XAML 标记分析和加载时间以及内存效率。
---
# 优化 XAML 标记

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

在内存中分析构造对象的 XAML 标记对于复杂的 UI 而言非常耗时。 你可以采取以下措施为你的应用提高 XAML 标记分析和加载时间效率以及内存效率。

在应用启动时进行限制，即仅加载初始 UI 所需的 XAML 标记。 检查初始页面中的标记，确认该页面不包含任何不需要的内容。 如果页面引用在其他文件中定义的用户控件或资源，则框架还会分析该文件。

在此示例中，因为 InitialPage.xaml 使用了 ExampleResourceDictionary.xaml 中的一个资源，因此在启动时必须分析整个 ExampleResourceDictionary.xaml。

**InitialPage.xaml。**

```xml
<Page x:Class="ExampleNamespace.InitialPage" ...> 
    <UserControl.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <ResourceDictionary Source="ExampleResourceDictionary.xaml"/>
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </UserControl.Resources>

    <Grid>
        <TextBox Foreground="{StaticResource TextBrush}"/>
    </Grid>
</Page>
```

**ExampleResourceDictionary.xaml。**

```xml
<ResourceDictionary>
    <SolidColorBrush x:Key="TextBrush" Color="#FF3F42CC"/>

    <!--This ResourceDictionary contains many other resources that
    used in the app, but not during startup.-->
</ResourceDictionary>
```

如果在应用中的许多页面上都使用了某个资源，将其存储在 App.xaml 中是一个很好的方法，并且可以避免重复。 但 App.xaml 将在启动时进行分析，因此任何仅在一个页面（除非该页面是初始页面）中使用的资源都应放置于页面的本地资源中。 此计数器示例演示的 App.xaml 包含仅由一个页面（非初始页面）使用的资源。 这会不必要地增加了应用启动时间。

**InitialPage.xaml。**

```xml
<Page ...>  <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->   
    <StackPanel>  
        <TextBox Foreground="{StaticResource InitialPageTextBrush}"/> 
    </StackPanel> 
</Page> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
```

**SecondPage.xaml。**

```xml
<Page ...>  <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
    <StackPanel> 
        <Button Content="Submit" Foreground="{StaticResource SecondPageTextBrush}" /> 
    </StackPanel> 
</Page> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
```

**App.xaml**

```xml
<Application ...> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
     <Application.Resources>  
        <SolidColorBrush x:Key="DefaultAppTextBrush" Color="#FF3F42CC"/> 
        <SolidColorBrush x:Key="InitialPageTextBrush" Color="#FF3F42CC"/> 
        <SolidColorBrush x:Key="SecondPageTextBrush" Color="#FF3F42CC"/> 
        <SolidColorBrush x:Key="ThirdPageTextBrush" Color="#FF3F42CC"/> 
    </Application.Resources> 
</Application> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
```

若要使上述计数器示例更有效率，可以将 `SecondPageTextBrush` 移动到 SecondPage.xaml 中，然后将 `ThirdPageTextBrush` 移动到 ThirdPage.xaml 中。 `InitialPageTextBrush` 可以保留在 App.xaml 中，因为在任何情况下，应用程序资源都必须在应用启动时进行分析。

## 最大程度减少元素计数

尽管 XAML 平台能够显示大量元素，但你可以使用最少数量的元素实现所需的视觉效果，从而使应用布局设置和呈现速度更快。

-   布局面板具有 [**Background**](https://msdn.microsoft.com/library/windows/apps/BR227512) 属性，因此无需只是为了将面板着色而将 [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371) 置于 Panel 前。

**低效。**

```xml
<Grid> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
        <Rectangle Fill="Black"/> 
    </Grid> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
```

**高效。**

```xml
<Grid Background="Black"/>
```

-   如果重用同一基于矢量的元素的次数足够多，则改为使用 [**Image**](https://msdn.microsoft.com/library/windows/apps/BR242752) 元素将会更加高效。 基于矢量的元素更加耗费资源，因为 CPU 必须分别创建每个单独的元素。 图像文件仅需要解码一次。

## 将看起来相同的多个画笔整合到一个资源中

XAML 平台将尝试缓存常用对象，这样可以尽可能经常地重用这些对象。 但是，XAML 难以判断在一部分标记中声明的画笔是否与在另一部分的标记中声明的画笔相同。 此处的示例使用 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) 进行演示，但使用 [**GradientBrush**](https://msdn.microsoft.com/library/windows/apps/BR210068) 的情况可能性更大也更为重要

**低效。**

```xml
<Page ... > <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
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
</Page> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
```

还需检查使用预定义颜色的画笔：`"Orange"` 和 `"#FFFFA500"` 是同一颜色。 若要解决重复问题，请将画笔定义为资源。 如果 其他页面中的控件使用了同一画笔，请将该画笔移动到 App.xaml 中。

**高效。**

```xml
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

## 尽量减少过度绘制

过度绘制是指采用同一屏幕像素绘制多个对象。 请注意，有时本指南与最大程度减少元素数目的需求之间会存在权衡关系。

-   如果元素由于透明或隐藏在其他元素之后的原因而不可见，并且它不会对布局产生影响，请将其删除。 如果元素在初始的视觉状态中不可见，但在其他视觉状态中可见，请将元素本身的 [**Visibility**](https://msdn.microsoft.com/library/windows/apps/BR208992) 设置为 **Collapsed**，并将该值更改为相应状态中的 **Visible**。 此类启发式方法存在例外情况：通常情况下，主要的视觉状态中属性所具有的值最好在元素上本地设置。
-   使用合成元素，而不是通过将多个元素分层来创建某个效果。 在此示例中，结果是双色的形状，上半部分是黑色（来自 [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704) 的背景），而下半部分是灰色（来自 **Grid** 黑色背景上的半透明白色 [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371) Alpha 混合）。 此处填充了实现效果所需的 150% 的像素。

**低效。**
    
```xml
    <Grid Background="Black"> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
        <Grid.RowDefinitions> 
            <RowDefinition Height="*"/> 
            <RowDefinition Height="*"/> 
        </Grid.RowDefinitions> 
        <Rectangle Grid.Row="1" Fill="White" Opacity=".5"/> 
    </Grid> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
```

**高效。**

```xml
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <Rectangle Fill="Black"/>
        <Rectangle Grid.Row="1" Fill="#FF7F7F7F"/>
    </Grid>
```

-   布局面板可以有两个目的：为区域着色，以及设置子元素的布局。 如果 Z 顺序中较为后面的元素已经为区域着色，则前面的布局面板无需再绘制该区域：它可以只专注于设置其子元素的布局。 下面提供了一个示例。

**低效。**

```xml
    <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
    <GridView Background="Blue">  
        <GridView.ItemTemplate> 
            <DataTemplate> 
                <Grid Background="Blue"/> 
            </DataTemplate> 
        </GridView.ItemTemplate> 
    </GridView> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
```

**高效。**

```xml
    <GridView Background="Blue">  
        <GridView.ItemTemplate> 
            <DataTemplate> 
                <Grid/> 
            </DataTemplate>
        </GridView.ItemTemplate>
    </GridView> 
```

如果 [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704) 必须进行点击测试，请对它设置透明背景值。

-   使用 [**Border**](https://msdn.microsoft.com/library/windows/apps/BR209253) 元素绘制对象周围的边框。 在此示例中，[**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704) 用作 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) 周围的临时边框。 但中心单元的所有像素要进行过度绘制。

**低效。**

```xml
    <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
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
    </Grid> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
```

**高效。**

```xml
    <Border BorderBrush="Blue" BorderThickness="5" Width="300" Height="45">
        <TextBox/>
    </Border>
```

-   请注意边距。 如果负边距扩展到另一方的呈现边界并引起过度绘制，则两个相邻元素将（可能意外）重叠。

将 [**DebugSettings.IsOverdrawHeatMapEnabled**](https://msdn.microsoft.com/library/windows/apps/Hh701823) 用作视觉诊断。 你可能会在场景中发现你未注意到的要进行绘制的对象。

## 缓存静态内容

过度绘制的另一个来源是由许多重叠元素形成的形状。 如果针对包含合成形状的 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911)，将 [**CacheMode**](https://msdn.microsoft.com/library/windows/apps/BR228084) 设置为 **BitmapCache**，平台会将该元素作为位图呈现一次，然后每帧使用该位图而不是过度绘制。

**低效。**

```xml
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

```xml
<Canvas Background="White" CacheMode="BitmapCache">
    <Ellipse Height="40" Width="40" Fill="Blue"/>
    <Ellipse Canvas.Left="21" Height="40" Width="40" Fill="Blue"/>
    <Ellipse Canvas.Top="13" Canvas.Left="10" Height="40" Width="40" Fill="Blue"/>
</Canvas>
```

请注意 [**CacheMode**](https://msdn.microsoft.com/library/windows/apps/BR228084) 的使用。 如果任意子形状具有动画效果，请不要使用此技术，因为可能需要在每一帧重新生成位图缓存，这违背了原本目的。

## ResourceDictionary

ResourceDictionary 通常用于存储几乎全局级别的资源。 你的应用想要在多个位置引用的资源。 例如，样式、画笔、模板等等。 一般情况下，我们优化了 ResourceDictionary，以便不实例化资源（除非另有要求）。 但是有一些你需要注意的地方。

**具有 x:Name 的资源**。 具有 x:Name 的任何资源都不会受益于平台优化，但是只要创建 ResourceDictionary，就会立即对其进行实例化。 之所以出现这种情况是因为 x:Name 会告诉平台你的应用需要对此资源的字段访问权限，因此平台需要创建某些内容来创建相关引用。

**UserControl 中的 ResourceDictionary**。 在 UserControl 内部定义的 ResourceDictionary 会产生负面影响。 平台将为 UserControl 的每个实例创建此类 ResourceDictionary 的副本。 如果你有经常使用的 UserControl，那么请将ResourceDictionary 移出 UserControl，并将其放在页面级别。

## 使用 XBF2

XBF2 是在运行时避免所有文本分析成本的 XAML 标记的二进制表示形式。 它还为加载和树创建优化了你的二进制文件，并允许适用于 XAML 类型的“快速路径”改进堆和对象创建成本，例如 VSM、ResourceDictionary、Styles 等。 由于它完全由内存映射，因此没有可用于加载和读取 XAML 页面的堆占用。 此外，它可以减少以 appx 格式存储的 XAML 页面的磁盘占用。 XBF2 是更紧凑的表示形式，并且它可以减少多达 50%的比较 XAML/XBF1 文件的磁盘占用。 例如，在转换为 XBF2 后，内置的“照片”应用会减少大约 60%，即从约 ~1MB 的 XBF1 资源下降到约 ~400KB 的 XBF2 资源。 我们还看到了应用在 CPU 方面受益了 15% 到 20%，在 Win32 堆方面受益了 10% 到 15%。

框架提供的 XAML 内置控件和字典已完全支持 XBF2。 对于你自己的应用，请确保你的项目文件可声明 TargetPlatformVersion 8.2 或更高版本。

若要检查你是否具有 XBF2，请在二进制编辑器中打开你的应用；如果有 XBF2，则第 12 个和第 13 个字节为 00 02。



<!--HONumber=May16_HO2-->


