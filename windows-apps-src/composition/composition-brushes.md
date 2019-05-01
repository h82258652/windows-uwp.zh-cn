---
ms.assetid: 03dd256f-78c0-e1b1-3d9f-7b3afab29b2f
title: 合成画笔
description: 画笔通过其输出绘制 Visual 的区域。 不同的画笔具有不同的输出类型。
ms.date: 04/19/2019
ms.topic: article
ms.custom: 19H1
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d51bc945c721ae15889dece8f84959f9a78192bc
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2019
ms.locfileid: "63790541"
---
# <a name="composition-brushes"></a>合成画笔
屏幕上的 UWP 应用中可见的所有内容均可见，因为它是通过画笔绘制。 你可以通过画笔绘制用户界面 (UI) 对象，内容可以为简单纯色图像或绘图到复杂的效果链。 本主题介绍使用 CompositionBrush 绘制的概念。

请注意，使用 XAML UWP 应用时，你可以选择使用 [XAML 画笔](/windows/uwp/design/style/brushes)或 [CompositionBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBrush) 绘制 UIElement。 一般来说，如果你的方案支持 XAML 画笔，则建议你选择 XAML 画笔，这样更加容易。 例如，设置按钮颜色动画、使用图像更改文本或图形填充。 另一方面，如果想要执行不支持像使用动画的掩码或动画的九网格拉伸或效果链绘制 XAML 画笔的某些内容，您可以使用 CompositionBrush 绘制使用 UIElement [XamlCompositionBrushBase](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase)。

操作可视化层时，必须使用 CompositionBrush 绘制 [SpriteVisual](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.SpriteVisual) 区域。

-   [必备条件](./composition-brushes.md#prerequisites)
-   [使用 CompositionBrush 进行绘制](./composition-brushes.md#paint-with-a-compositionbrush)
    -   [使用纯色绘制](./composition-brushes.md#paint-with-a-solid-color)
    -   [绘制带有线性渐变](./composition-brushes.md#paint-with-a-linear-gradient) 
    -   [使用径向渐变绘制](./composition-brushes.md#paint-with-a-radial-gradient)
    -   [使用图像绘制](./composition-brushes.md#paint-with-an-image)
    -   [绘制自定义绘图](./composition-brushes.md#paint-with-a-custom-drawing)
    -   [使用视频绘制](./composition-brushes.md#paint-with-a-video)
    -   [绘制具有筛选器效果](./composition-brushes.md#paint-with-a-filter-effect)
    -   [用使用不透明蒙板 CompositionBrush 绘图](./composition-brushes.md#paint-with-a-compositionbrush-with-opacity-mask-applied)
    -   [用使用 NineGrid stretch CompositionBrush 绘图](./composition-brushes.md#paint-with-a-compositionbrush-using-ninegrid-stretch)
    -   [使用背景像素为单位进行绘制](./composition-brushes.md#paint-using-background-pixels)
-   [组合 CompositionBrushes](./composition-brushes.md#combining-compositionbrushes)
-   [使用 XAML 画笔 vs。CompositionBrush](./composition-brushes.md#using-a-xaml-brush-vs-compositionbrush)
-   [相关的主题](./composition-brushes.md#related-topics)

## <a name="prerequisites"></a>先决条件
本概述假定你已熟悉基本合成应用程序的结构，如[可视化层概述](visual-layer.md)中所述。

## <a name="paint-with-a-compositionbrush"></a>使用 CompositionBrush 绘制

[CompositionBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBrush)通过输出“绘制”区域。 不同的画笔具有不同的输出类型。 某些画笔使用纯色绘制区域，其他的画笔可以使用渐变、图像、自定义绘图或效果绘制区域。 另外还有用于修改其他画笔行为的专用画笔。 例如，绘制区域时，可以使用不透明蒙板控制使用 CompositionBrush 绘制的区域，或者使用九网格控制应用到 CompositionBrush 的延伸。 CompositionBrush 可以是以下类型之一：

|类                                   |详细信息                                         |在  中引入|
|-------------------------------------|---------------------------------------------------------|--------------------------------------|
|[CompositionColorBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionColorBrush)         |使用纯色绘制区域                        |Windows 10 版本 1511 (SDK 10586)|
|[CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush)       |使用 [ICompositionSurface](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Composition.ICompositionSurface) 内容绘制区域|Windows 10 版本 1511 (SDK 10586)|
|[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)        |使用合成效果内容绘制区域 |Windows 10 版本 1511 (SDK 10586)|
|[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)          |使用带不透明蒙板的 CompositionBrush 绘制视觉对象 |Windows 10，版本 1607 (SDK 14393)
|[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)      |使用九网格拉伸的 CompositionBrush 绘制区域 |Windows 10，版本 1607 (SDK 14393)
|[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)|使用线性渐变绘制区域                    |Windows 10 版本 1709 (SDK 版本 16299)
|[CompositionRadialGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionradialgradientbrush)|使用径向渐变绘制区域                    |Windows 10，版本 1903 (Insider Preview SDK)
|[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)     |通过对应用程序的背景像素或桌面上应用程序窗口正后方的像素采用来绘制区域。 用作其他 CompositionBrush（如 CompositionEffectBrush）的输入 | Windows 10，版本 1607 (SDK 14393)

### <a name="paint-with-a-solid-color"></a>使用纯色绘制

[CompositionColorBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionColorBrush) 使用纯色绘制区域。 可以通过多种方式指定 SolidColorBrush 颜色。 例如，可以指定其 alpha、红色、蓝色和绿色 (ARGB) 通道，或者使用[颜色](https://docs.microsoft.com/uwp/api/windows.ui.colors)类中提供的其中一个预定义颜色。

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

[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush) 使用线性渐变绘制区域。 线性渐变在线条（渐变轴）中混合了两种或多种颜色。 你可以使用 GradientStop 对象指定渐变的颜色及其位置。

下图和代码所示为使用红黄 2 种起止颜色的 LinearGradientBrush 绘制的 SpriteVisual。

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

### <a name="paint-with-a-radial-gradient"></a>使用径向渐变绘制

一个[CompositionRadialGradientBrush](/uwp/api/windows.ui.composition.compositionradialgradientbrush)使用径向渐变绘制区域。 径向渐变的渐变，从该椭圆的中心开始和结束时间椭圆的半径与混合两个或多个颜色。 GradientStop 对象用于在渐变中定义的颜色和它们的位置。

下图和代码演示使用具有 2 个 GradientStops RadialGradientBrush 绘制 SpriteVisual。

![CompositionRadialGradientBrush](images/radial-gradient-brush.png)

```cs
Compositor _compositor;
SpriteVisual _gradientVisual;
CompositionRadialGradientBrush RGBrush;

_compositor = Window.Current.Compositor;

RGBrush = _compositor.CreateRadialGradientBrush();
RGBrush.ColorStops.Add(_compositor.CreateColorGradientStop(0, Colors.Aquamarine));
RGBrush.ColorStops.Add(_compositor.CreateColorGradientStop(1, Colors.DeepPink));
_gradientVisual = _compositor.CreateSpriteVisual();
_gradientVisual.Brush = RGBrush;
_gradientVisual.Size = new Vector2(200, 200);
```

### <a name="paint-with-an-image"></a>使用图像绘制

[CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) 使用 ICompositionSurface 上渲染的像素绘制区域。 例如，CompositionSurfaceBrush 可用于使用 [LoadedImageSurface](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.loadedimagesurface) API 通过 ICompositionSurface 上渲染的图像绘制区域。

下图和代码所示为使用 LoadedImageSurface 通过 ICompositionSurface 上渲染的 licorice 位图绘制的 SpriteVisual。 CompositionSurfaceBrush 的属性可用于拉伸和对齐可视边界内的位图。

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

### <a name="paint-with-a-custom-drawing"></a>使用自定义绘图绘制
[CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) 也可用于使用 [Win2D](https://microsoft.github.io/Win2D/html/Introduction.htm)（或 D2D）渲染的 ICompositionSurface 中的像素绘制区域。

下图和代码所示为通过 Win2D 使用 ICompositionSurface 上渲染的文本运行绘制的 SpriteVisual。 请注意，若要使用 Win2D，你需要在项目中包括 [Win2D NuGet](https://www.nuget.org/packages/Win2D.uwp) 程序包。

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

同样，CompositionSurfaceBrush 也可用于使用 Win2D 互操作通过 SwapChain 绘制 SpriteVisual。 [本示例](https://github.com/Microsoft/Win2D-Samples/tree/master/CompositionExample)提供如何使用 Win2D 绘制具有交换链的 SpriteVisual。

### <a name="paint-with-a-video"></a>使用视频绘制
[CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush)也可用于使用通过 [MediaPlayer](https://docs.microsoft.com/en-us/uwp/api/Windows.Media.Playback.MediaPlayer) 类加载的视频渲染的 ICompositionSurface 中的像素绘制区域。

下图和代码所示为使用加载到 ICompositionSurface 的视频绘制的 SpriteVisual。

```cs
Compositor _compositor;
SpriteVisual _videoVisual;
CompositionSurfaceBrush _videoBrush;

// MediaPlayer set up with a create from URI

_mediaPlayer = new MediaPlayer();

// Get a source from a URI. This could also be from a file via a picker or a stream
var source = MediaSource.CreateFromUri(new Uri("https://go.microsoft.com/fwlink/?LinkID=809007&clcid=0x409"));
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

### <a name="paint-with-a-filter-effect"></a>使用过滤效果绘制

[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush) 使用 CompositionEffect 输出绘制区域。 可视化层中的效果可以制作应用到源内容集合的可进行动画设置的过滤效果，如颜色、渐变、图像、视频、交换链、UI 区域或视觉对象树。 源内容通常使用其他 CompositionBrush 指定。

下图和代码所示为使用已应用去饱和度过滤效果的猫图像绘制的 SpriteVisual。

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

有关使用 CompositionBrush 创建效果的详细信息，请参阅[可视化层中的效果](https://docs.microsoft.com/en-us/windows/uwp/composition/composition-effects)

### <a name="paint-with-a-compositionbrush-with-opacity-mask-applied"></a>使用已应用不透明蒙板的 CompositionBrush 绘制

[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush) 使用已应用不透明度蒙板的 CompositionBrush 绘制区域。 不透明度蒙板源可以为 CompositionColorBrush、CompositionLinearGradientBrush、CompositionSurfaceBrush、CompositionEffectBrush 或 CompositionNineGridBrush 类型的任何 CompositionBrush。 不透明度蒙板必须指定为 CompositionSurfaceBrush。

下图和代码所示为使用 CompositionMaskBrush 绘制的 SpriteVisual。 蒙板源为 CompositionLinearGradientBrush，蒙上后的效果如同使用圆圈图像的圆圈作为蒙板。

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

### <a name="paint-with-a-compositionbrush-using-ninegrid-stretch"></a>使用九网格拉伸的 CompositionBrush 绘制

[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush) 使用通过九网格形式拉伸的 CompositionBrush 绘制区域。 九网格形式支持将 CompositionBrush 的边缘和角拉伸成与其中心不同的形状。 九网格拉伸源可以是 CompositionColorBrush、CompositionSurfaceBrush 或 CompositionEffectBrush 类型的任何 CompositionBrush。

以下代码所示为使用 CompositionNineGridBrush 绘制的 SpriteVisual。 蒙板源为使用九网格拉伸的 CompositionSurfaceBrush。

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

[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush) 使用区域后的内容绘制区域。 CompositionBackdropBrush 不可单独使用，但可用作其他 CompositionBrush（如 EffectBrush）的输入。 例如，通过将 CompositionBackdropBrush 用作模糊效果的输入，你可以实现霜冻玻璃效果。

以下代码所示为一个较小的可视化树，用于创建一个使用 CompositionSurfaceBrush 的图像并在该图像上覆盖霜冻的玻璃。 覆盖的霜冻玻璃通过在图像上放入具有 EffectBrush 填充效果的 SpriteVisual 创建。 EffectBrush 使用 CompositionBackdropBrush 作为模糊效果的输入。

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

## <a name="combining-compositionbrushes"></a>组合 CompositionBrush
许多 CompositionBrushes 使用其他 CompositionBrushes 作为输入。 例如，使用 SetSourceParameter 方法可以将其他 CompositionBrush 设为 CompositionEffectBrush 的输入。 下表列出了受支持的 CompositionBrush 组合。 请注意，使用不受支持的组合将会引发异常。

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
<td>否</td>
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
<td>否</td>
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
<td>否</td>
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


## <a name="using-a-xaml-brush-vs-compositionbrush"></a>使用 XAML 画笔 vs。CompositionBrush

下表提供了方案列表以及在应用程序中绘制 UIElement 或 SpriteVisual 时是规定使用 XAML 画笔还是合成画笔。 

> [!NOTE]
> 如果建议为 XAML UIElement 使用 CompositionBrush，则假定 CompositionBrush 是使用 XamlCompositionBrushBase 打包。

|应用场景                                                                   | XAML UIElement                                                                                                |合成 SpriteVisual
|---------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------|----------------------------------
|使用纯色绘制区域                                             |[SolidColorBrush](https://msdn.microsoft.com/library/windows/apps/BR242962)                                |[CompositionColorBrush](https://msdn.microsoft.com/library/windows/apps/Mt589399)
|使用动画颜色绘制区域                                          |[SolidColorBrush](https://msdn.microsoft.com/library/windows/apps/BR242962)                                |[CompositionColorBrush](https://msdn.microsoft.com/library/windows/apps/Mt589399)
|使用静态渐变绘制区域                                       |[LinearGradientBrush](https://msdn.microsoft.com/library/windows/apps/BR210108)                            |[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)
|使用动画渐变起止颜色绘制区域                                 |[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)                                                                                 |[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)
|使用图像绘制区域                                                |[ImageBrush](https://msdn.microsoft.com/library/windows/apps/BR210101)                                     |[CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415)
|使用网页绘制区域                                               |[WebViewBrush](https://msdn.microsoft.com/library/windows/apps/BR227703)                                   |不可用
|使用九网格拉伸的图像绘制区域                         |[图像控件](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image)                   |[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)
|使用动画九网格拉伸绘制区域                               |[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)                                                                                       |[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)
|使用交换链绘制区域                                             |[SwapChainPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel)                                                                                                 |带交换链互操作的 [CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415)
|使用视频绘制区域                                                 |[MediaElement](https://msdn.microsoft.com/library/windows/apps/mt187272.aspx)                                                                                                  |带媒体互操作的 [CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415)
|使用自定义 2D 绘图绘制区域                                       |Win2D 中的 [CanvasControl](https://microsoft.github.io/Win2D/html/T_Microsoft_Graphics_Canvas_UI_Xaml_CanvasControl.htm)                                                                                                 |带 Win2D 互操作的 [CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415)
|使用非动画蒙板绘制区域                                       |使用 XAML [形状](https://docs.microsoft.com/windows/uwp/graphics/drawing-shapes)定义蒙板   |[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)
|使用动画蒙板绘制区域                                        |[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)                                                                                           |[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)
|使用动画过滤效果绘制区域                               |[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)                                                                                         |[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)
|使用应用于背景像素的效果绘制区域        |[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)                                                                                        |[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)

## <a name="related-topics"></a>相关主题

[组合本机 DirectX 和 Direct2D 与互操作 BeginDraw 和 EndDraw](composition-native-interop.md)

[XAML 与 XamlCompositionBrushBase 画笔进行互操作](/windows/uwp/design/style/brushes#xamlcompositionbrushbase)
