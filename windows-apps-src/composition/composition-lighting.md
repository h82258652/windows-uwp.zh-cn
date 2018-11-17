---
author: daneuber
title: 合成光线
description: 合成光线 Api 可用于将动态 3D 照明添加到你的应用程序。
ms.author: jimwalk
ms.date: 07/16/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c5c7bfcb06eb673b0516cef7882685ebd19ddb97
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/16/2018
ms.locfileid: "7146280"
---
# <a name="using-lights-in-windows-ui"></a>Windows UI 中使用光

Windows.UI.Composition Api 使你能够创建实时动画和效果。 合成光线使 2D 应用程序中的 3D 照明。 在此概述中，我们将运行效果如何设置合成光、 标识视觉对象以接收每个光源，以及使用效果来定义内容的材料的功能。

> [!NOTE]
> 若要读取[XamlLight](/uwp/api/windows.ui.xaml.media.xamllight)对象将[Compositionlight](/uwp/api/Windows.UI.Composition.CompositionLight)照亮 XAML Uielement 的应用，请参阅[XAML 照明](xaml-lighting.md)。

合成光线允许你创建有趣 UI 通过允许：

- 光独立于场景实现沉浸式方案，如音乐播放场景中的其他对象的转换。
- 能够配对光的对象，因此它们一起移动独立于场景启用[展示](/design/style/reveal.md)Fluent 突出显示之类的方案的其余部分。
- 作为一组来创建材料和深度光和整个场景的转换。

合成光线支持三个关键概念：**光**、**目标**和**SceneLightingEffect**。

## <a name="light"></a>Light

[CompositionLight](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlight)允许你创建各种光，并将它们放置在坐标空间中。 这些光面向你想要标识为光照亮的视觉效果。

### <a name="light-types"></a>光类型

| 类型 | 说明 |
| --- | --- |
| [AmbientLight](/uwp/api/windows.ui.composition.ambientlight) | 发出出现的非定向光的光源反映由场景中的所有内容。 |
| [DistantLight](/uwp/api/windows.ui.composition.distantlight) | 大型无限远距离光源发射光在单个方向。 如太阳。 |
| [PointLight](/uwp/api/windows.ui.composition.pointlight) | 点光发射光在所有方向的源。 如灯泡。 |
| [聚焦](/uwp/api/windows.ui.composition.spotlight) | 光源发出内锥和外锥。 如手电筒。 |

## <a name="targets"></a>目标

当光面向一种可视 （添加到[目标](/uwp/api/windows.ui.composition.compositionlight.targets)列表），视觉对象及其所有后代已注意到，对此光源响应。 这可能很简单作为根处树和下面的所有视觉 PointLight 源响应点光方向的动画设置。

**ExclusionsFromTargets**使你能够与添加目标类似的方式删除或视觉效果的子树的一种可视的光。 获得 root 权限由被排除的可视树中的子元素均未亮起作为结果。

### <a name="sample-targets"></a>示例 （目标）

在下面的示例中，我们使用 CompositionPointLight 面向 XAML TextBlock。

```cs
    _pointLight = _compositor.CreatePointLight();
    _pointLight.Color = Colors.White;
    _pointLight.CoordinateSpace = text; //set up co-ordinate space for offset
    _pointLight.Targets.Add(text); //target XAML TextBlock
```

通过将动画添加到点光的偏移量，闪烁效果都很容易实现。

```cs
_pointLight.Offset = new Vector3(-(float)TextBlock.ActualWidth, (float)TextBlock.ActualHeight / 2, (float)TextBlock.FontSize);
```

请参阅完整的[文本闪现](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2014393/TextShimmer)示例在 WindowUIDevLabs 示例条样若要了解详细信息。

## <a name="restrictions"></a>限制

有几个因素，确定哪些内容将照亮 CompositionLight 时需要考虑。

概念 | 详细信息
--- | ---
**环境光** | 向场景添加非环境光将关闭所有现有的光。  不面向非环境光的项目将显示为黑色。  若要都能照亮周围的视觉对象不定向光的自然方式，使用与其他光一起使用的环境光。
**光的数量** | 你可以使用任何两个合成非环境光的任意组合要针对你的 UI。 环境光没有限制;发现，点和远的光。
**生命周期** | CompositionLight 可能会遇到生存期条件 (示例： 使用之前，垃圾回收器可能回收光对象)。  我们建议通过将光添加为成员来帮助管理生命周期的应用程序保持对你灯的引用。
**转换** | 光必须放在上面使用你的可视结构中的效果，例如[透视转换](/design/layout/3-d-perspective-effects.md)正确绘制的 UI 的节点。
**目标和坐标空间** | CoordinateSpace 是光属性必须设置中的所有视觉空间。 CompositionLight.Targets 必须 CoordinateSpace 树内。

## <a name="lighting-properties"></a>光属性

根据使用光的类型，光可以具有衰减和空间的属性。 并非所有光类型都使用所有属性。

属性 | 说明
--- | ---
**颜色** | 光的[颜色](/uwp/api/windows.ui.color)。 照明值由[D3D](https://docs.microsoft.com/windows/uwp/graphics-concepts/light-properties)漫射、 Ambient 和 Specular 定义正在发出的颜色的颜色。 照明的光; 使用了 RGBA 值不使用 alpha 颜色分量。
**Direction** | 光线的方向。 相对于其[CoordinateSpace](/uwp/api/windows.ui.composition.distantlight.coordinatespace) Visual 指定指光线方向。
**坐标空间** | 每个 Visual 具有隐式的 3D 坐标空间。 X 方向是从左到右。 Y 方向是从顶部到底部。 Z 方向是退出平面的点。 此坐标的原始点是左上角与可视，并且该单元设备无关的像素 (DIP)。 在此坐标中定义的光的偏移量。
**内锥和外锥细胞** | 聚光发出的光锥包括两个部分：一个明亮的内锥和一个外锥。 合成允许你控制内锥和外锥角和颜色。
**Offset** | 相对于其坐标空间 Visual 光源的偏移量。

> [!NOTE]
> 当多个灯命中同一个 Visual，或者每当光的颜色值获取足以超过 1.0，光的颜色可能由于固定光颜色通道的更改。

### <a name="advanced-lighting-properties"></a>高级照明属性

属性 | 说明
--- | ---
**强度** | 控制光的亮度。
**衰减** | 衰减控制光的强度如何随距离属性指定的最大距离而减弱。  常量，Quadradic 和线性衰减属性可以使用。

## <a name="getting-started-with-lighting"></a>开始使用照明

请按照以下常规步骤添加光操作：

- 创建和放置光： 创建光并将其放在指定的坐标空间。
- 标识光对象： 面向在相关的视觉效果的光。
- [可选]定义如何单个对象对光作出响应： 使用 SceneLightingEffect 与 EffectBrush 自定义用于显示涂刷反射光。 反射默认设置支持子级的光源的 CoordinateSpace 的照明。  可视对象与 SceneLightingEffect 绘制覆盖该视觉对象的默认光。

## <a name="scenelightingeffect"></a>SceneLightingEffect

[SceneLightingEffect](/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect)用于修改应用于[SpriteVisual](/uwp/api/Windows.UI.Composition.SpriteVisual) [CompositionLight](/uwp/api/windows.ui.composition.compositionlight)的目标的内容的默认光。

[SceneLightingEffect](/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect)经常用于材料创建。 SceneLightingEffect 是当你想要实现更复杂，如启用在图像上的反射属性和/或提供深度错觉与正常映射所使用的效果。 SceneLightingEffect 能够使用照明属性，例如高光和漫射金额自定义你的 UI。 你可以进一步自定义效果管道允许你单独混合和撰写与你的内容不同的照明反应的其余部分的照明效果。

> [!NOTE]
> 场景照明不会产生阴影;它是专注于 2D 呈现效果。  它不会考虑 3D 照明包含的方案的实际照明模型，包括阴影到。


属性 | 说明
--- | ---
**正常映射** | NormalMaps 创建其中正常指向光线会显得更亮并且正常指向立即将暗的纹理效果。 若要添加到你定向 visual NormalMap 使用使用 LoadedImageSurface 加载 NormalMap 资产[CompositionSurfaceBrush](/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) 。
**环绕** | 主要用于控制整体颜色反射环境属性。
**反射** | 镜面反射创建对象，从而使其看上去闪光上突出显示。 你可以控制镜面反射程度以及闪光的级别。  These properties are manipulated to create material effects like shinny metals or glossy paper.
**漫射** | 漫射的反射散布在所有方向的光。
**反射模型** | [反射模型](/uwp/api/windows.ui.composition.effects.scenelightingeffectreflectancemodel)允许你[Blinn Phong](https://docs.microsoft.com/visualstudio/designers/how-to-create-a-basic-phong-shader)和物理基于 Blinn Phong 之间进行选择。  如果你想要有压缩反射高光，你将选择以物理方式基于 Blinn Phong。

### <a name="sample-scenelightingeffect"></a>示例 (SceneLightingEffect)

下面的示例显示了如何将正常映射添加到 SceneLightingEffect。

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

- [在可视化层中创建材料和光](https://blogs.windows.com/buildingapps/2017/08/04/creating-materials-lights-visual-layer/)
- [照明概述](https://docs.microsoft.com/windows/uwp/graphics-concepts/lighting-overview)
- [CompositionCapabilities API](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositioncapabilities)
- [照明的数学](https://docs.microsoft.com/windows/uwp/graphics-concepts/mathematics-of-lighting)
- [SceneLightingEffect](https://docs.microsoft.com/uwp/api/windows.ui.composition.effects.scenelightingeffect)
- [WindowsUIDevLabs GitHub 存储库](https://github.com/Microsoft/WindowsUIDevLabs)
