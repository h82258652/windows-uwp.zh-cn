---
author: Jwmsft
ms.assetid: 54CC0BD4-1961-44D7-AB40-6E8B58E42D65
title: "绘制图形"
description: "了解如何绘制形状，如椭圆、矩形、多边形以及路径。 Path 类是在 XAML UI 中可视化基于相当复杂矢量的绘图语言的方法；例如，可以绘制贝塞尔曲线。"
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: 3add60b9067f40c5d330bd66a84eea41b465e8d7

---
# 绘制形状

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


** 重要的 API **

-   [**Path**](https://msdn.microsoft.com/library/windows/apps/BR243355)
-   [**Windows.UI.Xaml.Shapes 命名空间**](https://msdn.microsoft.com/library/windows/apps/BR243401)
-   [**Windows.UI.Xaml.Media 命名空间**](https://msdn.microsoft.com/library/windows/apps/BR243045)

了解如何绘制形状，如椭圆、矩形、多边形以及路径。 [**Path**](https://msdn.microsoft.com/library/windows/apps/BR243355) 类是在 XAML UI 中可视化基于相当复杂矢量的绘图语言的方法；例如，可以绘制贝塞尔曲线。

## 介绍

可以使用下面的两组类定义 XAML UI 中的空间区域：[**Shape**](https://msdn.microsoft.com/library/windows/apps/BR243377) 类和 [**Geometry**](https://msdn.microsoft.com/library/windows/apps/BR210041) 类。 这些类之间的主要区别在于，**Shape** 具有一个与其关联的画笔并可以呈现到屏幕，而 **Geometry** 只定义一个空间区域并且不进行呈现，除非它用于帮助将信息提供给另一 UI 属性。 你可以将 **Shape** 视为 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911)，其边界通过 **Geometry** 定义。 本主题主要讨论 **Shape** 类。

[**Shape**](https://msdn.microsoft.com/library/windows/apps/BR243377) 类包括 [**Line**](https://msdn.microsoft.com/library/windows/apps/BR243345)、[**Ellipse**](https://msdn.microsoft.com/library/windows/apps/BR243343)、[**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371)、[**Polygon**](https://msdn.microsoft.com/library/windows/apps/BR243359)、[**Polyline**](https://msdn.microsoft.com/library/windows/apps/BR243365) 和 [**Path**](https://msdn.microsoft.com/library/windows/apps/BR243355)。 **Path** 非常有趣，因为它可以定义任意几何图形，同时还会在此处介绍 [**Geometry**](https://msdn.microsoft.com/library/windows/apps/BR210041) 类，因为这是定义部分 **Path** 的一个方法。

## 形状的 Fill 和 Stroke

为了将 [**Shape**](https://msdn.microsoft.com/library/windows/apps/BR243377) 呈现到应用画布上，必须在它与 [**Brush**](https://msdn.microsoft.com/library/windows/apps/BR228076) 之间建立关联。 将 **Shape** 的 [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill) 属性设置为所需的 **Brush**。 有关画笔的详细信息，请参阅[使用笔画](using-brushes.md)。

[**Shape**](https://msdn.microsoft.com/library/windows/apps/BR243377) 还可以有一个 [**Stroke**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.stroke)（在形状的外围绘制的线条）。 **Stroke** 还需要一个用于定义其外观的 [**Brush**](https://msdn.microsoft.com/library/windows/apps/BR228076)，而且其 [**StrokeThickness**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.strokethickness) 应当具有非零值。 **StrokeThickness** 是一个属性，用来定义形状边缘的外围粗细。 如果你没有为 **Stroke** 指定 **Brush** 值，或者如果你将 **StrokeThickness** 设置为 0，则将不绘制形状周围的边界。

## Ellipse

[**Ellipse**](https://msdn.microsoft.com/library/windows/apps/BR243343) 是具有弯曲外围的形状。 若要创建基本的 **Ellipse**，请为 [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill) 指定 [**Width**](https://msdn.microsoft.com/library/windows/apps/BR208751)、[**Height**](https://msdn.microsoft.com/library/windows/apps/BR208718) 和 [**Brush**](https://msdn.microsoft.com/library/windows/apps/BR228076)。

下一个示例将创建一个 [**Ellipse**](https://msdn.microsoft.com/library/windows/apps/BR243343)，其 [**Width**](https://msdn.microsoft.com/library/windows/apps/BR208751) 为 200，[**Height**](https://msdn.microsoft.com/library/windows/apps/BR208718) 为 200，而且使用 [**SteelBlue**](https://msdn.microsoft.com/library/windows/apps/Hh748056) 颜色的 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) 作为其 [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill)。

```xml
<Ellipse Fill="SteelBlue" Height="200" Width="200" />
```

此处是呈现的 [**Ellipse**](https://msdn.microsoft.com/library/windows/apps/BR243343)。

![显示的椭圆。](images/shapes-ellipse.jpg)

在这种情况下，许多人会将 [**Ellipse**](https://msdn.microsoft.com/library/windows/apps/BR243343) 视为圆，但是实际上它是在 XAML 中声明圆形的方法：使用 [**Width**](https://msdn.microsoft.com/library/windows/apps/BR208751) 与 [**Height**](https://msdn.microsoft.com/library/windows/apps/BR208718) 相等的 **Ellipse**。

在将 [**Ellipse**](https://msdn.microsoft.com/library/windows/apps/BR243343) 放置在 UI 布局中时，会假设它的大小与具有该 [**Width**](https://msdn.microsoft.com/library/windows/apps/BR208751) 和 [**Height**](https://msdn.microsoft.com/library/windows/apps/BR208718) 的矩形相同；不呈现外围外部的区域，但它仍是该椭圆的布局槽大小的一部分。

6 个 [**Ellipse**](https://msdn.microsoft.com/library/windows/apps/BR243343) 元素为一个组，它们属于 [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/BR227538) 控件的控件模板，并且 2 个同心 **Ellipse** 元素属于 [**RadioButton**](https://msdn.microsoft.com/library/windows/apps/BR227544)。

## <span id="Rectangle"></span><span id="rectangle"></span><span id="RECTANGLE"></span>Rectangle

[**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371) 形状有四个边而且相对的两个边相等。 若要创建基本的 **Rectangle**，请指定 [**Width**](https://msdn.microsoft.com/library/windows/apps/BR208751)、[**Height**](https://msdn.microsoft.com/library/windows/apps/BR208718) 和 [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill)。

你可以为 [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371) 创建圆角。 若要创建圆角，请指定 [**RadiusX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.rectangle.radiusx.aspx) 和 [**RadiusY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.rectangle.radiusy) 属性的值。 这些属性指定椭圆的 X 轴和 Y 轴，以定义角的曲线。 **RadiusX** 的最大允许值为 [**Width**](https://msdn.microsoft.com/library/windows/apps/BR208751) 的一半，**RadiusY** 的最大允许值为 [**Height**](https://msdn.microsoft.com/library/windows/apps/BR208718) 的一半。

下一个示例创建 [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371)，其 [**Width**](https://msdn.microsoft.com/library/windows/apps/BR208751) 为 200，[**Height**](https://msdn.microsoft.com/library/windows/apps/BR208718) 为 100。 它将 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) 的 [**Blue**](https://msdn.microsoft.com/library/windows/apps/Hh747837) 值用于其 [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill)，并将 **SolidColorBrush** 的 [**Black**](https://msdn.microsoft.com/library/windows/apps/Hh747833) 值用于其 [**Stroke**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.stroke)。 我们将 [**StrokeThickness**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.strokethickness) 设置为 3。 我们将 [**RadiusX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.rectangle.radiusx.aspx) 属性设置为 50，并将 [**RadiusY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.rectangle.radiusy) 属性设置为 10，从而为 **Rectangle** 创建圆角。

```xml
<Rectangle Fill="Blue"
           Width="200"
           Height="100"
           Stroke="Black"
           StrokeThickness="3"
           RadiusX="50"
           RadiusY="10" />
           ```

Here's the rendered [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371).

![A rendered Rectangle.](images/shapes-rectangle.jpg)

**Tip**  There are some scenarios for UI definitions where instead of using a [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371), a [**Border**](https://msdn.microsoft.com/library/windows/apps/BR209250) might be more appropriate. If your intention is to create a rectangle shape around other content, it might be better to use **Border** because it can have child content and will automatically size around that content, rather than using the fixed dimensions for height and width like **Rectangle** does. A **Border** also has the option of having rounded corners if you set the [**CornerRadius**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.border.cornerradius) property.

 

On the other hand, a [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371) is probably a better choice for control composition. A **Rectangle** shape is seen in many control templates because it's used as a "FocusVisual" part for focusable controls. Whenever the control is in a "Focused" visual state, this rectangle is made visible, in other states it's hidden.

## Polygon

A [**Polygon**](https://msdn.microsoft.com/library/windows/apps/BR243359) is a shape with a boundary defined by an arbitrary number of points. The boundary is created by connecting a line from one point to the next, with the last point connected to the first point. The [**Points**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.polygon.points.aspx) property defines the collection of points that make up the boundary. In XAML, you define the points with a comma-separated list. In code-behind you use a [**PointCollection**](https://msdn.microsoft.com/library/windows/apps/BR210220) to define the points and you add each individual point as a [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) value to the collection.

You don't need to explicitly declare the points such that the start point and end point are both specified as the same [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) value. The rendering logic for a [**Polygon**](https://msdn.microsoft.com/library/windows/apps/BR243359) assumes that you are defining a closed shape and will connect the end point to the start point implicitly.

The next example creates a [**Polygon**](https://msdn.microsoft.com/library/windows/apps/BR243359) with 4 points set to `(10,200)`, `(60,140)`, `(130,140)`, and `(180,200)`. It uses a [**LightBlue**](https://msdn.microsoft.com/library/windows/apps/Hh747960) value of [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) for its [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill), and has no value for [**Stroke**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.stroke) so it has no perimeter outline.

```xml
<Polygon Fill="LightBlue"
         Points="10,200,60,140,130,140,180,200" />
```

此处是呈现的 [**Polygon**](https://msdn.microsoft.com/library/windows/apps/BR243359)。

![呈现的多边形。](images/shapes-polygon.jpg)

**提示** 在除了声明形状顶点的其他方案的 XAML 中，[**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) 值通常用作类型。 例如，**Point** 属于触摸事件的事件数据，因此你可以精确了解触摸操作发生在坐标空间的哪个位置。 有关 **Point** 以及如何将其用于 XAML 或代码的详细信息，请参阅 [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) 的 API 参考主题。

 

## Line

[**Line**](https://msdn.microsoft.com/library/windows/apps/BR243345) 只是一条在坐标空间中的两个点之间绘制的直线。 **Line** 忽略为 [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill) 提供的任何值，因为它没有内部空间。 对于 **Line**，请确保为 [**Stroke**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.stroke) 和 [**StrokeThickness**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.strokethickness) 属性指定值，否则 **Line** 将不呈现。

不要使用 [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) 值指定 [**Line**](https://msdn.microsoft.com/library/windows/apps/BR243345) 形状，而应针对 [**X1**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.line.x1.aspx)、[**Y1**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.line.y1.aspx)、[**X2**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.line.x2.aspx) 和 [**Y2**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.line.y2.aspx) 使用离散的 [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx) 值。 这会使得横线或竖线的标记最少。 例如，`<Line Stroke="Red" X2="400"/>` 定义一条长为 400 个像素的横线。 另一对 X,Y 属性在默认情况下为 0，因此，从点的角度看，此 XAML 将绘制一条从 `(0,0)` 到 `(400,0)` 的直线。 如果你希望它从 (0,0) 之外的任意点开始，则可以使用 [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027) 移动整个 **Line**。

## <span id="_Polyline"></span><span id="_polyline"></span><span id="_POLYLINE"></span> Polyline

[**Polyline**](https://msdn.microsoft.com/library/windows/apps/BR243365) 与 [**Polygon**](https://msdn.microsoft.com/library/windows/apps/BR243359) 类似，该形状的边也是通过一组点来进行定义，只不过 **Polyline** 的最后一个点不与第一个点相连。

**注意** 你可以在 [**Points**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.polyline.points.aspx) 中为 [**Polyline**](https://msdn.microsoft.com/library/windows/apps/BR243365) 明确设置相同的起点和终点，但是，在这种情况下，你可能已改用 [**Polygon**](https://msdn.microsoft.com/library/windows/apps/BR243359)。

 

如果你指定 [**Polyline**](https://msdn.microsoft.com/library/windows/apps/BR243365) 的 [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill)，则 **Fill** 会绘制形状的内部空间，即使为 **Polyline** 设置的 [**Points**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.polyline.points.aspx) 的起点和终点不相交也是如此。 如果你没有指定 **Fill**，则 **Polyline** 与指定了多个单独的、其连续直线的起点和终点相交的 [**Line**](https://msdn.microsoft.com/library/windows/apps/BR243345) 元素时所呈现的内容相似。

就像 [**Polygon**](https://msdn.microsoft.com/library/windows/apps/BR243359) 一样，[**Points**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.polyline.points.aspx) 属性定义组成边的点集。 在 XAML 中，使用逗号分隔的列表定义点。 在代码隐藏中，使用 [**PointCollection**](https://msdn.microsoft.com/library/windows/apps/BR210220) 来定义点，并将每个单独的点作为 [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) 结构添加到集合中。

此示例创建 [**Polyline**](https://msdn.microsoft.com/library/windows/apps/BR243365)，其 4 个顶点分别设置为 `(10,200)`、`(60,140)`、`(130,140)` 以及 `(180,200)`。 定义了一个没有 [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill) 的 [**Stroke**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.stroke)。

```xml
<Polyline Stroke="Black"
        StrokeThickness="4"
        Points="10,200,60,140,130,140,180,200" />
```

此处是呈现的 [**Polyline**](https://msdn.microsoft.com/library/windows/apps/BR243365)。 请注意，第一个点和最后一个点不像在 [**Polygon**](https://msdn.microsoft.com/library/windows/apps/BR243359) 那样由 [**Stroke**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.stroke) 轮廓连接起来。

![呈现的折线。](images/shapes-polyline.jpg)

## Path

[**Path**](https://msdn.microsoft.com/library/windows/apps/BR243355) 是最通用的 [**Shape**](https://msdn.microsoft.com/library/windows/apps/BR243377)，因为使用它可以定义任意几何图形。 但是这种通用性非常复杂。 让我们来看看如何在 XAML 中创建一个基本的 **Path**。

定义其路径具有 [**Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data) 属性的几何图形。 可使用两种技术设置 **Data**：

-   你可以在 XAML 中为 [**Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data) 定义字符串值。 在这种形状中，**Path.Data** 值对于图形采用序列化格式。 在首次设置了该值后，你通常无需以字符串对该值进行文本编辑。 而是应当使用能够在图面上的设计或绘制标记中工作的设计工具。 然后，可以保存或导出输出内容，系统会为你提供一个包含 **Path.Data** 信息的 XAML 文件或 XAML 字符串片段。
-   可以将 [**Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data) 属性设置为单个 [**Geometry**](https://msdn.microsoft.com/library/windows/apps/BR210041) 对象。 这可以通过在代码或在 XAML 中来完成。 这个 **Geometry** 通常是充当容器的 [**GeometryGroup**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.geometrygroup)，该容器可以将多个几何图形定义组合到单个对象中以形成对象模型。 这样做最常见的理由是，你希望使用一个或多个可以定义为 [**PathFigure**](https://msdn.microsoft.com/library/windows/apps/BR210143) 的 [**Segments**](https://msdn.microsoft.com/library/windows/apps/BR210164) 值（例如 [**BezierSegment**](https://msdn.microsoft.com/library/windows/apps/BR228068)）的曲线和复杂形状。

此示例显示了一个 [**Path**](https://msdn.microsoft.com/library/windows/apps/BR243355)，它是由以下操作生成的：使用 Blend for Visual Studio 生成少数几个矢量形状，然后将结果另存为 XAML。 整个 **Path** 由一条贝塞尔曲线和一条线段组成。 此示例主要是为了举例说明 [**Path.Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data) 序列化格式中存在的元素以及各个数字所代表的含义。

此 [**Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data) 从 move 命令（由“M”指示）开始，该命令为此路径指定起点的绝对值。

第一段是三次方贝塞尔曲线，起点为 `(100,200)`，终点为 `(400,175)`，通过两个控制点 `(100,25)` 和 `(400,350)` 绘制而成。 这一段由 [**Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data) 属性字符串中的“C”命令指示。

第二段从横线的绝对值命令“H”开始，该命令指定一条从上一个子路径的终结点 `(400,175)` 到新的终结点 `(280,175)` 绘制的线段。 由于这是一个横线命令，因此指定的值为 x 坐标。

```xml
<Path Stroke="DarkGoldenRod" 
      StrokeThickness="3"
      Data="M 100,200 C 100,25 400,350 400,175 H 280" />
      ```

Here's the rendered [**Path**](https://msdn.microsoft.com/library/windows/apps/BR243355).

![A rendered Path.](images/shapes-path.jpg)

The next example shows a usage of the other technique we discussed: a [**GeometryGroup**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.geometrygroup) with a [**PathGeometry**](https://msdn.microsoft.com/library/windows/apps/BR210168). This example exercises some of the contributing geometry types that can be used as part of a **PathGeometry**: [**PathFigure**](https://msdn.microsoft.com/library/windows/apps/BR210143) and the various elements that can be a segment in [**PathFigure.Segments**](https://msdn.microsoft.com/library/windows/apps/BR210164).

```xml
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

使用 [**PathGeometry**](https://msdn.microsoft.com/library/windows/apps/BR210168) 可能比填充 [**Path.Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data) 字符串更具可读性。 另一方面，[**Path.Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data) 使用兼容于可缩放的向量图形 (SVG) 图像路径定义的语法，因此它可用于从 SVG 移植图形，或者用作工具（例如 Blend）的输出。

 

 







<!--HONumber=Jul16_HO2-->


