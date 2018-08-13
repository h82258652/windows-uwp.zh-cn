---
author: jwmsft
ms.assetid: 03dd256f-78c0-e1b1-3d9f-7b3afab29b2f
title: 合成画笔
description: 画笔通过其输出绘制 Visual 的区域。 不同的画笔具有不同的输出类型。
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 103ecd24c35d75d3ea1d305d7294048dc628d2e2
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/13/2018
ms.locfileid: "1038567"
---
# <a name="composition-brushes"></a>合成画笔
从 UWP 应用程序屏幕上看到所有是可见的因为它由画笔绘制。 画笔使您能够绘制图像或复杂的效果链绘图介于简单、 纯色颜色的内容的用户界面 (UI) 对象。 本主题介绍与 CompositionBrush 绘制的概念。

请注意，当使用 XAML UWP 应用程序，您可以选择绘制[XAML 画笔](/windows/uwp/design/style/brushes)或[CompositionBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBrush)UIElement。 通常情况下，会更简单，建议您选择 XAML 画笔，如果您的方案支持的 XAML 画笔。 例如，动画一个按钮，更改文本或图像与形状的填充的颜色。 另一方面，如果您试图执行某些不支持与动画的掩码或动画效果的九网格拉伸或效果链 painting 像 XAML 画笔操作，您可以使用 CompositionBrush 绘制使用[UIElementXamlCompositionBrushBase](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase)。

可视化层处理时，必须使用 CompositionBrush 来绘制[SpriteVisual](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.SpriteVisual)的区域。

-   [必备条件](./composition-brushes.md#prerequisites)
-   [使用 CompositionBrush 绘制](./composition-brushes.md#paint-with-a-compositionbrush)
    -   [使用纯色绘制](./composition-brushes.md#paint-with-a-solid-color)
    -   [使用线性渐变绘制](./composition-brushes.md#paint-with-a-linear-gradient)
    -   [用图像绘制](./composition-brushes.md#paint-with-an-image)
    -   [绘制具有自定义绘图](./composition-brushes.md#paint-with-a-custom-drawing)
    -   [绘制了视频](./composition-brushes.md#paint-with-a-video)
    -   [绘制具有筛选效果](./composition-brushes.md#paint-with-a-filter-effect)
    -   [绘制具有与不透明度掩码 CompositionBrush](./composition-brushes.md#paint-with-a-compositionbrush-with-opacity-mask-applied)
    -   [与使用 NineGrid 拉伸 CompositionBrush 绘制](./composition-brushes.md#paint-with-a-compositionbrush-using-ninegrid-stretch)
    -   [使用背景像素绘制](./composition-brushes.md#paint-using-background-pixels)
-   [组合 CompositionBrushes](./composition-brushes.md#combining-compositionbrushes)
-   [使用 XAML 画笔与 CompositionBrush](./composition-brushes.md#using-a-xaml-brush-vs-compositionbrush)
-   [相关主题](./composition-brushes.md#related-topics)

## <a name="prerequisites"></a>必备条件
本概述假定您已经熟悉基本撰写应用程序的结构[可视化层概述](visual-layer.md)中所述。

## <a name="paint-with-a-compositionbrush"></a>使用 CompositionBrush 绘制

[CompositionBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBrush) "复制"带有其输出的区域。 不同的画笔具有不同的输出类型。 某些画笔绘制带有纯色，其他人与渐变、 图像、 自定义绘图或效果的区域。 还有修改其他画笔的行为的专用的画笔。 例如，不透明度掩码可用于的控制通过 CompositionBrush 绘制的区域或九网格可用于控制绘制区域时应用于 CompositionBrush 拉伸。 CompositionBrush 可以是下列类型之一：

|类                                   |详细信息                                         |中引入|
|-------------------------------------|---------------------------------------------------------|--------------------------------------|
|[CompositionColorBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionColorBrush)         |使用纯色绘制一个区域                        |Windows 10 年 11 月更新 (SDK 10586)|
|[CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush)       |绘制的[ICompositionSurface](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Composition.ICompositionSurface)内容区域|Windows 10 年 11 月更新 (SDK 10586)|
|[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)        |绘制的合成效果的内容区域 |Windows 10 年 11 月更新 (SDK 10586)|
|[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)          |绘制具有与不透明度掩码 CompositionBrush visual |Windows 10 周年日更新 (SDK 14393)
|[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)      |与使用 NineGrid 拉伸 CompositionBrush 绘制区域 |Windows 10 周年日更新 SDK (14393)
|[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)|使用线性渐变绘制一个区域                    |Windows 10 属于创建者更新 (内幕 Preview SDK)
|[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)     |通过采样从是应用程序的背景像素或直接后面的应用程序窗口在桌面上像素绘制一个区域。 用作 CompositionEffectBrush 类似的另一个 CompositionBrush 输入 | Windows 10 周年日更新 (SDK 14393)

### <a name="paint-with-a-solid-color"></a>使用纯色绘制

[CompositionColorBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionColorBrush)使用绘制区域纯色。 有多种方式指定 SolidColorBrush 的颜色。 例如，可以指定其 alpha、 红色、 蓝色和绿色 (ARGB) 通道，或使用一种[颜色](https://docs.microsoft.com/uwp/api/windows.ui.colors)类提供的预定义颜色。

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

[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)绘制区域使用线性渐变。 线性渐变跨线条，渐变轴混合两个或多个颜色。 GradientStop 对象用于指定渐变和它们的位置中的颜色。

下面的图和代码显示了与使用红色和黄色颜色 2 停止使用 LinearGradientBrush 绘制 SpriteVisual。

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

### <a name="paint-with-an-image"></a>用图像绘制

[CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush)像素呈现到 ICompositionSurface 绘制区域。 例如，可以使用 CompositionSurfaceBrush 来用图像呈现拖到使用[LoadedImageSurface](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.loadedimagesurface) API ICompositionSurface 面上绘制的区域。

下面的图和代码显示 SpriteVisual 绘制与呈现在使用 LoadedImageSurface ICompositionSurface licorice 的位图。 CompositionSurfaceBrush 属性可用于伸展以及对齐位图的可视边界内。

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

### <a name="paint-with-a-custom-drawing"></a>绘制具有自定义绘图
[CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush)还可用于使用像素使用[Win2D](http://microsoft.github.io/Win2D/html/Introduction.htm) （或 D2D） 呈现 ICompositionSurface 绘制区域。

下面的代码演示 SpriteVisual 绘制文本运行呈现到 ICompositionSurface 与使用 Win2D。 请注意，若要使用的 Win2D 您需要在您的项目中包括的[Win2D NuGet](http://www.nuget.org/packages/Win2D.uwp)程序包。

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

同样，CompositionSurfaceBrush，还可以使用绘制与使用 Win2D 互操作 SwapChain SpriteVisual。 [此示例](https://github.com/Microsoft/Win2D-Samples/tree/master/CompositionExample)提供了如何使用 Win2D 绘制与 swapchain SpriteVisual 的示例。

### <a name="paint-with-a-video"></a>绘制了视频
[CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush)还可用于呈现使用通过[MediaPlayer](https://docs.microsoft.com/en-us/uwp/api/Windows.Media.Playback.MediaPlayer)类加载视频 ICompositionSurface 像素使用绘制区域。

下面的代码演示 SpriteVisual 绘制与加载到 ICompositionSurface 上的视频。

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

### <a name="paint-with-a-filter-effect"></a>绘制具有筛选效果

[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)使用绘制区域的 CompositionEffect 输出。 Visual 层中的效果可能视为动画处理筛选效果应用于如颜色、 渐变、 图像、 视频、 swapchains、 区域的 UI 或树的直观表示形式的源内容的集合。 通常使用另一个 CompositionBrush 指定源内容。

下面的图和代码显示了带有映像的饱和度减少应用的筛选器效果 cat 绘制 SpriteVisual。

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

有关创建使用 CompositionBrushes 效果的详细信息，请参阅[Visual 层中的效果](https://docs.microsoft.com/en-us/windows/uwp/composition/composition-effects)

### <a name="paint-with-a-compositionbrush-with-opacity-mask-applied"></a>绘制具有 CompositionBrush 应用的不透明度掩码

[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)使用绘制区域 CompositionBrush，其应用于不透明度掩码。 不透明度掩码的源可以是类型 CompositionColorBrush，CompositionLinearGradientBrush、 CompositionSurfaceBrush、 CompositionEffectBrush 或 CompositionNineGridBrush 任何 CompositionBrush。 必须为 CompositionSurfaceBrush 指定的不透明度掩码。

下面的图和代码显示了使用 CompositionMaskBrush 绘制 SpriteVisual。 掩码的源是圆的 CompositionLinearGradientBrush 其屏蔽看起来像一个使用作为掩码的图像的圆。

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

### <a name="paint-with-a-compositionbrush-using-ninegrid-stretch"></a>与使用 NineGrid 拉伸 CompositionBrush 绘制

[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)绘制伸展使用的九网格隐喻 CompositionBrush 区域。 九网格比拟使您能够比其中心的不同拉伸边缘和 CompositionBrush 的角。 九网格拉伸的源可以由任何类型 CompositionColorBrush 的 CompositionBrush、 CompositionSurfaceBrush 或 CompositionEffectBrush。

下面的代码演示如何使用 CompositionNineGridBrush 绘制 SpriteVisual 掩码的源是 CompositionSurfaceBrush 其伸展使用九网格。

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

[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)绘制区域后面的内容区域。 CompositionBackdropBrush 从未使用其自己的但改为用作 EffectBrush 类似的另一个 CompositionBrush 的输入。 例如，可以通过使用 CompositionBackdropBrush 作为模糊效果的输入，获得毛玻璃效果。

下面的代码显示了小型的可视树创建使用 CompositionSurfaceBrush 和图像上方毛玻璃覆盖图像。 通过将填充图像上方 EffectBrush SpriteVisual 创建毛玻璃覆盖。 EffectBrush 使用 CompositionBackdropBrush 作为模糊效果的输入。

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
CompositionBrushes 大量使用其他 CompositionBrushes 作为输入。 例如，使用 SetSourceParameter 方法可设置另一个 CompositionBrush 作为 CompositionEffectBrush 的输入。 下表列出了支持的 CompositionBrushes 的组合。 请注意，使用不受支持的组合将引发异常。

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

下表提供方案和 XAML 或合成画笔使用绘制 UIElement 或应用程序中的 SpriteVisual 时是否预定义的列表。 

> [!NOTE]
> 如果 CompositionBrush XAML UIElement 的建议，则假定使用 XamlCompositionBrushBase 打包 CompositionBrush。

|情形                                                                   | XAML UIElement                                                                                                |合成 SpriteVisual
|---------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------|----------------------------------
|使用纯色绘制区域                                             |[SolidColorBrush](https://msdn.microsoft.com/library/windows/apps/BR242962)                                |[CompositionColorBrush](https://msdn.microsoft.com/library/windows/apps/Mt589399)
|使用动画效果的颜色绘制区域                                          |[SolidColorBrush](https://msdn.microsoft.com/library/windows/apps/BR242962)                                |[CompositionColorBrush](https://msdn.microsoft.com/library/windows/apps/Mt589399)
|使用静态渐变绘制区域                                       |[LinearGradientBrush](https://msdn.microsoft.com/library/windows/apps/BR210108)                            |[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)
|使用动画渐变光圈绘制区域                                 |[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)                                                                                 |[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)
|使用图像绘制区域                                                |[ImageBrush](https://msdn.microsoft.com/library/windows/apps/BR210101)                                     |[CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415)
|使用网页绘制区域                                               |[WebViewBrush](https://msdn.microsoft.com/library/windows/apps/BR227703)                                   |不适用
|用图像使用 NineGrid 拉伸绘制区域                         |[图像控件](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image)                   |[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)
|使用动画 NineGrid 拉伸绘制区域                               |[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)                                                                                       |[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)
|使用 swapchain 绘制区域                                             |[SwapChainPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel)                                                                                                 |[CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415)具有 swapchain 互操作
|使用视频绘制区域                                                 |[MediaElement](https://msdn.microsoft.com/library/windows/apps/mt187272.aspx)                                                                                                  |[CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415)具有媒体互操作
|使用自定义 2D 绘图绘制区域                                       |从 Win2D [CanvasControl](http://microsoft.github.io/Win2D/html/T_Microsoft_Graphics_Canvas_UI_Xaml_CanvasControl.htm)                                                                                                 |[CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415)具有 Win2D 互操作
|使用非动画掩码绘制区域                                       |使用 XAML[形状](https://docs.microsoft.com/windows/uwp/graphics/drawing-shapes)定义掩码   |[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)
|使用动画掩码绘制区域                                        |[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)                                                                                           |[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)
|使用动画的筛选效果绘制区域                               |[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)                                                                                         |[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)
|带有效果应用于背景像素绘制区域        |[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)                                                                                        |[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)

## <a name="related-topics"></a>相关主题

[合成本机 DirectX 和 Direct2D 互操作 BeginDraw 与 EndDraw](composition-native-interop.md)

[与 XamlCompositionBrushBase XAML 画笔互操作](/windows/uwp/design/style/brushes#xamlcompositionbrushbase)
