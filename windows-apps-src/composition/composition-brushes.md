---
author: jwmsft
ms.assetid: 03dd256f-78c0-e1b1-3d9f-7b3afab29b2f
title: 合成画笔
description: 画笔通过其输出绘制 Visual 的区域。 不同的画笔具有不同的输出类型。
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 730d5ae9062fe39533cd615facaf5beaa7d02ffd
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/16/2018
ms.locfileid: "6995101"
---
# <a name="composition-brushes"></a>合成画笔
从 UWP 应用程序在屏幕上可见的所有内容都是可见，因为它由画笔绘制。 画笔使你能够与内容为图像或绘图到复杂的效果链范围从简单纯色绘制用户界面 (UI) 的对象。 本主题介绍了使用 CompositionBrush 绘制的概念。

请注意，当使用 XAML UWP 应用中，你可以选择绘制[XAML 画笔](/windows/uwp/design/style/brushes)或[CompositionBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBrush)UIElement。 通常情况下，会更简单，最好选择 XAML 画笔，如果你的方案受 XAML 画笔。 例如，对的按钮，更改的文本或图像的形状填充颜色进行动画处理。 另一方面，如果你正在执行，如动画的遮罩或动画的九网格拉伸或效果链绘制 XAML 画笔不受支持，可用于 CompositionBrush 绘制[使用 UIElementXamlCompositionBrushBase](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase)。

使用可视化层时，必须使用 CompositionBrush 绘制[SpriteVisual](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.SpriteVisual)的区域。

-   [先决条件](./composition-brushes.md#prerequisites)
-   [使用 CompositionBrush 绘制](./composition-brushes.md#paint-with-a-compositionbrush)
    -   [使用纯色绘制](./composition-brushes.md#paint-with-a-solid-color)
    -   [使用线性渐变绘制](./composition-brushes.md#paint-with-a-linear-gradient)
    -   [绘制的图像](./composition-brushes.md#paint-with-an-image)
    -   [使用自定义绘制绘画](./composition-brushes.md#paint-with-a-custom-drawing)
    -   [绘制视频](./composition-brushes.md#paint-with-a-video)
    -   [绘制的筛选效果](./composition-brushes.md#paint-with-a-filter-effect)
    -   [使用不透明蒙板 CompositionBrush 绘制](./composition-brushes.md#paint-with-a-compositionbrush-with-opacity-mask-applied)
    -   [使用 NineGrid stretch CompositionBrush 绘制](./composition-brushes.md#paint-with-a-compositionbrush-using-ninegrid-stretch)
    -   [使用背景像素绘制](./composition-brushes.md#paint-using-background-pixels)
-   [组合 CompositionBrushes](./composition-brushes.md#combining-compositionbrushes)
-   [使用 XAML 画笔与 CompositionBrush](./composition-brushes.md#using-a-xaml-brush-vs-compositionbrush)
-   [相关主题](./composition-brushes.md#related-topics)

## <a name="prerequisites"></a>先决条件
本概述假定你已熟悉基本合成应用程序，结构[可视化层概述](visual-layer.md)中所述。

## <a name="paint-with-a-compositionbrush"></a>使用 CompositionBrush 绘制

[CompositionBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBrush) "绘制"通过其输出区域。 不同的画笔具有不同的输出类型。 某些画笔绘制区域使用纯色，其他与渐变、 图像、 自定义绘制或效果。 此外存在修改其他画笔的行为的特殊的画笔。 例如，通过 CompositionBrush 绘制的区域的控件可用于不透明蒙板或九网格可用于控制时绘制区域应用于 CompositionBrush 的拉伸。 CompositionBrush 可以是以下类型之一：

|类                                   |详细信息                                         |中引入了|
|-------------------------------------|---------------------------------------------------------|--------------------------------------|
|[CompositionColorBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionColorBrush)         |使用纯色绘制区域                        |Windows 10 11 月更新 (10586 SDK)|
|[CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush)       |使用[ICompositionSurface](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Composition.ICompositionSurface)的内容绘制区域|Windows 10 11 月更新 (10586 SDK)|
|[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)        |使用合成效果的内容绘制区域 |Windows 10 11 月更新 (10586 SDK)|
|[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)          |绘制与具有不透明蒙板 CompositionBrush 视觉对象 |Windows 10 周年更新 (SDK 14393)
|[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)      |使用 NineGrid stretch CompositionBrush 绘制区域 |Windows 10 周年更新 SDK (14393)
|[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)|使用一种线性渐变绘制一个区域                    |Windows 10 Fall Creators Update (Insider Preview SDK)
|[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)     |通过从应用的背景像素或直接在桌面上的应用程序的窗口后面的像素取样来绘制区域。 使用作为到另一个 CompositionBrush CompositionEffectBrush 类似的输入 | Windows 10 周年更新 (SDK 14393)

### <a name="paint-with-a-solid-color"></a>使用纯色绘制

[CompositionColorBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionColorBrush)绘制区域使用纯色。 有多种方法可用于指定颜色的 solidcolorbrush。 例如，你可以指定其 alpha、 红色、 蓝色和绿色 (ARGB) 通道或使用一种[颜色](https://docs.microsoft.com/uwp/api/windows.ui.colors)类提供的预定义颜色。

下图和代码显示了一个较小的可视化树来创建一个矩形，该矩形用黑色画笔描边，并使用颜色值为 0x9ACD32 的纯色画笔进行绘制。

![CompositionColorBrush](images/composition-compositioncolorbrush.png)

```cs
Compositor _compositor;
ContainerVisual _container;
SpriteVisual _colorVisual1, _colorVisual2;
CompositionColorBrush _blackBrush, _greenBrush;

_compositor = Window.Current.Compositor;
_container = _compositor.CreateContainerVisual();

_blackBrush = _compositor.CreateColorBrush(Colors.Black);
_colorVisual1= _compositor.CreateSpriteVisual();
_colorVisual1.Brush = _blackBrush;
_colorVisual1.Size = new Vector2(156, 156);
_colorVisual1.Offset = new Vector3(0, 0, 0);
_container.Children.InsertAtBottom(_colorVisual1);

_ greenBrush = _compositor.CreateColorBrush(Color.FromArgb(0xff, 0x9A, 0xCD, 0x32));
_colorVisual2 = _compositor.CreateSpriteVisual();
_colorVisual2.Brush = _greenBrush;
_colorVisual2.Size = new Vector2(150, 150);
_colorVisual2.Offset = new Vector3(3, 3, 0);
_container.Children.InsertAtBottom(_colorVisual2);
```

### <a name="paint-with-a-linear-gradient"></a>使用线性渐变绘制

[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)绘制一个区域的线性渐变。 线性渐变混合行，渐变轴上的两个或多个颜色。 你可以使用 GradientStop 对象以指定的颜色渐变和它们的位置中。

下图和代码显示了具有 2 个停止点使用红色和黄色颜色四面 LinearGradientBrush SpriteVisual。

![CompositionLinearGradientBrush](images/composition-compositionlineargradientbrush.png)

```cs
Compositor _compositor;
SpriteVisual _gradientVisual;
CompositionLinearGradientBrush _redyellowBrush;

_compositor = Window.Current.Compositor;

_redyellowBrush = _compositor.CreateLinearGradientBrush();
_redyellowBrush.ColorStops.Add(_compositor.CreateColorGradientStop(0, Colors.Red));
_redyellowBrush.ColorStops.Add(_compositor.CreateColorGradientStop(1, Colors.Yellow));
_gradientVisual = _compositor.CreateSpriteVisual();
_gradientVisual.Brush = _redyellowBrush;
_gradientVisual.Size = new Vector2(156, 156);
```

### <a name="paint-with-an-image"></a>绘制的图像

[CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush)到 ICompositionSurface 呈现的像素绘制一个区域。 例如，可以使用 CompositionSurfaceBrush 来与图像呈现到使用[LoadedImageSurface](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.loadedimagesurface) API ICompositionSurface 图面上绘制的区域。

下图和代码显示了 SpriteVisual 绘制的呈现到使用 LoadedImageSurface ICompositionSurface 其中该位图。 CompositionSurfaceBrush 的属性可用于拉伸和对齐的可视对象边界内的位图。

![CompositionSurfaceBrush](images/composition-compositionsurfacebrush.png)

```cs
Compositor _compositor;
SpriteVisual _imageVisual;
CompositionSurfaceBrush _imageBrush;

_compositor = Window.Current.Compositor;

_imageBrush = _compositor.CreateSurfaceBrush();

// The loadedSurface has a size of 0x0 till the image has been been downloaded, decoded and loaded to the surface. We can assign the surface to the CompositionSurfaceBrush and it will show up once the image is loaded to the surface.
LoadedImageSurface _loadedSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/licorice.jpg"));
_imageBrush.Surface = _loadedSurface;

_imageVisual = _compositor.CreateSpriteVisual();
_imageVisual.Brush = _imageBrush;
_imageVisual.Size = new Vector2(156, 156);
```

### <a name="paint-with-a-custom-drawing"></a>使用自定义绘制绘画
[CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush)还可以用于绘制某个区域中使用[Win2D](http://microsoft.github.io/Win2D/html/Introduction.htm) （或 D2D） 呈现 ICompositionSurface 像素。

以下代码演示了 SpriteVisual 绘制文本运行呈现到 ICompositionSurface 使用 Win2D。 请注意，若要使用的 Win2D 你需要在你的项目中包括[Win2D NuGet](http://www.nuget.org/packages/Win2D.uwp)程序包。

```cs
Compositor _compositor;
CanvasDevice _device;
CompositionGraphicsDevice _compositionGraphicsDevice;
SpriteVisual _drawingVisual;
CompositionSurfaceBrush _drawingBrush;

_device = CanvasDevice.GetSharedDevice();
_compositionGraphicsDevice = CanvasComposition.CreateCompositionGraphicsDevice(_compositor, _device);

_drawingBrush = _compositor.CreateSurfaceBrush();
CompositionDrawingSurface _drawingSurface = _compositionGraphicsDevice.CreateDrawingSurface(new Size(256, 256), DirectXPixelFormat.B8G8R8A8UIntNormalized, DirectXAlphaMode.Premultiplied);

using (var ds = CanvasComposition.CreateDrawingSession(_drawingSurface))
{
     ds.Clear(Colors.Transparent);
     var rect = new Rect(new Point(2, 2), (_drawingSurface.Size.ToVector2() - new Vector2(4, 4)).ToSize());
     ds.FillRoundedRectangle(rect, 15, 15, Colors.LightBlue);
     ds.DrawRoundedRectangle(rect, 15, 15, Colors.Gray, 2);
     ds.DrawText("This is a composition drawing surface", rect, Colors.Black, new CanvasTextFormat()
     {
          FontFamily = "Comic Sans MS",
          FontSize = 32,
          WordWrapping = CanvasWordWrapping.WholeWord,
          VerticalAlignment = CanvasVerticalAlignment.Center,
          HorizontalAlignment = CanvasHorizontalAlignment.Center
     }
);

_drawingBrush.Surface = _drawingSurface;

_drawingVisual = _compositor.CreateSpriteVisual();
_drawingVisual.Brush = _drawingBrush;
_drawingVisual.Size = new Vector2(156, 156);
```

同样，CompositionSurfaceBrush 还可用于绘制与使用 Win2D 互操作 SwapChain SpriteVisual。 [此示例](https://github.com/Microsoft/Win2D-Samples/tree/master/CompositionExample)提供了如何使用 Win2D 绘制与 swapchain SpriteVisual 的示例。

### <a name="paint-with-a-video"></a>绘制视频
[CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush)还可以用于绘制某个区域中使用[MediaPlayer](https://docs.microsoft.com/en-us/uwp/api/Windows.Media.Playback.MediaPlayer)类通过加载视频呈现 ICompositionSurface 像素。

下面的代码演示 SpriteVisual 四面加载到 ICompositionSurface 视频。

```cs
Compositor _compositor;
SpriteVisual _videoVisual;
CompositionSurfaceBrush _videoBrush;

// MediaPlayer set up with a create from URI

_mediaPlayer = new MediaPlayer();

// Get a source from a URI. This could also be from a file via a picker or a stream
var source = MediaSource.CreateFromUri(new Uri("http://go.microsoft.com/fwlink/?LinkID=809007&clcid=0x409"));
var item = new MediaPlaybackItem(source);
_mediaPlayer.Source = item;
_mediaPlayer.IsLoopingEnabled = true;

// Get the surface from MediaPlayer and put it on a brush
_videoSurface = _mediaPlayer.GetSurface(_compositor);
_videoBrush = _compositor.CreateSurfaceBrush(_videoSurface.CompositionSurface);

_videoVisual = _compositor.CreateSpriteVisual();
_videoVisual.Brush = _videoBrush;
_videoVisual.Size = new Vector2(156, 156);
```

### <a name="paint-with-a-filter-effect"></a>绘制的筛选效果

[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)绘制的 CompositionEffect 输出一个区域。 可视化层中的效果可能视为可进行动画处理的筛选效果应用于源内容，例如颜色渐变、 图像、 视频、 交换链、 运行你的 UI 的区域或树的视觉对象的集合。 通常使用另一个 CompositionBrush 指定源内容。

下图和代码显示了 SpriteVisual 绘制的效果饱和度减少筛选器应用 cat 映像。

![CompositionEffectBrush](images/composition-cat-desaturated.png)

```cs
Compositor _compositor;
SpriteVisual _effectVisual;
CompositionEffectBrush _effectBrush;

_compositor = Window.Current.Compositor;

var graphicsEffect = new SaturationEffect {
                              Saturation = 0.0f,
                              Source = new CompositionEffectSourceParameter("mySource")
                         };

var effectFactory = _compositor.CreateEffectFactory(graphicsEffect);
_effectBrush = effectFactory.CreateBrush();

CompositionSurfaceBrush surfaceBrush =_compositor.CreateSurfaceBrush();
LoadedImageSurface loadedSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/cat.jpg"));
SurfaceBrush.surface = loadedSurface;

_effectBrush.SetSourceParameter("mySource", surfaceBrush);

_effectVisual = _compositor.CreateSpriteVisual();
_effectVisual.Brush = _effectBrush;
_effectVisual.Size = new Vector2(156, 156);
```

有关创建使用 CompositionBrushes 效果的详细信息，请参阅[可视化层中的效果](https://docs.microsoft.com/en-us/windows/uwp/composition/composition-effects)

### <a name="paint-with-a-compositionbrush-with-opacity-mask-applied"></a>绘制 CompositionBrush 与应用的不透明蒙板

[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)与向其应用不透明蒙板绘制 CompositionBrush 一个区域。 不透明蒙板源可以是类型 CompositionColorBrush，CompositionLinearGradientBrush、 CompositionSurfaceBrush、 CompositionEffectBrush 或 CompositionNineGridBrush 任何 CompositionBrush。 必须为 CompositionSurfaceBrush 指定不透明蒙板。

下图和代码显示了四面 CompositionMaskBrush SpriteVisual。 掩码的源是圆形的 CompositionLinearGradientBrush 这屏蔽看起来像使用图像作为掩码一个圆形。

![CompositionMaskBrush](images/composition-compositionmaskbrush.png)

```cs
Compositor _compositor;
SpriteVisual _maskVisual;
CompositionMaskBrush _maskBrush;

_compositor = Window.Current.Compositor;

_maskBrush = _compositor.CreateMaskBrush();

CompositionLinearGradientBrush _sourceGradient = _compositor.CreateLinearGradientBrush();
_sourceGradient.ColorStops.Add(_compositor.CreateColorGradientStop(0,Colors.Red));
_sourceGradient.ColorStops.Add(_compositor.CreateColorGradientStop(1,Colors.Yellow));
_maskBrush.Source = _sourceGradient;

LoadedImageSurface loadedSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/circle.png"), new Size(156.0, 156.0));
_maskBrush.Mask = _compositor.CreateSurfaceBrush(loadedSurface);

_maskVisual = _compositor.CreateSpriteVisual();
_maskVisual.Brush = _maskBrush;
_maskVisual.Size = new Vector2(156, 156);
```

### <a name="paint-with-a-compositionbrush-using-ninegrid-stretch"></a>使用 NineGrid stretch CompositionBrush 绘制

[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)绘制使用九网格隐喻拉伸 CompositionBrush 一个区域。 九网格隐喻可比其中心以不同方式拉伸边缘和 CompositionBrush 的角。 九网格拉伸的源可以通过任何 CompositionBrush 的类型 CompositionColorBrush、 CompositionSurfaceBrush 或 CompositionEffectBrush。

下面的代码演示 SpriteVisual 四面 CompositionNineGridBrush。 掩码的源是 CompositionSurfaceBrush 这拉伸使用九网格。

```cs
Compositor _compositor;
SpriteVisual _nineGridVisual;
CompositionNineGridBrush _nineGridBrush;

_compositor = Window.Current.Compositor;

_ninegridBrush = _compositor.CreateNineGridBrush();

// nineGridImage.png is 50x50 pixels; nine-grid insets, as measured relative to the actual size of the image, are: left = 1, top = 5, right = 10, bottom = 20 (in pixels)

LoadedImageSurface _imageSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/nineGridImage.png"));
_nineGridBrush.Source = _compositor.CreateSurfaceBrush(_imageSurface);

// set Nine-Grid Insets

_ninegridBrush.SetInsets(1, 5, 10, 20);

// set appropriate Stretch on SurfaceBrush for Center of Nine-Grid

sourceBrush.Stretch = CompositionStretch.Fill;

_nineGridVisual = _compositor.CreateSpriteVisual();
_nineGridVisual.Brush = _ninegridBrush;
_nineGridVisual.Size = new Vector2(100, 75);
```

### <a name="paint-using-background-pixels"></a>使用背景像素绘制

[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)绘制区域背后的内容区域。 CompositionBackdropBrush 永远不会使用其自己的但改为使用作为另一个 CompositionBrush EffectBrush 类似的输入。 例如，通过使用 CompositionBackdropBrush 作为模糊效果的输入，你可以实现毛玻璃效果。

以下代码显示了一个较小的可视化树来创建使用 CompositionSurfaceBrush 和图像上方的毛玻璃覆盖层的图像。 由放置在图像上方 EffectBrush： 装满 SpriteVisual 创建毛玻璃覆盖层。 EffectBrush 使用 CompositionBackdropBrush 作为模糊效果的输入。

```cs
Compositor _compositor;
SpriteVisual _containerVisual;
SpriteVisual _imageVisual;
SpriteVisual _backdropVisual;

_compositor = Window.Current.Compositor;

// Create container visual to host the visual tree
_containerVisual = _compositor.CreateContainerVisual();

// Create _imageVisual and add it to the bottom of the container visual.
// Paint the visual with an image.

CompositionSurfaceBrush _licoriceBrush = _compositor.CreateSurfaceBrush();

LoadedImageSurface loadedSurface = 
    LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/licorice.jpg"));
_licoriceBrush.Surface = loadedSurface;

_imageVisual = _compositor.CreateSpriteVisual();
_imageVisual.Brush = _licoriceBrush;
_imageVisual.Size = new Vector2(156, 156);
_imageVisual.Offset = new Vector3(0, 0, 0);
_containerVisual.Children.InsertAtBottom(_imageVisual)

// Create a SpriteVisual and add it to the top of the containerVisual.
// Paint the visual with an EffectBrush that applies blur to the content
// underneath the Visual to create a frosted glass effect.

GaussianBlurEffect blurEffect = new GaussianBlurEffect(){
                                    Name = "Blur",
                                    BlurAmount = 1.0f,
                                    BorderMode = EffectBorderMode.Hard,
                                    Source = new CompositionEffectSourceParameter("source");
                                    };

CompositionEffectFactory blurEffectFactory = _compositor.CreateEffectFactory(blurEffect);
CompositionEffectBrush _backdropBrush = blurEffectFactory.CreateBrush();

// Create a BackdropBrush and bind it to the EffectSourceParameter source

_backdropBrush.SetSourceParameter("source", _compositor.CreateBackdropBrush());
_backdropVisual = _compositor.CreateSpriteVisual();
_backdropVisual.Brush = _licoriceBrush;
_backdropVisual.Size = new Vector2(78, 78);
_backdropVisual.Offset = new Vector3(39, 39, 0);
_containerVisual.Children.InsertAtTop(_backdropVisual);
```

## <a name="combining-compositionbrushes"></a>组合 CompositionBrushes
大量 CompositionBrushes 使用其他 CompositionBrushes 作为输入。 例如，使用 SetSourceParameter 方法可将另一个 CompositionBrush 设置为 CompositionEffectBrush 的输入。 下表列出了 CompositionBrushes 的受支持的组合。 请注意，使用不受支持的组合将引发异常。

<table>
<tbody>
<tr>
<th>Brush</th>
<th>EffectBrush.SetSourceParameter()</th>
<th>MaskBrush.Mask</th>
<th>MaskBrush.Source</th>
<th>NineGridBrush.Source</th>
</tr>
<tr>
<td>CompositionColorBrush</td>
<td>是</td>
<td>是</td>
<td>是</td>
<td>是</td>
</tr>
<tr>
<td>CompositionLinear<br />GradientBrush</td>
<td>是</td>
<td>是</td>
<td>是</td>
<td>NO</td>
</tr>
<tr>
<td>CompositionSurfaceBrush</td>
<td>是</td>
<td>是</td>
<td>是</td>
<td>是</td>
</tr>
<tr>
<td>CompositionEffectBrush</td>
<td>否</td>
<td>否</td>
<td>是</td>
<td>NO</td>
</tr>
<tr>
<td>CompositionMaskBrush</td>
<td>否</td>
<td>否</td>
<td>否</td>
<td>否</td>
</tr>
<tr>
<td>CompositionNineGridBrush</td>
<td>是</td>
<td>是</td>
<td>是</td>
<td>NO</td>
</tr>
<tr>
<td>CompositionBackdropBrush</td>
<td>是</td>
<td>否</td>
<td>否</td>
<td>否</td>
</tr>
</tbody>
</table>


## <a name="using-a-xaml-brush-vs-compositionbrush"></a>使用 XAML 画笔与 CompositionBrush

下表列出了方案和 XAML 或合成画笔使用绘制 UIElement 或应用程序中的 SpriteVisual 时是否预定义。 

> [!NOTE]
> 如果 CompositionBrush XAML UIElement 的建议，它将假定使用 XamlCompositionBrushBase 打包 CompositionBrush。

|情形                                                                   | XAML UIElement                                                                                                |合成 SpriteVisual
|---------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------|----------------------------------
|使用纯色绘制区域                                             |[SolidColorBrush](https://msdn.microsoft.com/library/windows/apps/BR242962)                                |[CompositionColorBrush](https://msdn.microsoft.com/library/windows/apps/Mt589399)
|使用动画效果的颜色绘制区域                                          |[SolidColorBrush](https://msdn.microsoft.com/library/windows/apps/BR242962)                                |[CompositionColorBrush](https://msdn.microsoft.com/library/windows/apps/Mt589399)
|使用静态的渐变绘制区域                                       |[LinearGradientBrush](https://msdn.microsoft.com/library/windows/apps/BR210108)                            |[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)
|使用动画的梯度停止点绘制区域                                 |[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)                                                                                 |[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)
|使用图像绘制区域                                                |[ImageBrush](https://msdn.microsoft.com/library/windows/apps/BR210101)                                     |[CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415)
|网页使用绘制区域                                               |[WebViewBrush](https://msdn.microsoft.com/library/windows/apps/BR227703)                                   |不适用
|与图像使用 NineGrid stretch 绘制某个区域                         |[图像控件](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image)                   |[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)
|使用动画 NineGrid stretch 绘制区域                               |[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)                                                                                       |[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)
|使用 swapchain 绘制区域                                             |[SwapChainPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel)                                                                                                 |[CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415)带 swapchain 互操作
|使用视频绘制区域                                                 |[MediaElement](https://msdn.microsoft.com/library/windows/apps/mt187272.aspx)                                                                                                  |[CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415)带媒体互操作
|使用自定义 2D 绘图绘制区域                                       |从 Win2D [CanvasControl](http://microsoft.github.io/Win2D/html/T_Microsoft_Graphics_Canvas_UI_Xaml_CanvasControl.htm)                                                                                                 |[CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415)带 Win2D 互操作
|使用非动画掩码绘制区域                                       |使用 XAML[形状](https://docs.microsoft.com/windows/uwp/graphics/drawing-shapes)定义蒙板   |[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)
|使用动画遮罩绘制区域                                        |[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)                                                                                           |[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)
|使用动画的筛选效果绘制区域                               |[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)                                                                                         |[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)
|带有效果应用到后台像素绘制某个区域        |[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)                                                                                        |[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)

## <a name="related-topics"></a>相关主题

[合成本机 DirectX 和 Direct2D 互操作带有 BeginDraw 和 EndDraw](composition-native-interop.md)

[XamlCompositionBrushBase 与 XAML 画笔互操作](/windows/uwp/design/style/brushes#xamlcompositionbrushbase)
