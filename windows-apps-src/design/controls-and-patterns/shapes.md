---
author: Jwmsft
ms.assetid: 54CC0BD4-1961-44D7-AB40-6E8B58E42D65
title: 绘制图形
description: 了解如何绘制形状，如椭圆、矩形、多边形以及路径。 Path 类是在 XAML UI 中可视化基于相当复杂矢量的绘图语言的方法；例如，可以绘制贝塞尔曲线。
ms.author: jimwalk
ms.date: 11/16/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 984653ad20fc40035528ab7e32b904e64d6ff8c5
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2018
ms.locfileid: "5695971"
---
# <a name="draw-shapes"></a>绘制形状

了解如何绘制形状，如椭圆、矩形、多边形以及路径。 [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path) 类是在 XAML UI 中可视化基于相当复杂矢量的绘图语言的方法；例如，可以绘制贝塞尔曲线。

> **重要 API**：[路径类](/uwp/api/Windows.UI.Xaml.Shapes.Path)、[Windows.UI.Xaml.Shapes 命名空间](/uwp/api/Windows.UI.Xaml.Shapes)、[Windows.UI.Xaml.Media 命名空间](/uwp/api/Windows.UI.Xaml.Media)


可以使用下面的两组类定义 XAML UI 中的空间区域：[**Shape**](/uwp/api/Windows.UI.Xaml.Shapes.Shape) 类和 [**Geometry**](/uwp/api/Windows.UI.Xaml.Media.Geometry) 类。 这些类之间的主要区别在于，**Shape** 具有一个与其关联的画笔并可以呈现到屏幕，而 **Geometry** 只定义一个空间区域并且不进行呈现，除非它用于帮助将信息提供给另一 UI 属性。 你可以将 **Shape** 视为 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911)，其边界通过 **Geometry** 定义。 本主题主要讨论 **Shape** 类。

[**Shape**](/uwp/api/Windows.UI.Xaml.Shapes.Shape) 类包括 [**Line**](/uwp/api/Windows.UI.Xaml.Shapes.Line)、[**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse)、[**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle)、[**Polygon**](/uwp/api/Windows.UI.Xaml.Shapes.Polygon)、[**Polyline**](/uwp/api/Windows.UI.Xaml.Shapes.Polyline) 和 [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path)。 **Path** 非常有趣，因为它可以定义任意几何图形，同时还会在此处介绍 [**Geometry**](/uwp/api/Windows.UI.Xaml.Media.Geometry) 类，因为这是定义部分 **Path** 的一个方法。

## <a name="fill-and-stroke-for-shapes"></a>形状的 Fill 和 Stroke

为了将 [**Shape**](/uwp/api/Windows.UI.Xaml.Shapes.Shape) 呈现到应用画布上，必须在它与 [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) 之间建立关联。 将 **Shape** 的 [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill) 属性设置为所需的 **Brush**。 有关画笔的详细信息，请参阅[使用笔画](../style/brushes.md)。

[**Shape**](/uwp/api/Windows.UI.Xaml.Shapes.Shape) 还可以有一个 [**Stroke**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.stroke)（在形状的外围绘制的线条）。 **Stroke** 还需要一个用于定义其外观的 [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush)，而且其 [**StrokeThickness**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.strokethickness) 应当具有非零值。 **StrokeThickness** 是一个属性，用来定义形状边缘的外围粗细。 如果你没有为 **Stroke** 指定 **Brush** 值，或者如果你将 **StrokeThickness** 设置为 0，则将不绘制形状周围的边界。

## <a name="ellipse"></a>Ellipse

[**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse) 是具有弯曲外围的形状。 若要创建基本的 **Ellipse**，请为 [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill) 指定 [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width)、[**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) 和 [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush)。

下一个示例将创建一个 [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse)，其 [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) 为 200，[**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) 为 200，而且使用 [**SteelBlue**](https://msdn.microsoft.com/library/windows/apps/Hh748056) 颜色的 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) 作为其 [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill)。

```xaml
<Ellipse Fill="SteelBlue" Height="200" Width="200" />
```

```csharp
var ellipse1 = new Ellipse();
ellipse1.Fill = new SolidColorBrush(Windows.UI.Colors.SteelBlue);
ellipse1.Width = 200;
ellipse1.Height = 200;

// When you create a XAML element in code, you have to add
// it to the XAML visual tree. This example assumes you have
// a panel named 'layoutRoot' in your XAML file, like this:
// <Grid x:Name="layoutRoot>
layoutRoot.Children.Add(ellipse1);
```

此处是呈现的 [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse)。

![显示的椭圆。](images/shapes-ellipse.jpg)

在这种情况下，许多人会将 [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse) 视为圆，但是实际上它是在 XAML 中声明圆形的方法：使用 [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) 与 [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) 相等的 **Ellipse**。

在将 [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse) 放置在 UI 布局中时，会假设它的大小与具有该 [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) 和 [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) 的矩形相同；不呈现外围外部的区域，但它仍是该椭圆的布局槽大小的一部分。

6 个 [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse) 元素为一个组，它们属于 [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/BR227538) 控件的控件模板，并且 2 个同心 **Ellipse** 元素属于 [**RadioButton**](https://msdn.microsoft.com/library/windows/apps/BR227544)。

## <a name="span-idrectanglespanspan-idrectanglespanspan-idrectanglespanrectangle"></a><span id="Rectangle"></span><span id="rectangle"></span><span id="RECTANGLE"></span>Rectangle

[**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) 形状有四个边而且相对的两个边相等。 若要创建基本的 **Rectangle**，请指定 [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width)、[**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) 和 [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill)。

你可以为 [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) 创建圆角。 若要创建圆角，请指定 [**RadiusX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.rectangle.radiusx.aspx) 和 [**RadiusY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.rectangle.radiusy) 属性的值。 这些属性指定椭圆的 X 轴和 Y 轴，以定义角的曲线。 **RadiusX** 的最大允许值为 [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) 的一半，**RadiusY** 的最大允许值为 [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) 的一半。

下一个示例创建 [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle)，其 [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) 为 200，[**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) 为 100。 它将 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) 的 [**Blue**](https://msdn.microsoft.com/library/windows/apps/Hh747837) 值用于其 [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill)，并将 **SolidColorBrush** 的 [**Black**](https://msdn.microsoft.com/library/windows/apps/Hh747833) 值用于其 [**Stroke**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.stroke)。 我们将 [**StrokeThickness**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.strokethickness) 设置为 3。 我们将 [**RadiusX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.rectangle.radiusx.aspx) 属性设置为 50，并将 [**RadiusY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.rectangle.radiusy) 属性设置为 10，从而为 **Rectangle** 创建圆角。

```xaml
<Rectangle Fill="Blue"
           Width="200"
           Height="100"
           Stroke="Black"
           StrokeThickness="3"
           RadiusX="50"
           RadiusY="10" />
```

```csharp
var rectangle1 = new Rectangle();
rectangle1.Fill = new SolidColorBrush(Windows.UI.Colors.Blue);
rectangle1.Width = 200;
rectangle1.Height = 100;
rectangle1.Stroke = new SolidColorBrush(Windows.UI.Colors.Black);
rectangle1.StrokeThickness = 3;
rectangle1.RadiusX = 50;
rectangle1.RadiusY = 10;

// When you create a XAML element in code, you have to add
// it to the XAML visual tree. This example assumes you have
// a panel named 'layoutRoot' in your XAML file, like this:
// <Grid x:Name="layoutRoot>
layoutRoot.Children.Add(rectangle1);
```

此处是呈现的 [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle)。

![呈现的矩形。](images/shapes-rectangle.jpg)

**提示**有某些方案对于 UI 定义，而不是使用一个[**矩形**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle)，[**边框**](https://msdn.microsoft.com/library/windows/apps/BR209250)可能更合理。 如果你打算在其他内容周围创建一个矩形形状，最好使用 **Border**，因为它可能会有子内容而且将自动在子内容周围调整大小，而不是像 **Rectangle** 那样使用固定的高度和宽度尺寸。 **Border** 在你设置了 [**CornerRadius**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.border.cornerradius) 属性的情况下还提供了创建圆角的选项。

另一方面，[**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) 可能是控件组合的较好选择。 **Rectangle** 形状可以在很多控件模板中看到，因为它用作可获得焦点的控件的“FocusVisual”部分。 当控件处于“已设置焦点”视觉状态时，此矩形可见，在其他状态时，它处于隐藏状态。

## <a name="polygon"></a>Polygon

[**Polygon**](/uwp/api/Windows.UI.Xaml.Shapes.Polygon) 是通过任意数量的点来定义边的形状。 边通过用直线将点一个一个连接起来（最后一个点与第一个点相连）而创建。 [**Points**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.polygon.points.aspx) 属性定义组成边的点集。 在 XAML 中，使用逗号分隔的列表定义点。 在代码隐藏文件中，使用 [**PointCollection**](https://msdn.microsoft.com/library/windows/apps/BR210220) 定义各个点，并将每个点作为一个 [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) 值添加到集合中。

你不必为了将起点和终点指定为相同的 [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) 值而明确声明点。 [**Polygon**](/uwp/api/Windows.UI.Xaml.Shapes.Polygon) 的呈现逻辑假设你要定义闭合形状而且会自动将终点与起点连起来。

下一个示例创建 [**Polygon**](/uwp/api/Windows.UI.Xaml.Shapes.Polygon)，其 4 个顶点分别设置为 `(10,200)`、`(60,140)`、`(130,140)` 和 `(180,200)`。 它使用 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) 的 [**LightBlue**](https://msdn.microsoft.com/library/windows/apps/Hh747960) 值作为其 [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill)，而且其 [**Stroke**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.stroke) 没有值，以便它没有外围轮廓。

```xaml
<Polygon Fill="LightBlue"
         Points="10,200,60,140,130,140,180,200" />
```

```csharp
var polygon1 = new Polygon();
polygon1.Fill = new SolidColorBrush(Windows.UI.Colors.LightBlue);

var points = new PointCollection();
points.Add(new Windows.Foundation.Point(10, 200));
points.Add(new Windows.Foundation.Point(60, 140));
points.Add(new Windows.Foundation.Point(130, 140));
points.Add(new Windows.Foundation.Point(180, 200));
polygon1.Points = points;

// When you create a XAML element in code, you have to add
// it to the XAML visual tree. This example assumes you have
// a panel named 'layoutRoot' in your XAML file, like this:
// <Grid x:Name="layoutRoot>
layoutRoot.Children.Add(polygon1);
```

此处是呈现的 [**Polygon**](/uwp/api/Windows.UI.Xaml.Shapes.Polygon)。

![呈现的多边形。](images/shapes-polygon.jpg)

**提示**[**点**](https://msdn.microsoft.com/library/windows/apps/BR225870)值通常用作 XAML 中的类型除了声明形状顶点的其他方案。 例如，**Point** 属于触摸事件的事件数据，因此你可以精确了解触摸操作发生在坐标空间的哪个位置。 有关 **Point** 以及如何将其用于 XAML 或代码的详细信息，请参阅 [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) 的 API 参考主题。

## <a name="line"></a>Line

[**Line**](/uwp/api/Windows.UI.Xaml.Shapes.Line) 只是一条在坐标空间中的两个点之间绘制的直线。 **Line** 忽略为 [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill) 提供的任何值，因为它没有内部空间。 对于 **Line**，请确保为 [**Stroke**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.stroke) 和 [**StrokeThickness**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.strokethickness) 属性指定值，否则 **Line** 将不呈现。

不要使用 [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) 值指定 [**Line**](/uwp/api/Windows.UI.Xaml.Shapes.Line) 形状，而应针对 [**X1**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.line.x1.aspx)、[**Y1**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.line.y1.aspx)、[**X2**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.line.x2.aspx) 和 [**Y2**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.line.y2.aspx) 使用离散的 [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx) 值。 这会使得横线或竖线的标记最少。 例如，`<Line Stroke="Red" X2="400"/>` 定义一条长为 400 个像素的横线。 另一对 X,Y 属性在默认情况下为 0，因此，从点的角度看，此 XAML 将绘制一条从 `(0,0)` 到 `(400,0)` 的直线。 如果你希望它从 (0,0) 之外的任意点开始，则可以使用 [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027) 移动整个 **Line**。

```xaml
<Line Stroke="Red" X2="400"/>
```

```csharp
var line1 = new Line();
line1.Stroke = new SolidColorBrush(Windows.UI.Colors.Red);
line1.X2 = 400;

// When you create a XAML element in code, you have to add
// it to the XAML visual tree. This example assumes you have
// a panel named 'layoutRoot' in your XAML file, like this:
// <Grid x:Name="layoutRoot>
layoutRoot.Children.Add(line1);
```

## <a name="span-idpolylinespanspan-idpolylinespanspan-idpolylinespan-polyline"></a><span id="_Polyline"></span><span id="_polyline"></span><span id="_POLYLINE"></span>Polyline

[**Polyline**](/uwp/api/Windows.UI.Xaml.Shapes.Polyline) 与 [**Polygon**](/uwp/api/Windows.UI.Xaml.Shapes.Polygon) 类似，该形状的边也是通过一组点来进行定义，只不过 **Polyline** 的最后一个点不与第一个点相连。

**注意**你可以明确设置相同的起点和[**点**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.polyline.points.aspx)中的终结点设置的[**折线**](/uwp/api/Windows.UI.Xaml.Shapes.Polyline)，但是，在这种情况下你可能有[**多边形**](/uwp/api/Windows.UI.Xaml.Shapes.Polygon)改为。

如果你指定 [**Polyline**](/uwp/api/Windows.UI.Xaml.Shapes.Polyline) 的 [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill)，则 **Fill** 会绘制形状的内部空间，即使为 **Polyline** 设置的 [**Points**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.polyline.points.aspx) 的起点和终点不相交也是如此。 如果你没有指定 **Fill**，则 **Polyline** 与指定了多个单独的、其连续直线的起点和终点相交的 [**Line**](/uwp/api/Windows.UI.Xaml.Shapes.Line) 元素时所呈现的内容相似。

就像 [**Polygon**](/uwp/api/Windows.UI.Xaml.Shapes.Polygon) 一样，[**Points**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.polyline.points.aspx) 属性定义组成边的点集。 在 XAML 中，使用逗号分隔的列表定义点。 在代码隐藏中，使用 [**PointCollection**](https://msdn.microsoft.com/library/windows/apps/BR210220) 来定义点，并将每个单独的点作为 [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) 结构添加到集合中。

此示例创建 [**Polyline**](/uwp/api/Windows.UI.Xaml.Shapes.Polyline)，其 4 个顶点分别设置为 `(10,200)`、`(60,140)`、`(130,140)` 以及 `(180,200)`。 定义了一个没有 [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill) 的 [**Stroke**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.stroke)。

```xaml
<Polyline Stroke="Black"
        StrokeThickness="4"
        Points="10,200,60,140,130,140,180,200" />
```

```csharp
var polyline1 = new Polyline();
polyline1.Stroke = new SolidColorBrush(Windows.UI.Colors.Black);
polyline1.StrokeThickness = 4;

var points = new PointCollection();
points.Add(new Windows.Foundation.Point(10, 200));
points.Add(new Windows.Foundation.Point(60, 140));
points.Add(new Windows.Foundation.Point(130, 140));
points.Add(new Windows.Foundation.Point(180, 200));
polyline1.Points = points;

// When you create a XAML element in code, you have to add
// it to the XAML visual tree. This example assumes you have
// a panel named 'layoutRoot' in your XAML file, like this:
// <Grid x:Name="layoutRoot>
layoutRoot.Children.Add(polyline1);
```

此处是呈现的 [**Polyline**](/uwp/api/Windows.UI.Xaml.Shapes.Polyline)。 请注意，第一个点和最后一个点不像在 [**Polygon**](/uwp/api/Windows.UI.Xaml.Shapes.Polygon) 那样由 [**Stroke**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.stroke) 轮廓连接起来。

![呈现的折线。](images/shapes-polyline.jpg)

## <a name="path"></a>Path

[**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path) 是最通用的 [**Shape**](/uwp/api/Windows.UI.Xaml.Shapes.Shape)，因为使用它可以定义任意几何图形。 但是这种通用性非常复杂。 让我们来看看如何在 XAML 中创建一个基本的 **Path**。

定义其路径具有 [**Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data) 属性的几何图形。 可使用两种技术设置 **Data**：

- 你可以在 XAML 中为 [**Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data) 定义字符串值。 在这种形状中，**Path.Data** 值对于图形采用序列化格式。 在首次设置了该值后，你通常无需以字符串对该值进行文本编辑。 而是应当使用能够在图面上的设计或绘制标记中工作的设计工具。 然后，可以保存或导出输出内容，系统会为你提供一个包含 **Path.Data** 信息的 XAML 文件或 XAML 字符串片段。
- 可以将 [**Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data) 属性设置为单个 [**Geometry**](/uwp/api/Windows.UI.Xaml.Media.Geometry) 对象。 这可以通过在代码或在 XAML 中来完成。 这个 **Geometry** 通常是充当容器的 [**GeometryGroup**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.geometrygroup)，该容器可以将多个几何图形定义组合到单个对象中以形成对象模型。 这样做最常见的理由是，你希望使用一个或多个可以定义为 [**PathFigure**](https://msdn.microsoft.com/library/windows/apps/BR210143) 的 [**Segments**](https://msdn.microsoft.com/library/windows/apps/BR210164) 值（例如 [**BezierSegment**](https://msdn.microsoft.com/library/windows/apps/BR228068)）的曲线和复杂形状。

此示例显示了一个 [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path)，它是由以下操作生成的：使用 Blend for Visual Studio 生成少数几个矢量形状，然后将结果另存为 XAML。 整个 **Path** 由一条贝塞尔曲线和一条线段组成。 此示例主要是为了举例说明 [**Path.Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data) 序列化格式中存在的元素以及各个数字所代表的含义。

此 [**Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data) 从 move 命令（由“M”指示）开始，该命令为此路径指定起点的绝对值。

第一段是三次方贝塞尔曲线，起点为 `(100,200)`，终点为 `(400,175)`，通过两个控制点 `(100,25)` 和 `(400,350)` 绘制而成。 这一段由 [**Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data) 属性字符串中的“C”命令指示。

第二段从横线的绝对值命令“H”开始，该命令指定一条从上一个子路径的终结点 `(400,175)` 到新的终结点 `(280,175)` 绘制的线段。 由于这是一个横线命令，因此指定的值为 x 坐标。

```xaml
<Path Stroke="DarkGoldenRod" 
      StrokeThickness="3"
      Data="M 100,200 C 100,25 400,350 400,175 H 280" />
```

此处是显示的 [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path)。

![显示的路径。](images/shapes-path.jpg)

下一示例将介绍我们讨论过的其他技术的用法：具有 [**PathGeometry**](https://msdn.microsoft.com/library/windows/apps/BR210168) 的 [**GeometryGroup**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.geometrygroup)。 本例对某些可用作 **PathGeometry** 的一部分的参与几何类型进行练习：[**PathFigure**](https://msdn.microsoft.com/library/windows/apps/BR210143) 和在 [**PathFigure.Segments**](https://msdn.microsoft.com/library/windows/apps/BR210164) 中作为片段的各种元素。

```xaml
<Path Stroke="Black" StrokeThickness="1" Fill="#CCCCFF">
    <Path.Data>
        <GeometryGroup>
            <RectangleGeometry Rect="50,5 100,10" />
            <RectangleGeometry Rect="5,5 95,180" />
            <EllipseGeometry Center="100, 100" RadiusX="20" RadiusY="30"/>
            <RectangleGeometry Rect="50,175 100,10" />
            <PathGeometry>
                <PathGeometry.Figures>
                    <PathFigureCollection>
                        <PathFigure IsClosed="true" StartPoint="50,50">
                            <PathFigure.Segments>
                                <PathSegmentCollection>
                                    <BezierSegment Point1="75,300" Point2="125,100" Point3="150,50"/>
                                    <BezierSegment Point1="125,300" Point2="75,100"  Point3="50,50"/>
                                </PathSegmentCollection>
                            </PathFigure.Segments>
                        </PathFigure>
                    </PathFigureCollection>
                </PathGeometry.Figures>
            </PathGeometry>
        </GeometryGroup>
    </Path.Data>
</Path>
```

```csharp
var path1 = new Windows.UI.Xaml.Shapes.Path();
path1.Fill = new SolidColorBrush(Windows.UI.Color.FromArgb(255, 204, 204, 255));
path1.Stroke = new SolidColorBrush(Windows.UI.Colors.Black);
path1.StrokeThickness = 1;

var geometryGroup1 = new GeometryGroup();
var rectangleGeometry1 = new RectangleGeometry();
rectangleGeometry1.Rect = new Rect(50, 5, 100, 10);
var rectangleGeometry2 = new RectangleGeometry();
rectangleGeometry2.Rect = new Rect(5, 5, 95, 180);
geometryGroup1.Children.Add(rectangleGeometry1);
geometryGroup1.Children.Add(rectangleGeometry2);

var ellipseGeometry1 = new EllipseGeometry();
ellipseGeometry1.Center = new Point(100, 100);
ellipseGeometry1.RadiusX = 20;
ellipseGeometry1.RadiusY = 30;
geometryGroup1.Children.Add(ellipseGeometry1);

var pathGeometry1 = new PathGeometry();
var pathFigureCollection1 = new PathFigureCollection();
var pathFigure1 = new PathFigure();
pathFigure1.IsClosed = true;
pathFigure1.StartPoint = new Windows.Foundation.Point(50, 50);
pathFigureCollection1.Add(pathFigure1);
pathGeometry1.Figures = pathFigureCollection1;

var pathSegmentCollection1 = new PathSegmentCollection();
var pathSegment1 = new BezierSegment();
pathSegment1.Point1 = new Point(75, 300);
pathSegment1.Point2 = new Point(125, 100);
pathSegment1.Point3 = new Point(150, 50);
pathSegmentCollection1.Add(pathSegment1);

var pathSegment2 = new BezierSegment();
pathSegment2.Point1 = new Point(125, 300);
pathSegment2.Point2 = new Point(75, 100);
pathSegment2.Point3 = new Point(50, 50);
pathSegmentCollection1.Add(pathSegment2);
pathFigure1.Segments = pathSegmentCollection1;

geometryGroup1.Children.Add(pathGeometry1);
path1.Data = geometryGroup1;

// When you create a XAML element in code, you have to add
// it to the XAML visual tree. This example assumes you have
// a panel named 'layoutRoot' in your XAML file, like this:
// <Grid x:Name="layoutRoot>
layoutRoot.Children.Add(path1);
```

此处是呈现的 [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path)。

![呈现的路径。](images/shapes-path-2.png)

使用 [**PathGeometry**](https://msdn.microsoft.com/library/windows/apps/BR210168) 可能比填充 [**Path.Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data) 字符串更具可读性。 另一方面，[**Path.Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data) 使用兼容于可缩放的向量图形 (SVG) 图像路径定义的语法，因此它可用于从 SVG 移植图形，或者用作工具（例如 Blend）的输出。
