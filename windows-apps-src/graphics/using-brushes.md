---
author: Jwmsft
ms.assetid: 02141F86-355E-4046-86EA-2A89D615B7DB
title: "使用画笔"
description: "Brush 对象用于绘制形状、文本和控件各个部分的内部或轮廓，以便所绘制的对象在 UI 中可见。"
translationtype: Human Translation
ms.sourcegitcommit: f5934600cc185c952acc57ae38e0b190466e0dfa
ms.openlocfilehash: dc415135a05a63226a6b2d0b828245fe2f713788

---
# 使用画笔

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**重要的 API**

-   [**Brush**](https://msdn.microsoft.com/library/windows/apps/BR228076)

[**Brush**](https://msdn.microsoft.com/library/windows/apps/BR228076) 对象用于绘制形状、文本和控件各个部分的内部或轮廓，以便所绘制的对象在 UI 中可见。 让我们了解一下可用的画笔以及如何使用画笔。

## 画笔简介

若要绘制在应用画布上显示的对象（如 [**Shape**](https://msdn.microsoft.com/library/windows/apps/BR243377)，或者 [**Control**](https://msdn.microsoft.com/library/windows/apps/BR209390) 的各个部分），可以使用 [**Brush**](https://msdn.microsoft.com/library/windows/apps/BR228076)。 例如，将 **Shape** 的 [**Fill**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.shapes.shape.fill.aspx) 属性或者 **Control** 的 [**Background**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.background.aspx) 和 [**Foreground**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.foreground.aspx) 属性设置为 **Brush** 值，该 **Brush** 确定 UI 元素在 UI 中的绘制或呈现方式。 画笔包括 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962)、[**LinearGradientBrush**](https://msdn.microsoft.com/library/windows/apps/BR210108)、[**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/BR210101) 和 [**WebViewBrush**](https://msdn.microsoft.com/library/windows/apps/BR227703) 这几种不同类型。

## 纯色画笔

[**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) 使用单一的 [**Color**](https://msdn.microsoft.com/library/windows/apps/Hh673723)（如红色或蓝色）来绘制区域。 这是最基本的画笔。 在 XAML 中可通过以下三种方式定义 **SolidColorBrush** 及其指定的纯色：预定义颜色名称、十六进制颜色值或者属性元素语法。

### 预定义颜色名称

可以使用预定义的颜色名称（如 [**Yellow**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.colors.yellow.aspx) 或 [**Magenta**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.colors.magenta.aspx)）。 共有 256 种已命名的颜色。 XAML 解析器会将颜色名称转换为具有正确颜色通道的 [**Color**](https://msdn.microsoft.com/library/windows/apps/Hh673723) 结构。 这 256 种已命名的颜色基于级联样式表 3 层 (CSS3) 规范中的 *X11* 颜色名称，因此，如果你以前有过 Web 开发或设计经验，则可能已经熟悉这个已命名颜色的列表。

下面是将 [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371) 的 [**Fill**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.shapes.shape.fill.aspx) 属性设置为预定义颜色 [**Red**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.colors.red.aspx) 的示例。

```xml
<Rectangle Width="100" Height="100" Fill="Red" />
```

下图显示将 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) 应用于 [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371) 时的情况。

![显示的 SolidColorBrush。](images/brushes-solidcolorbrush.jpg)

如果 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) 是使用 XAML 以外的代码定义的，则每个已命名的颜色都作为 [**Colors**](https://msdn.microsoft.com/library/windows/apps/windows.ui.colors) 类的静态属性值提供。 例如，若要声明 **SolidColorBrush** 的 [**Color**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.solidcolorbrush.color.aspx) 值以表示命名颜色“兰花紫”，请将 **Color** 值设置为静态值 [**Colors.Orchid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.colors.orchid.aspx)。

### 十六进制颜色值

可以使用十六进制格式字符串来声明精确的 24 位颜色值，其中有 8 位 alpha 通道用于 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962)。 从 0 到 F 这一范围中的两个字符定义每个组成部分的值，而且十六进制字符串中各个组成部分的值顺序为：alpha 通道（不透明度）、红色通道、绿色通道和蓝色通道 (**ARGB**)。 例如，十六进制值“\#FFFF0000”定义完全不透明的红色（alpha=“FF”，红色=“FF”，绿色=“00”，蓝色=“00”）。

下面的 XAML 示例将 [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371) 的 [**Fill**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.shapes.shape.fill.aspx) 属性设置为十六进制值“\#FFFF0000”，其结果与使用命名颜色 [**Colors.Red**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.colors.red.aspx) 时相同。

```xml
<StackPanel>
  <Rectangle Width="100" Height="100" Fill="#FFFF0000" />
</StackPanel>
```

### <span id="Property_element_syntax__"></span><span id="property_element_syntax__"></span><span id="PROPERTY_ELEMENT_SYNTAX__"></span>属性元素语法

你可以使用属性元素语法来定义 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962)。 此语法比前面的方法更详细，但是，你可以针对元素指定其他属性值（如 [**Opacity**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.brush.opacity.aspx)）。 有关 XAML 语法的详细信息（包括属性元素语法），请参阅 [XAML 概述](https://msdn.microsoft.com/library/windows/apps/Mt185595)和 [XAML 语法指南](https://msdn.microsoft.com/library/windows/apps/Mt185596)。

在前面的几个示例中，你在语法中甚至从未看到过字符串“SolidColorBrush”。 要创建的画笔是以隐含方式作为有意的 XAML 语言速记的一部分自动创建的，可以帮助使 UI 定义在最常用的情况下简单。 下一个示例将创建一个 [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371) 并明确创建 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) 以作为 [**Rectangle.Fill**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.shapes.shape.fill.aspx) 属性的元素值。 **SolidColorBrush** 的 [**Color**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.solidcolorbrush.color.aspx) 设置为 [**Blue**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.colors.blue.aspx)，[**Opacity**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.brush.opacity.aspx) 设置为 0.5。

```xml
<Rectangle Width="200" Height="150">
    <Rectangle.Fill>
        <SolidColorBrush Color="Blue" Opacity="0.5" />
    </Rectangle.Fill>
</Rectangle>
```

## <span id="Linear_gradient_brushes_"></span><span id="linear_gradient_brushes_"></span><span id="LINEAR_GRADIENT_BRUSHES_"></span>线性渐变画笔

[**LinearGradientBrush**](https://msdn.microsoft.com/library/windows/apps/BR210108) 使用沿着直线定义的渐变绘制一个区域。 这条直线称为*渐变轴*。 你可以使用 [**GradientStop**](https://msdn.microsoft.com/library/windows/apps/BR210078) 对象沿着渐变轴来指定渐变颜色及其位置。 默认情况下，渐变轴从画笔绘制区域的左上角到右下角，这会产生对角底纹。

[**GradientStop**](https://msdn.microsoft.com/library/windows/apps/BR210078) 是渐变画笔的基本构建块。 渐变停点指定在向绘制区域应用画笔时，画笔的哪个 [**Color**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.gradientstop.color.aspx) 位于渐变轴上的 [**Offset**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.gradientstop.offset.aspx) 处。

渐变停点的 [**Color**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.gradientstop.color.aspx) 属性指定渐变停点的颜色。 你可以使用预定义颜色名称或指定十六进制的 **ARGB** 值来设置颜色。

[**GradientStop**](https://msdn.microsoft.com/library/windows/apps/BR210078) 的 [**Offset**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.gradientstop.offset.aspx) 属性指定每个 **GradientStop** 在渐变轴上的位置。 **Offset** 是 **double** 型参数，范围为 0 到 1。 如果 **Offset** 为 0，则会将 **GradientStop** 放在渐变轴的起点，也就是说靠近 [**StartPoint**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.lineargradientbrush.startpoint.aspx) 的位置。 如果 **Offset** 为 1，则会将 **GradientStop** 放在 [**EndPoint**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.lineargradientbrush.endpoint.aspx) 处。 有用的 [**LinearGradientBrush**](https://msdn.microsoft.com/library/windows/apps/BR210108) 至少应当有两个 **GradientStop** 值，每个 **GradientStop** 都应当指定一个不同的[**Color**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.gradientstop.color.aspx)，而且具有一个不同的 **Offset**（范围是从 0 到 1）。

下面的示例使用四种颜色创建了一种线性渐变效果，并用它来绘制 [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371)。

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

渐变停点之间每个点的颜色均以两个边界渐变停点指定的颜色组合呈线性相互融合。 该图突出显示了上述示例中的渐变停点。 圆圈标出了渐变停点的位置，虚线显示的是渐变轴。

![渐变停点](images/linear-gradients-stops.png) 你可以通过将 [**StartPoint**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.lineargradientbrush.startpoint.aspx) 和 [**EndPoint**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.lineargradientbrush.endpoint.aspx) 属性设置为不同于 `(0,0)` 和 `(1,1)` 起始默认值的值，更改渐变停点所在的直线。 通过更改 **StartPoint** 和 **EndPoint** 坐标值，可以创建水平或垂直渐变，颠倒渐变方向，或者加快渐变速度以便应用于比整个绘制区域小的范围。 若要加快渐变，必须将 **StartPoint** 和/或 **EndPoint** 的值设置为 0 到 1 之间的值。 例如，如果你需要一个水平渐变，该渐变所有的淡化都发生在画笔的左半部分，画笔的右侧是纯色且与上一个 [**GradientStop**](https://msdn.microsoft.com/library/windows/apps/BR210078) 颜色相同，请将 **StartPoint** 指定为 `(0,0)`，将 **EndPoint** 指定为 `(0.5,0)`。

### <span id="Use_tools_to_make_gradients"></span><span id="use_tools_to_make_gradients"></span><span id="USE_TOOLS_TO_MAKE_GRADIENTS"></span>使用工具创建渐变

既然你知道了线性渐变的工作原理，你可以使用 Visual Studio 或 Blend 简化创建这些渐变的工作。 若要创建一个渐变，请在设计图面或 XAML 视图中选择要应用渐变的对象。 展开“笔画”****并选择“线性渐变”****选项卡（请参阅下一张屏幕截图）。

![使用 Visual Studio 创建线性渐变。](images/tool-gradient-brush-1.png)

现在你可以更改梯度停止点的颜色，并使用底部的栏滑动其位置。 也可以通过单击该栏添加新的梯度停止点，并通过将停止点拖离该栏来删除它们（请参阅下一张屏幕截图）。

![控制梯度停止点的属性窗口的底部的栏。](images/tool-gradient-brush-2.png)

## <span id="Image_brushes"></span><span id="image_brushes"></span><span id="IMAGE_BRUSHES"></span>图像画笔

[**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/BR210101) 绘制一个包含一个图像的区域，要绘制的图像来自图像文件源。 你可以使用要加载的图像的路径来设置 [**ImageSource**](https://msdn.microsoft.com/library/windows/apps/BR210107) 属性。 通常，图像源来自 **Content** 项目，该项目是应用资源的一部分。

在默认情况下，[**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/BR210101) 会拉伸图像以完全填满绘制的区域，如果绘制区域的纵横比与图像的纵横比不同，这可能会使图像失真。 你可以更改此行为，方式是将 [**Stretch**](https://msdn.microsoft.com/library/windows/apps/BR242975) 属性更改为其默认值 **Fill** 以外的值并将它设置为 **None**、**Uniform** 或 **UniformToFill**。

下一个示例将创建一个 [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/BR210101) 并将 [**ImageSource**](https://msdn.microsoft.com/library/windows/apps/BR210107) 设置为名为“licorice.jpg”的图像，必须将该图像作为资源包括在你的应用中。 **ImageBrush** 随后会绘制由 [**Ellipse**](https://msdn.microsoft.com/library/windows/apps/BR243343) 形状定义的区域。

```xml
<Ellipse Height="200" Width="300">
   <Ellipse.Fill>
     <ImageBrush ImageSource="licorice.jpg" />
   </Ellipse.Fill>
</Ellipse>
```

下面是显示的 [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/BR210101) 的外观。

![显示的 ImageBrush。](images/brushes-imagebrush.jpg)

[**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/BR210101) 和 [**Image**](https://msdn.microsoft.com/library/windows/apps/BR242752) 均按统一资源标识符 (URI) 引用图像源文件，该图像源文件使用多种可能的图像格式。 这些图像源文件指定为 URI。 有关指定图像源、可用的图像格式并将它们打包到一个应用中的详细信息，请参阅[图像和 ImageBrush](https://msdn.microsoft.com/library/windows/apps/Mt280382)。

## 画笔和文本

还可以使用画布向文本元素应用呈现特征。 例如，[**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) 的 [**Foreground**](https://msdn.microsoft.com/library/windows/apps/BR209665) 属性接受 [**Brush**](https://msdn.microsoft.com/library/windows/apps/BR228076)。 你可以向文本应用此处描述的任何画笔。 但是，在向文本应用画笔时一定要格外谨慎，原因在于，如果你使用的画笔融合到文本的背景中（与什么背景无关）或者与文本字符的轮廓混在一起，有可能会使文本不可读。 在多数情况下，除非你希望文本元素主要用于装饰，否则使用 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) 可以实现文本元素的可读性。

甚至在使用纯色时，也要确保你选择的文本颜色相对于文本布局容器的背景颜色具有足够大的对比度。 为了实现可读性，需要考虑文本前景和文本容器背景之间的对比度级别。

## WebViewBrush

[**WebViewBrush**](https://msdn.microsoft.com/library/windows/apps/BR227703) 是一种特殊类型的画笔，可以访问通常在 [**WebView**](https://msdn.microsoft.com/library/windows/apps/BR227702) 控件中查看的内容。 **WebViewBrush** 将内容绘制到呈现图面的另一个具有 [**Brush**](https://msdn.microsoft.com/library/windows/apps/BR228076) 型属性的元素上，而不是在矩形 **WebView** 控件区域中绘制内容。 **WebViewBrush** 并非对于所有的画笔方案都适合，但它对于切换 **WebView** 非常有用。 有关详细信息，请参阅 **WebViewBrush**。

## 画笔作为 XAML 资源

你可以将任何画笔声明为 XAML 资源字典中的键控 XAML 资源。 这便于复制已经应用到 UI 中多个元素的画笔值。 这些画笔值随后将共享并应用到以 XAML 中的 [{StaticResource}](https://msdn.microsoft.com/library/windows/apps/Mt185588) 用法形式引用画笔资源的任何情况。 这包括你拥有一个引用共享画笔的 XAML 控件模板以及控件模板本身是键控 XAML 资源的情况。

## 代码中的画笔

比起使用代码来定义画笔来说，使用 XAML 指定画笔是更典型的行为。 这是因为画笔通常被定义为 XAML 资源，并且由于画笔值通常是设计工具的输出或者作为 XAML UI 定义的一部分。 然而，对于你可能希望使用代码定义画笔的很少情况，所有 [**Brush**](https://msdn.microsoft.com/library/windows/apps/BR228076) 类型均可用于代码实例化。

若要使用代码创建 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962)，请使用采用 [**Color**](https://msdn.microsoft.com/library/windows/apps/Hh673723) 参数的构造函数。 传递作为 [**Colors**](https://msdn.microsoft.com/library/windows/apps/windows.ui.colors) 类的静态属性的值，如下所示：

```cs
SolidColorBrush blueBrush = new SolidColorBrush(Windows.UI.Colors.Blue);
```

```vb
Dim blueBrush as SolidColorBrush = New SolidColorBrush(Windows.UI.Colors.Blue)
```

```cpp
blueBrush = ref new SolidColorBrush(Windows::UI::Colors::Blue);
```

对于 [**WebViewBrush**](https://msdn.microsoft.com/library/windows/apps/BR227703) 和 [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/BR210101)，请使用默认的构造函数，然后在尝试使用用于 UI 属性的该画笔前调用其他 API。

-   在你使用代码定义 [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/BR210101) 时，[**ImageSource**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.imagebrush.imagesourceproperty.aspx) 需要 [**BitmapImage**](https://msdn.microsoft.com/library/windows/apps/BR243235)（而非 URI）。 如果源是一个流，请使用 [**SetSourceAsync**](https://msdn.microsoft.com/library/windows/apps/JJ191522) 方法来初始化该值。 如果源是一个 URI，（其中包含应用中使用 **ms-appx** 或 **ms-resource** 方案的内容），请使用采用 URI 的 [**BitmapImage**](https://msdn.microsoft.com/library/windows/apps/br243238.aspx) 构造函数。 如果在检索或解码图像资源时存在任何计时问题，而你可能在图像资源可用前需要使用替代内容用以显示，则还可以考虑处理 [**ImageOpened**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imagebrush.imageopened.aspx) 事件。
-   对于 [**WebViewBrush**](https://msdn.microsoft.com/library/windows/apps/BR227703)，如果你最近已重设 [**SourceName**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewbrush.sourcename.aspx) 属性或者如果 [**WebView**](https://msdn.microsoft.com/library/windows/apps/BR227702) 的内容也随代码更改，则可能需要调用 [**Redraw**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewbrush.redraw.aspx)。

有关代码示例，请参阅 [**WebViewBrush**](https://msdn.microsoft.com/library/windows/apps/BR227703) 和 [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/BR210101) 的参考页面。
 

 







<!--HONumber=Nov16_HO1-->


