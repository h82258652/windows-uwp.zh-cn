---
title: 组合阴影
description: 卷影 Api，可以将动态的可自定义阴影添加到 UI 内容。
ms.date: 07/16/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9541ea1c00d473bc4881a80d8597625592e278f9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57630832"
---
# <a name="shadows-in-windows-ui"></a>Windows UI 中的阴影

[DropShadow](/uwp/api/Windows.UI.Composition.DropShadow)类提供方法来创建可应用于可配置卷影[SpriteVisual](/uwp/api/windows.ui.composition.spritevisual)或[LayerVisual](/uwp/api/windows.ui.composition.layervisual) （视觉对象的子树）。 由于是按照惯例，可视化层中的对象，可以使用 CompositionAnimations 动画 DropShadow 的所有属性。

## <a name="basic-drop-shadow"></a>基本的投影

若要创建基本卷影，只需创建新 DropShadow，并将其关联到你的视觉对象。 阴影是默认情况下矩形。 一组标准属性是可用于调整你阴影的外观。

```cs
var basicRectVisual = _compositor.CreateSpriteVisual();
basicRectVisual.Brush = _compositor.CreateColorBrush(Colors.Blue);
basicRectVisual.Offset = new Vector3(100, 100, 20);
basicRectVisual.Size = new Vector2(300, 300);

var basicShadow = _compositor.CreateDropShadow();
basicShadow.BlurRadius = 25f;
basicShadow.Offset = new Vector3(20, 20, 20);

basicRectVisual.Shadow = basicShadow;
```

![基本 DropShadow 矩形视觉对象](images/rectangular-dropshadow.png)

## <a name="shaping-the-shadow"></a>调整卷影

有几种方法来为你 DropShadow 定义形状：

- **使用默认**-默认情况下 DropShadow 形状定义的 CompositionDropShadowSourcePolicy 上的默认模式。 默认值为矩形 SpriteVisual，除非提供掩码。 LayerVisual，默认值为继承使用视觉对象的画笔的 alpha 的掩码。
- **设置掩码**– 您可以设置[掩码](/uwp/api/windows.ui.composition.dropshadow.mask)属性来定义阴影不透明蒙板。
- **指定要使用继承掩码**-设置[SourcePolicy](/uwp/api/windows.ui.composition.dropshadow.sourcepolicy)属性，以使用[CompositionDropShadowSourcePolicy](/uwp/api/windows.ui.composition.compositiondropshadowsourcepolicy)。 若要使用生成从视觉对象的画笔的 alpha 的掩码的 InheritFromVisualContent。

## <a name="masking-to-match-your-content"></a>屏蔽以匹配你的内容

如果你想要其匹配视觉对象的内容将卷影可以为卷影掩码属性，使用视觉对象的画笔或设置要将内容从自动继承掩码的阴影。 如果使用 LayerVisual，阴影将继承默认情况下的掩码。

```cs
var imageSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/myImage.png"));
var imageBrush = _compositor.CreateSurfaceBrush(imageSurface);

var imageSpriteVisual = _compositor.CreateSpriteVisual();
imageSpriteVisual.Size = new Vector2(400,400);
imageSpriteVisual.Offset = new Vector3(100, 500, 20);
imageSpriteVisual.Brush = imageBrush;

var shadow = _compositor.CreateDropShadow();
shadow.Mask = imageBrush;
// or use shadow.SourcePolicy = CompositionDropShadowSourcePolicy.InheritFromVisualContent;
shadow.BlurRadius = 25f;
shadow.Offset = new Vector3(20, 20, 20);

imageSpriteVisual.Shadow = shadow;
```

![连接的 web 图像带掩码的投影](images/ms-brand-web-dropshadow.png)

## <a name="using-an-alternative-mask"></a>使用替代掩码

在某些情况下，你可能想要调整阴影，以便它与视觉对象的内容不匹配。 若要实现此效果，您将需要使用具有 alpha 的画笔将掩码属性显式设置。

在以下示例中，我们加载两个面-一个用于影像内容，一个用于卷影掩码：

```cs
var imageSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/myImage.png"));
var imageBrush = _compositor.CreateSurfaceBrush(imageSurface);

var circleSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/myCircleImage.png"));
var customMask = _compositor.CreateSurfaceBrush(circleSurface);

var imageSpriteVisual = _compositor.CreateSpriteVisual();
imageSpriteVisual.Size = new Vector2(400,400);
imageSpriteVisual.Offset = new Vector3(100, 500, 20);
imageSpriteVisual.Brush = imageBrush;

var shadow = _compositor.CreateDropShadow();
shadow.Mask = customMask;
shadow.BlurRadius = 25f;
shadow.Offset = new Vector3(20, 20, 20);

imageSpriteVisual.Shadow = shadow;
```

![带圆圈的连接的 web 映像屏蔽投影](images/ms-brand-web-masked-dropshadow.png)

## <a name="animating"></a>对进行动画处理

由于是标准可视化层中，可以使用组合动画动画 DropShadow 属性。 下面，我们修改上面的阴影的模糊半径进行动画处理的少量示例中的代码。

```cs
ScalarKeyFrameAnimation blurAnimation = _compositor.CreateScalarKeyFrameAnimation();
blurAnimation.InsertKeyFrame(0.0f, 25.0f);
blurAnimation.InsertKeyFrame(0.7f, 50.0f);
blurAnimation.InsertKeyFrame(1.0f, 25.0f);
blurAnimation.Duration = TimeSpan.FromSeconds(4);
blurAnimation.IterationBehavior = AnimationIterationBehavior.Forever;
shadow.StartAnimation("BlurRadius", blurAnimation);
```

## <a name="shadows-in-xaml"></a>XAML 中的阴影

如果你想要添加到更复杂的框架元素的阴影，有几种与阴影 XAML 和组合之间的互操作的方法：

1. 使用[DropShadowPanel](https://github.com/Microsoft/UWPCommunityToolkit/blob/master/Microsoft.Toolkit.Uwp.UI.Controls/DropShadowPanel/DropShadowPanel.Properties.cs) Windows 社区工具包中提供。 请参阅[DropShadowPanel 文档](https://docs.microsoft.com/windows/uwpcommunitytoolkit/controls/DropShadowPanel)有关如何使用它的详细信息。
1. 创建视觉对象使用与卷影主机环境并将其绑定到 XAML 讲义视觉对象。
1. 使用组合示例库[SamplesCommon](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SamplesCommon/SamplesCommon) CompositionShadow 的自定义控件。 请参阅此处的使用情况的示例。

## <a name="performance"></a>性能

尽管可视化层具有很多优化到位，以使效果有效且可用，但生成阴影可以是一个相对高昂的操作，具体取决于您设置有哪些选项。 下面是高级别成本的各种阴影。 请注意，尽管某些阴影可能开销很大，但它们仍可能适合在某些情况下尽量少使用。

卷影特征| 开销
------------- | -------------
“矩形”    | 低
Shadow.Mask      | 高
CompositionDropShadowSourcePolicy.InheritFromVisualContent | 高
静态模糊半径 | 低
对模糊半径进行动画处理 | 高

## <a name="additional-resources"></a>其他资源

- [组合 DropShadow API](/uwp/api/Windows.UI.Composition.DropShadow)
- [WindowsUIDevLabs GitHub 存储库](https://github.com/Microsoft/WindowsUIDevLabs)