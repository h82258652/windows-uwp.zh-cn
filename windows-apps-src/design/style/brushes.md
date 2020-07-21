---
ms.assetid: 02141F86-355E-4046-86EA-2A89D615B7DB
title: 使用画笔
description: Brush 对象用于绘制形状、文本和控件各个部分的内部或轮廓，以便所绘制的对象在 UI 中可见。
ms.date: 04/28/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 350b07e96d95b043116f2eb8029a2352360c50d6
ms.sourcegitcommit: 490b563462853f10f14825f2358e4852ee1011fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/01/2020
ms.locfileid: "82633003"
---
# <a name="using-brushes-to-paint-backgrounds-foregrounds-and-outlines"></a>使用画笔绘制背景、前景和轮廓

使用 [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) 对象来绘制 XAML 形状、文本和控件的内部和轮廓，使其在应用程序 UI 中可见。

> **重要的 API**：[Brush 类](/uwp/api/Windows.UI.Xaml.Media.Brush)

## <a name="introduction-to-brushes"></a>画笔简介

若要绘制在应用画布上显示的[**形状**](/uwp/api/Windows.UI.Xaml.Shapes.Shape)、文本或[**控件**](/uwp/api/Windows.UI.Xaml.Controls.Control)的部分，请将该形状  的 [**Fill**](/uwp/api/windows.ui.xaml.shapes.shape.fill) 属性或控件  的 [**Background**](/uwp/api/windows.ui.xaml.controls.control.background) 和 [**Foreground**](/uwp/api/windows.ui.xaml.controls.control.foreground) 属性设置为 **Brush** 值。

画笔类型多样，包括： 
-   [**AcrylicBrush**](/uwp/api/windows.ui.xaml.media.acrylicbrush)
-   [**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush)
-   [**LinearGradientBrush**](/uwp/api/Windows.UI.Xaml.Media.LinearGradientBrush) 
-   [**RadialGradientBrush**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush) 
-   [**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush)
-   [**WebViewBrush**](/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush)
-   [**XamlCompositionBrushBase**](/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase)

## <a name="solid-color-brushes"></a>纯色画笔

[SolidColorBrush](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) 使用单一的 [Color](/uwp/api/Windows.UI.Color)（如红色或蓝色）来绘制区域   。 这是最基本的画笔。 在 XAML 中，可通过以下三种方式定义 **SolidColorBrush** 及其指定的颜色：预定义颜色名称、十六进制颜色值或者属性元素语法。

### <a name="predefined-color-names"></a>预定义颜色名称

可以使用预定义的颜色名称（如 [Yellow](/uwp/api/windows.ui.colors.yellow) 或 [Magenta](/uwp/api/windows.ui.colors.magenta)）   。 共有 256 种已命名的颜色。 XAML 解析器会将颜色名称转换为具有正确颜色通道的 [Color](/uwp/api/Windows.UI.Color) 结构  。 这 256 种已命名的颜色基于级联样式表 3 层 (CSS3) 规范中的 X11 颜色名称，因此，如果你以前有过 Web 开发或设计经验，则可能已经熟悉这个已命名颜色的列表  。

下面是将 [Rectangle](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) 的 [Fill](/uwp/api/windows.ui.xaml.shapes.shape.fill) 属性设置为预定义颜色 [Red](/uwp/api/windows.ui.colors.red) 的示例    。

```xaml
<Rectangle Width="100" Height="100" Fill="Red" />
```

![呈现的 SolidColorBrush](images/brushes-solidcolorbrush.jpg)

 应用于矩形的 SolidColorBrush

如果 [SolidColorBrush](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) 是使用 XAML 以外的代码定义的，则每个已命名的颜色都作为 [Colors](https://docs.microsoft.com/uwp/api/windows.ui.colors) 类的静态属性值提供   。 例如，若要声明 SolidColorBrush 的 [Color](/uwp/api/windows.ui.xaml.media.solidcolorbrush.color) 值以表示命名颜色“兰花紫”，请将 Color 值设置为静态值 [Colors.Orchid](/uwp/api/windows.ui.colors.orchid)     。

### <a name="hexadecimal-color-values"></a>十六进制颜色值

可以使用十六进制格式字符串来声明精确的 24 位颜色值，其中有 8 位 alpha 通道用于 [SolidColorBrush](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush)  。 从 0 到 F 这一范围中的两个字符定义每个组成部分的值，而且十六进制字符串中各个组成部分的值顺序为：alpha 通道（不透明度）、红色通道、绿色通道和蓝色通道 (ARGB  )。 例如，十六进制值“\#FFFF0000”定义完全不透明的红色（alpha=“FF”，红色=“FF”，绿色=“00”，蓝色=“00”）。

下面的 XAML 示例将 [Rectangle](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) 的 [Fill](/uwp/api/windows.ui.xaml.shapes.shape.fill) 属性设置为十六进制值“\#FFFF0000”，其结果与使用命名颜色 [Colors.Red](/uwp/api/windows.ui.colors.red) 时相同    。

```xml
<StackPanel>
  <Rectangle Width="100" Height="100" Fill="#FFFF0000" />
</StackPanel>
```

### <a name="property-element-syntax"></a>属性元素语法

你可以使用属性元素语法来定义 [SolidColorBrush](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush)  。 此语法比前面的方法更详细，但是，你可以针对元素指定其他属性值（如 [Opacity](/uwp/api/windows.ui.xaml.media.brush.opacity)）  。 有关 XAML 语法（包括属性元素语法）的详细信息，请参阅 [XAML 概述](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-overview)和 [XAML 语法指南](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-syntax-guide)。

在前面的示例中，要创建的画笔以隐含方式作为专门编写的 XAML 语言速记的一部分自动创建，该速记有助于使 UI 定义对最常见的用例保持简单。 下一个示例将创建一个 [Rectangle](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) 并明确创建 [SolidColorBrush](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) 以作为 [Rectangle.Fill](/uwp/api/windows.ui.xaml.shapes.shape.fill) 属性的元素值    。 SolidColorBrush 的 [Color](/uwp/api/windows.ui.xaml.media.solidcolorbrush.color) 设置为 [Blue](/uwp/api/windows.ui.colors.blue)，[Opacity](/uwp/api/windows.ui.xaml.media.brush.opacity) 设置为 0.5     。

```xml
<Rectangle Width="200" Height="150">
    <Rectangle.Fill>
        <SolidColorBrush Color="Blue" Opacity="0.5" />
    </Rectangle.Fill>
</Rectangle>
```

## <a name="linear-gradient-brushes"></a>线性渐变画笔

[LinearGradientBrush](/uwp/api/Windows.UI.Xaml.Media.LinearGradientBrush) 使用沿着直线定义的渐变绘制一个区域  。 这条直线称为“渐变轴”  。 你可以使用 [GradientStop](/uwp/api/Windows.UI.Xaml.Media.GradientStop) 对象沿着渐变轴来指定渐变颜色及其位置  。 默认情况下，渐变轴从画笔绘制区域的左上角到右下角，这会产生对角底纹。

[GradientStop](/uwp/api/Windows.UI.Xaml.Media.GradientStop) 是渐变画笔的基本构建块  。 渐变停点指定在向绘制区域应用画笔时，画笔的哪个 [Color](/uwp/api/windows.ui.xaml.media.gradientstop.color) 位于渐变轴上的 [Offset](/uwp/api/windows.ui.xaml.media.gradientstop.offset) 处   。

渐变停点的 [Color](/uwp/api/windows.ui.xaml.media.gradientstop.color) 属性指定渐变停点的颜色  。 你可以使用预定义颜色名称或指定十六进制的 ARGB 值来设置颜色  。

[GradientStop](/uwp/api/Windows.UI.Xaml.Media.GradientStop) 的 [Offset](/uwp/api/windows.ui.xaml.media.gradientstop.offset) 属性指定每个 GradientStop 在渐变轴上的位置    。 Offset 是 double 型参数，范围为 0 到 1   。 如果 Offset 为 0，则会将 GradientStop 放在渐变轴的起点，也就是说靠近 [StartPoint](/uwp/api/windows.ui.xaml.media.lineargradientbrush.startpoint) 的位置    。 如果 Offset 为 1，则会将 GradientStop 放在 [EndPoint](/uwp/api/windows.ui.xaml.media.lineargradientbrush.endpoint) 处    。 有用的 [LinearGradientBrush](/uwp/api/Windows.UI.Xaml.Media.LinearGradientBrush) 至少应当有两个 GradientStop 值，每个 GradientStop 都应当指定一个不同的[Color](/uwp/api/windows.ui.xaml.media.gradientstop.color)，而且具有一个不同的 Offset（范围是从 0 到 1）      。

下面的示例使用四种颜色创建了一种线性渐变效果，并用它来绘制 [Rectangle](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle)  。

```xml
<!-- This rectangle is painted with a diagonal linear gradient. -->
<Rectangle Width="200" Height="100">
    <Rectangle.Fill>
        <LinearGradientBrush StartPoint="0,0" EndPoint="1,1">
            <GradientStop Color="Yellow" Offset="0.0" x:Name="GradientStop1"/>
            <GradientStop Color="Red" Offset="0.25" x:Name="GradientStop2"/>
            <GradientStop Color="Blue" Offset="0.75" x:Name="GradientStop3"/>
            <GradientStop Color="LimeGreen" Offset="1.0" x:Name="GradientStop4"/>
        </LinearGradientBrush>
    </Rectangle.Fill>
</Rectangle>
```

渐变停点之间每个点的颜色均以两个边界渐变停点指定的颜色组合呈线性相互融合。 下图突出显示了上述示例中的渐变停点。 圆圈标出了渐变停点的位置，虚线显示的是渐变轴。

![渐变停点](images/linear-gradients-stops.png)

*由两个边界渐变停点指定的颜色组合*

可以通过将 [**StartPoint**](/uwp/api/windows.ui.xaml.media.lineargradientbrush.startpoint) 和 [**EndPoint**](/uwp/api/windows.ui.xaml.media.lineargradientbrush.endpoint) 属性设置为不同于 `(0,0)` 和 `(1,1)` 起始默认值的值，来更改渐变停点所在的线。 通过更改 StartPoint 和 EndPoint 坐标值，可以创建水平或垂直渐变，颠倒渐变方向，或者加快渐变速度以便应用于比整个绘制区域小的范围   。 若要加快渐变，必须将 StartPoint 和/或 EndPoint 的值设置为 0 到 1 之间的值   。 例如，如果你需要一个水平渐变，该渐变所有的淡化都发生在画笔的左半部分，画笔的右侧是纯色且与上一个 [GradientStop](/uwp/api/Windows.UI.Xaml.Media.GradientStop) 颜色相同，请将 StartPoint 指定为 `(0,0)`，将 EndPoint 指定为 `(0.5,0)`   。

### <a name="use-tools-to-make-gradients"></a>使用工具创建渐变

既然你知道了线性渐变的工作原理，你可以使用 Visual Studio 或 Blend 简化创建这些渐变的工作。 若要创建一个渐变，请在设计图面或 XAML 视图中选择要应用渐变的对象。 展开“笔画”并选择“线性渐变”选项卡   。

![在 Visual Studio 中创建线性渐变](images/tool-gradient-brush-1.png)

*在 Visual Studio 中创建线性渐变*

现在你可以更改渐变停点的颜色，并使用底部的栏滑动其位置。 也可以通过单击该栏添加新的渐变停点，并通过将停点拖离该栏来删除它们（请参阅下一张屏幕截图）。

![控制渐变停点的属性窗口的底部的栏](images/tool-gradient-brush-2.png)

*渐变设置滑块*

## <a name="radial-gradient-brushes"></a>径向渐变画笔

[**RadialGradientBrush**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush) 在椭圆内绘制，该椭圆由 [**Center**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.center)、[**RadiusX**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.radiusx) 和 [**RadiusY**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.radiusy) 属性定义。 渐变的颜色开始于椭圆的中心之处，结束于半径之处。

径向渐变的颜色由添加到 [**GradientStops**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.gradientstops) 集合属性的颜色停点定义。 每个渐变停点指定一个颜色和一个沿渐变方向的偏移量。

渐变原点默认为中心，可以使用 [**GradientOrigin**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.gradientorigin) 属性进行偏移。

[MappingMode](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.mappingmode) 定义 [**Center**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.center)、[**RadiusX**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.radiusx)、[**RadiusY**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.radiusy) 和 [**GradientOrigin**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.gradientorigin) 是表示相对坐标还是表示绝对坐标。

当 [**MappingMode**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.mappingmode) 设置为 `RelativeToBoundingBox` 时，这三个属性的 X 和 Y 值会被视为元素边界的相对值，其中 `(0,0)` 和 `(1,1)` 分别表示 [**Center**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.center)、[**RadiusX**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.radiusx) 和 [**RadiusY**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.radiusy) 属性的元素边界的左上方和右下方，`(0,0)` 表示 [**GradientOrigin**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.gradientorigin) 属性的中心。

当 [**MappingMode**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.mappingmode) 设置为 `Absolute` 时，这三个属性的 X 和 Y 值将被视为元素边界内的绝对坐标。

下面的示例使用四种颜色创建了一种线性渐变效果，并用它来绘制 [Rectangle](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle)  。

```xml
<!-- This rectangle is painted with a radial gradient. -->
<Rectangle Width="200" Height="200">
    <Rectangle.Fill>
        <media:RadialGradientBrush>
            <GradientStop Color="Blue" Offset="0.0" />
            <GradientStop Color="Yellow" Offset="0.2" />
            <GradientStop Color="LimeGreen" Offset="0.4" />
            <GradientStop Color="LightBlue" Offset="0.6" />
            <GradientStop Color="Blue" Offset="0.8" />
            <GradientStop Color="LightGray" Offset="1" />
        </media:RadialGradientBrush>
    </Rectangle.Fill>
</Rectangle>
```

渐变停点之间每个点的颜色均以两个边界渐变停点指定的颜色组合沿径向相互融合。 下图突出显示了上述示例中的渐变停点。 

![渐变停点](images/radial-gradient.png)

渐变停点 

## <a name="image-brushes"></a>图像画笔

[ImageBrush](/uwp/api/Windows.UI.Xaml.Media.ImageBrush) 绘制一个包含一个图像的区域，要绘制的图像来自图像文件源  。 你可以使用要加载的图像的路径来设置 [ImageSource](/uwp/api/Windows.UI.Xaml.Media.ImageSource) 属性  。 通常，图像源来自 Content 项目，该项目是应用资源的一部分  。

在默认情况下，[ImageBrush](/uwp/api/Windows.UI.Xaml.Media.ImageBrush) 会拉伸图像以完全填满绘制的区域，如果绘制区域的纵横比与图像的纵横比不同，这可能会使图像失真  。 你可以更改此行为，方式是将 [Stretch](/uwp/api/windows.ui.xaml.media.tilebrush.stretch) 属性更改为其默认值 Fill 以外的值并将它设置为 None、Uniform 或 UniformToFill      。

下一个示例将创建一个 [ImageBrush](/uwp/api/Windows.UI.Xaml.Media.ImageBrush) 并将 [ImageSource](/uwp/api/Windows.UI.Xaml.Media.ImageSource) 设置为名为“licorice.jpg”的图像，必须将该图像作为资源包括在你的应用中   。 ImageBrush 随后会绘制由 [Ellipse](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse) 形状定义的区域   。

```xml
<Ellipse Height="200" Width="300">
   <Ellipse.Fill>
     <ImageBrush ImageSource="licorice.jpg" />
   </Ellipse.Fill>
</Ellipse>
```

![显示的 ImageBrush。](images/brushes-imagebrush.jpg)

呈现的 ImageBrush 

[ImageBrush](/uwp/api/Windows.UI.Xaml.Media.ImageBrush) 和 [Image](/uwp/api/Windows.UI.Xaml.Controls.Image) 均按统一资源标识符 (URI) 引用图像源文件，该图像源文件使用多种可能的图像格式   。 这些图像源文件指定为 URI。 有关指定图像源、可用的图像格式并将它们打包到一个应用中的详细信息，请参阅 [Image 和 ImageBrush](https://docs.microsoft.com/windows/uwp/controls-and-patterns/images-imagebrushes)。

## <a name="brushes-and-text"></a>画笔和文本

还可以使用画布向文本元素应用呈现特征。 例如，[TextBlock](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) 的 [Foreground](/uwp/api/windows.ui.xaml.controls.textblock.foreground) 属性接受 [Brush](/uwp/api/Windows.UI.Xaml.Media.Brush)    。 你可以向文本应用此处描述的任何画笔。 但是，请小心使用应用于文本的画笔，因为如果使用出血到背景中的画笔，任何背景都可能会使文本无法阅读。 在多数情况下，除非你希望文本元素主要用于装饰，否则使用 [SolidColorBrush](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) 可以实现文本元素的可读性  。

甚至在使用纯色时，也要确保你选择的文本颜色相对于文本布局容器的背景颜色具有足够大的对比度。 为了实现可读性，需要考虑文本前景和文本容器背景之间的对比度级别。

## <a name="webviewbrush"></a>WebViewBrush

[WebViewBrush](/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush) 是一种特殊类型的画笔，可以访问通常在 [WebView](/uwp/api/Windows.UI.Xaml.Controls.WebView) 控件中查看的内容   。 WebViewBrush 将内容绘制到呈现图面的另一个具有 [Brush](/uwp/api/Windows.UI.Xaml.Media.Brush) 型属性的元素上，而不是在矩形 WebView 控件区域中绘制内容    。 WebViewBrush 并非对于所有的画笔方案都适合，但它对于切换 WebView 非常有用   。 有关详细信息，请参阅 [**WebViewBrush**](/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush)。

## <a name="xamlcompositionbrushbase"></a>XamlCompositionBrushBase

[XamlCompositionBrushBase](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase) 是一个基类，用于创建使用 [CompositionBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBrush) 绘制 XAML UI 元素的自定义画笔   。

这支持 Windows.UI.Xaml 和 Windows.UI.Composition 层之间的“下拉”互操作，如[可视化层概述](/windows/uwp/composition/visual-layer)中所述  。 

要创建自定义画笔，请创建一个继承自 XamlCompositionBrushBase 的新类，并实现所需方法。

例如，这可以用于使用 [CompositionEffectBrush](/uwp/api/Windows.UI.Composition.CompositionEffectBrush) 将[效果](/uwp/composition/composition-effects)应用到 XAML UIElement，如在由 [XamlLight](/uwp/api/windows.ui.xaml.media.xamllight) 照明时控制 XAML UIElement 的反射属性的“GaussianBlurEffect”或 [SceneLightingEffect](/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect)      。

有关代码示例，请参阅 [**XamlCompositionBrushBase**](/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase)。

## <a name="brushes-as-xaml-resources"></a>画笔作为 XAML 资源

你可以将任何画笔声明为 XAML 资源字典中的键控 XAML 资源。 这便于复制已经应用到 UI 中多个元素的画笔值。 这些画笔值随后将共享并应用到以 XAML 中的 [{StaticResource}](/uwp/xaml-platform/staticresource-markup-extension) 用法形式引用画笔资源的任何情况。 这包括你拥有一个引用共享画笔的 XAML 控件模板以及控件模板本身是键控 XAML 资源的情况。

## <a name="brushes-in-code"></a>代码中的画笔

比起使用代码来定义画笔来说，使用 XAML 指定画笔是更典型的行为。 这是因为画笔通常被定义为 XAML 资源，并且由于画笔值通常是设计工具的输出或者作为 XAML UI 定义的一部分。 然而，对于你可能希望使用代码定义画笔的很少情况，所有 [Brush](/uwp/api/Windows.UI.Xaml.Media.Brush) 类型均可用于代码实例化  。

若要使用代码创建 [SolidColorBrush](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush)，请使用采用 [Color](/uwp/api/Windows.UI.Color) 参数的构造函数   。 传递作为 [Colors](/uwp/api/windows.ui.colors) 类的静态属性的值，如下所示  ：

```cs
SolidColorBrush blueBrush = new SolidColorBrush(Windows.UI.Colors.Blue);
```

```vb
Dim blueBrush as SolidColorBrush = New SolidColorBrush(Windows.UI.Colors.Blue)
```

```cppwinrt
Windows::UI::Xaml::Media::SolidColorBrush blueBrush{ Windows::UI::Colors::Blue() };
```

```cpp
blueBrush = ref new SolidColorBrush(Windows::UI::Colors::Blue);
```

对于 [WebViewBrush](/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush) 和 [ImageBrush](/uwp/api/Windows.UI.Xaml.Media.ImageBrush)，请使用默认的构造函数，然后在尝试使用用于 UI 属性的该画笔前调用其他 API   。

-   使用代码定义 [ImageBrush](/uwp/api/Windows.UI.Xaml.Media.ImageBrush) 时，[ImageSource](/uwp/api/windows.ui.xaml.media.imagebrush.imagesourceproperty) 需要 [BitmapImage](/uwp/api/Windows.UI.Xaml.Media.Imaging.BitmapImage)（而非 URI）    。 如果源是一个流，请使用 [SetSourceAsync](/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsourceasync) 方法来初始化该值  。 如果源是一个 URI（其中包含应用中使用 ms-appx 或 ms-resource 方案的内容），请使用获取 URI 的 [BitmapImage](/uwp/api/windows.ui.xaml.media.imaging.bitmapimage) 构造函数    。 如果在检索或解码图像资源时存在任何计时问题，而你可能在图像资源可用前需要使用替代内容用以显示，则还可以考虑处理 [ImageOpened](/uwp/api/windows.ui.xaml.media.imagebrush.imageopened) 事件  。
-   对于 [WebViewBrush](/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush)，如果你最近已重设 [SourceName](/uwp/api/windows.ui.xaml.controls.webviewbrush.sourcename) 属性或者如果 [WebView](/uwp/api/Windows.UI.Xaml.Controls.WebView) 的内容也随代码更改，则可能需要调用 [Redraw](/uwp/api/windows.ui.xaml.controls.webviewbrush.redraw)     。

有关代码示例，请参阅 [**WebViewBrush**](/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush)、[**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush) 和 [**XamlCompositionBrushBase**](/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase)。
 

 




