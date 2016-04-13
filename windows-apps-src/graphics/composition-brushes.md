---
ms.assetid: 03dd256f-78c0-e1b1-3d9f-7b3afab29b2f
合成画笔
画笔通过其输出绘制 Visual 的区域。 不同的画笔具有不同的输出类型。
---
# 合成画笔

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

画笔通过其输出绘制 [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) 的区域。 不同的画笔具有不同的输出类型。 合成 API 提供了三种画笔类型：

-   [
            **CompositionColorBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589399) 使用纯色绘制视觉对象
-   [
            **CompositionSurfaceBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589415) 使用合成图面的内容绘制视觉对象
-   [
            **CompositionEffectBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589406) 使用合成效果的内容绘制视觉对象

所有画笔均继承自 [**CompositionBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589398)；它们可由 [**Compositor**](https://msdn.microsoft.com/library/windows/apps/Dn706789) 直接或间接创建，并且是独立于设备的资源。 尽管画笔都是独立于设备的，但 [**CompositionSurfaceBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589415) 和 [**CompositionEffectBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589406) 需使用依赖于设备的合成图面的内容绘制 [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858)。

-   [先决条件](./composition-brushes.md#prerequisites)
-   [颜色基础知识](./composition-brushes.md#color-basics)
    -   [Alpha 模式](./composition-brushes.md#alpha-modes)
-   [使用颜色画笔](./composition-brushes.md#using-color-brush)
-   [使用图面画笔](./composition-brushes.md#using-surface-brush)
-   [配置拉伸和对齐](./composition-brushes.md#configuring-stretch-and-alignment)

## 先决条件

本概述假定你已熟悉基本合成应用程序的结构，如[合成 UI](visual-layer.md) 中所述。

## 颜色基础知识

在使用 [**CompositionColorBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589399) 绘制之前，你需要选择颜色。 合成 API 使用 Windows 运行时结构和 Color 来表示某种颜色。 Color 结构使用 sRGB 编码。 sRGB 编码将颜色划分为以下四种：alpha、红色、绿色和蓝色。 每个分量均由一个浮点值表示，该值通常介于 0.0 和 1.0 之间。 值 0.0 表示完全没有该颜色，而值 1.0 表示该颜色完全呈现。 对于 alpha 分量，0.0 表示完全透明的颜色，而 1.0 表示完全不透明的颜色。

### Alpha 模式

[
            **CompositionColorBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589399) 中的 Color 值始终表示 straight alpha。

## 使用颜色画笔

若要创建颜色画笔，请调用 Compositor.[**CreateColorBrush**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.composition.compositor.createcolorbrush.aspx) 方法，从而返回 [**CompositionColorBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589399)。 **CompositionColorBrush** 的默认颜色为 \#00000000。 下图和代码显示了一个较小的可视化树来创建一个矩形，该矩形用黑色画笔描边，并使用颜色值为 0x9ACD32 的纯色画笔进行绘制。

![CompositionColorBrush](images/composition-compositioncolorbrush.png)
```cs
Compositor _compositor;
ContainerVisual _container;
SpriteVisual visual1, visual2;
CompositionColorBrush _blackBrush, _greenBrush; 

_compositor = new Compositor();
_container = _compositor.CreateContainerVisual();

_blackBrush = _compositor.CreateColorBrush(Colors.Black);
visual1 = _compositor.CreateSpriteVisual();
visual1.Brush = _blackBrush;
visual1.Size = new Vector2(156, 156);
visual1.Offset = new Vector3(0, 0, 0);

_ greenBrush = _compositor.CreateColorBrush(Color.FromArgb(0xff, 0x9A, 0xCD, 0x32));
Visual2 = _compositor.CreateSpriteVisual();
Visual2.Brush = _greenBrush;
Visual2.Size = new Vector2(150, 150);
Visual2.Offset = new Vector3(3, 3, 0);
```

与其他画笔不同，创建 [**CompositionColorBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589399) 是一项开销相对较少的操作。 每次呈现时你都可以创建 **CompositionColorBrush** 对象，而对性能仅产生较小的影响或不产生任何影响。

## 使用图面画笔

[
            **CompositionSurfaceBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589415) 使用合成图面（由 [**ICompositionSurface**](https://msdn.microsoft.com/library/windows/apps/Dn706819) 对象表示）绘制视觉对象 下图显示了一个使用 licorice 位图绘制的正方形，其中该位图使用 D2D 呈现在 **ICompositionSurface** 上。

![CompositionSurfaceBrush](images/composition-compositionsurfacebrush.png)
第一个示例初始化合成图面以供与画笔结合使用。 合成图面使用帮助程序方法 LoadImage 进行创建，该方法将 [**CompositionSurfaceBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589415) 和 Url 用作字符串。 它将从该 Url 加载图像，将该图像呈现在 [**ICompositionSurface**](https://msdn.microsoft.com/library/windows/apps/Dn706819) 上，并将图面设置为 **CompositionSurfaceBrush** 的内容。 请注意，**ICompositionSurface** 仅在本机代码中公开，因此 LoadImage 方法仅在本机代码中实现。

```cs
LoadImage(Brush,
          "ms-appx:///Assets/liqorice.png");
```

若要创建图面画笔，请调用 Compositor.[**CreateSurfaceBrush**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.composition.compositor.createsurfacebrush.aspx) 方法。 该方法返回 [**CompositionSurfaceBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589415) 对象。 以下代码显示了可用于通过 **CompositionSurfaceBrush** 的内容绘制可视对象的代码。

```cs
Compositor _compositor;
ContainerVisual _container;
SpriteVisual visual;
CompositionSurfaceBrush _surfaceBrush;

_surfaceBrush = _compositor.CreateSurfaceBrush();
LoadImage(_surfaceBrush, "ms-appx:///Assets/liqorice.png");
visual.Brush = _surfaceBrush;
```

## 配置拉伸和对齐

有时，[**CompositionSurfaceBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589415) 的 [**ICompositionSurface**](https://msdn.microsoft.com/library/windows/apps/Dn706819) 的内容不能完全填充正在绘制的视觉区域。 如果出现这种情况，合成 API 将使用该画笔的 [**HorizontalAlignmentRatio**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.composition.compositionsurfacebrush.horizontalalignmentratio.aspx)、 [**VerticalAlignmentRatio**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionsurfacebrush.verticalalignmentratio) 和 [**Stretch**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionsurfacebrush.stretch) 模式设置来确定如何填充剩余的区域。

-   [
            **HorizontalAlignmentRatio**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.composition.compositionsurfacebrush.horizontalalignmentratio.aspx) 和 [**VerticalAlignmentRatio**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionsurfacebrush.verticalalignmentratio) 均属于浮点类型，并且均可用于控制画笔在视觉对象边界内的定位。
    -   值 0.0 指示将画笔的左/上角与可视对象的左/上角对齐
    -   值 0.5 指示将画笔的中心与可视对象的中心对齐
    -   值 1.0 指示将画笔的右/下角与可视对象的右/下角对齐
-   [
            **Stretch**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionsurfacebrush.stretch) 属性接受 [**CompositionStretch**](https://msdn.microsoft.com/library/windows/apps/Dn706786) 枚举定义的以下值：
    -   None：不会为了填充可视对象边界而拉伸画笔。 请留意此拉伸设置：如果画笔超出可视对象边界， 将剪裁该画笔的内容。 可使用 [**HorizontalAlignmentRatio**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.composition.compositionsurfacebrush.horizontalalignmentratio.aspx) 和 [**VerticalAlignmentRatio**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionsurfacebrush.verticalalignmentratio) 属性 控制用于绘制可视对象边界的画笔部分。
    -   Uniform：画笔将缩放以填充可视对象边界；保留画笔的纵横比。 这是默认值。
    -   UniformToFill：画笔将进行缩放，以便可以完全填充可视对象边界；保留画笔的纵横比。
    -   Fill：画笔将缩放以填充可视对象边界。 因为画笔的高度和宽度是独立缩放的，所以可能不能保留画笔的原始纵横比。 也就是说，画笔可能为了完全填充可视对象边界而失真。

 

 






<!--HONumber=Mar16_HO1-->


