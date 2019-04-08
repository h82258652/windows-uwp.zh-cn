---
title: 组合照明
description: 组合照明 Api 可用于将动态 3D 光照添加到你的应用程序。
ms.date: 07/16/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 733ce75942a05482ade88c1510e788f1cbd515d4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57602202"
---
# <a name="using-lights-in-windows-ui"></a>Windows UI 中使用灯

Windows.UI.Composition Api 使你能够创建实时动画和效果。 组合照明使 3D 光照 2D 应用程序中。 在此概述中，我们将通过如何设置组合灯、 确定视觉对象以接收每个光源和效果用于定义你的内容的材料的功能运行。

> [!NOTE]
> 如何读取[XamlLight](/uwp/api/windows.ui.xaml.media.xamllight)对象将应用[CompositionLights](/uwp/api/Windows.UI.Composition.CompositionLight)照射 XAML UIElements，请参阅[XAML 照明](xaml-lighting.md)。

组合照明允许您创建有趣的 UI，从而：

- 轻型独立于场景中，从而启用沉浸式方案，如音乐播放场景中的其他对象的转换。
- 对具有光线的对象，以便它们一起移动的能力独立于场景中，从而启用方案，如 Fluent 其余[揭示](/windows/uwp/design/style/reveal)突出显示。
- 作为一个组来创建材料和深度光和整个场景的转换。

组合照明支持三个关键概念：**Light**，**目标**，和**SceneLightingEffect**。

## <a name="light"></a>浅色

[CompositionLight](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlight) ，可创建各种光和将其放在坐标空间。 这些灯面向想要标识为光照亮的视觉对象。

### <a name="light-types"></a>光类型

| 在任务栏的搜索框中键入 | 描述 |
| --- | --- |
| [AmbientLight](/uwp/api/windows.ui.composition.ambientlight) | 发出出现的非定向光源的光源反射场景中的所有内容。 |
| [DistantLight](/uwp/api/windows.ui.composition.distantlight) | 无限大远处的光源的发光的一个方向。 如 sun。 |
| [PointLight](/uwp/api/windows.ui.composition.pointlight) | 发出的所有方向光的光点源。 如灯泡。 |
| [聚焦](/uwp/api/windows.ui.composition.spotlight) | 发出的光线的内部和外部圆锥光源。 如手电筒。 |

## <a name="targets"></a>目标

当灯针对视觉对象 (将添加到[目标](/uwp/api/windows.ui.composition.compositionlight.targets)列表)，视觉对象及其所有子代都识别并响应此光源。 这可以是如下简单的形式对点光源方向的动画树和下面的所有视觉对象的根处的 PointLight 源做出反应的设置。

**ExclusionsFromTargets**使您能够以类似方式与添加目标删除照明的视觉对象或视觉对象的子树。 在树中排除视觉对象取得 root 权限的子级不因此亮。

### <a name="sample-targets"></a>示例 （目标）

在下面的示例中，我们使用 CompositionPointLight 面向 XAML TextBlock。

```cs
    _pointLight = _compositor.CreatePointLight();
    _pointLight.Color = Colors.White;
    _pointLight.CoordinateSpace = text; //set up co-ordinate space for offset
    _pointLight.Targets.Add(text); //target XAML TextBlock
```

通过将动画添加到点光的偏移量，可以轻松实现闪烁效果。

```cs
_pointLight.Offset = new Vector3(-(float)TextBlock.ActualWidth, (float)TextBlock.ActualHeight / 2, (float)TextBlock.FontSize);
```

请参阅完整[文本闪现](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2014393/TextShimmer)WindowUIDevLabs 示例条样，若要了解更多示例。

## <a name="restrictions"></a>限制

有几个 CompositionLight 将为文字确定哪些内容时要考虑的因素。

概念 | 详细信息
--- | ---
**环境光线** | 将非环境光线添加到您的场景将关闭所有现有的光线。  未针对非环境光线的项将显示为黑色。  若要照亮周围的视觉对象未针对光线很自然地，使用与其他灯结合环境光线。
**灯** | 您可以使用任何两个非环境组合灯的任意组合以面向您的 UI。 环境光不会限制;找出，点光和存有一定距离的光。
**生存期** | CompositionLight 可能会遇到生存期条件 (示例： 垃圾回收器可以回收的光对象，然后使用它)。  我们建议通过将光添加为成员，才能帮助应用程序管理生命周期中保持对您的灯的引用。
**转换** | 光必须放置在上方使用效果，例如 UI 节点[透视转换](/windows/uwp/design/layout/3-d-perspective-effects)中要正确绘制在可视结构。
**目标和坐标空间** | CoordinateSpace 是灯属性必须设置所有的可视空间。 CompositionLight.Targets 必须 CoordinateSpace 树中。

## <a name="lighting-properties"></a>照明属性

根据所使用光的类型，光可以具有衰减和空间的属性。 并非所有光类型都使用所有属性。

属性 | 描述
--- | ---
**颜色** | [颜色](/uwp/api/windows.ui.color)光。 通过定义照明颜色值[D3D](https://docs.microsoft.com/windows/uwp/graphics-concepts/light-properties)漫射、 Ambient，并定义所发出的颜色的反射。 照明的光; 使用 RGBA 值不使用 alpha 颜色组件。
**方向** | 光的方向。 光所指方向指定相对于其[CoordinateSpace](/uwp/api/windows.ui.composition.distantlight.coordinatespace) Visual。
**坐标空间** | 每个视觉对象具有隐式的三维坐标空间。 X 方向是从左到右。 Y 方向是从上到下。 Z 方向是从平面的点。 此坐标的原始点是左上角的视觉对象，且单位是设备独立像素 (DIP)。 在此坐标中定义的光源的偏移量。
**内部和外部圆锥** | 聚光发出的光锥包括两个部分：一个明亮的内锥和一个外锥。 组合允许你控制对内部和外部锥角和颜色。
**偏移量** | 相对于其坐标空间 Visual 光源的偏移量。

> [!NOTE]
> 当多个光源达到相同的视觉对象，或每当光源的颜色值获取足够大，以超过 1.0 时，可能会由于夹紧的灯颜色通道更改光的颜色。

### <a name="advanced-lighting-properties"></a>高级照明属性

属性 | 描述
--- | ---
**强度** | 控制光的亮度。
**衰减** | 衰减控制光的强度如何随距离属性指定的最大距离而减弱。  常量，Quadradic 和线性衰减属性可用。

## <a name="getting-started-with-lighting"></a>开始使用照明

请按照以下常规步骤添加灯操作：

- 创建并将系统正常运行：创建光，并将它们放在指定的坐标空间。
- 标识对象发光对象：目标灯在相关的视觉对象。
- [可选]定义个别对象向灯做出反应：使用与 EffectBrush SceneLightingEffect 自定义用于显示 SpriteVisual 光线反射。 反射的默认值支持子级的光源的 CoordinateSpace 的照明。  使用 SceneLightingEffect 绘制视觉对象将覆盖该视觉对象默认照明。

## <a name="scenelightingeffect"></a>SceneLightingEffect

[SceneLightingEffect](/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect)用于修改应用到的内容的默认光照[SpriteVisual](/uwp/api/Windows.UI.Composition.SpriteVisual)面向[CompositionLight](/uwp/api/windows.ui.composition.compositionlight)。

[SceneLightingEffect](/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect)经常用于材料的创建。 SceneLightingEffect 是你想要实现更复杂，例如启用映像上的反射属性和/或使用法线贴图提供深度的错觉时使用的效果。 SceneLightingEffect 提供要使用的照明属性，反射高光和漫射量等自定义 UI 的功能。 您可以进一步自定义光照效果，从而可以单独进行混合和 compose 与你的内容的不同光源反应效果管道的其余部分使用。

> [!NOTE]
> 场景的照明不会产生阴影;它是侧重于 2D 呈现的效果。  它不考虑包含真实照明模型，包括阴影的考虑因素 3D 光照方案。


属性 | 描述
--- | ---
**法线贴图** | NormalMaps 创建其中指示灯正常指向将更亮和正常指向立即将不断变暗的纹理的效果。 若要向目标 visual 使用 NormalMap [CompositionSurfaceBrush](/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush)使用 LoadedImageSurface 加载 NormalMap 资产。
**环境** | 环境属性主要用于控制总体颜色反射。
**反射** | 反射高光反射对象，使它们看起来有光泽上创建突出显示。 您可以控制的反射高光反射级别以及闪光的级别。  这些属性操作创建材料效果，例如 shinny 金属或光泽纸。
**漫射** | 扩散型的反射散布在所有方向上的指示灯。
**反射度模型** | [反射度模型](/uwp/api/windows.ui.composition.effects.scenelightingeffectreflectancemodel)允许你选择之间[Blinn 冯氏](https://docs.microsoft.com/visualstudio/designers/how-to-create-a-basic-phong-shader)和以物理方式基于 Blinn Phong。  如果想要具有精简反射高光，您可以选择以物理方式基于 Blinn 冯氏。

### <a name="sample-scenelightingeffect"></a>示例 (SceneLightingEffect)

下面的示例演示如何向 SceneLightingEffect 添加法线贴图。

```cs
CompositionBrush CreateNormalMapBrush(ICompositionSurface normalMapImage)
{
    var colorSourceEffect = new ColorSourceEffect()
    {
        Color = Colors.White
    };
    var sceneLightingEffect = new SceneLightingEffect()
    {
        NormalMapSource = new CompositionEffectSourceParameter("NormalMap")
    };

    var compositeEffect = new ArithmeticCompositeEffect()
    {
        Source1 = colorSourceEffect,
        Source2 = sceneLightingEffect,
    };

    var factory = _compositor.CreateEffectFactory(sceneLightingEffect);

    var normalMapBrush = _compositor.CreateSurfaceBrush();
    normalMapBrush.Surface = normalMapImage;
    normalMapBrush.Stretch = CompositionStretch.Fill;

    var brush = factory.CreateBrush();
    brush.SetSourceParameter("NormalMap", normalMapBrush);

    return brush;
}
```

## <a name="related-articles"></a>相关文章

- [在可视化层中创建材料和光线](https://blogs.windows.com/buildingapps/2017/08/04/creating-materials-lights-visual-layer/)
- [照明概述](https://docs.microsoft.com/windows/uwp/graphics-concepts/lighting-overview)
- [CompositionCapabilities API](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositioncapabilities)
- [照明的数学](https://docs.microsoft.com/windows/uwp/graphics-concepts/mathematics-of-lighting)
- [SceneLightingEffect](https://docs.microsoft.com/uwp/api/windows.ui.composition.effects.scenelightingeffect)
- [WindowsUIDevLabs GitHub 存储库](https://github.com/Microsoft/WindowsUIDevLabs)
