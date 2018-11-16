---
author: Jwmsft
title: XAML 属性的动画
description: 使用合成动画设置动画的 XAML 元素。
ms.author: jimwalk
ms.date: 09/13/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
ms.localizationpriority: medium
ms.openlocfilehash: 9372ba818805446948a444632e809ec06691c5e5
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/16/2018
ms.locfileid: "6993632"
---
# <a name="animating-xaml-elements-with-composition-animations"></a>使用合成动画设置动画的 XAML 元素

本文介绍了新属性，使你对 XAML UIElement 使用合成动画的性能和轻松使用设置 XAML 属性进行动画处理。

在 Windows 10 版本 1809 之前，你有 2 个选项来生成你的 UWP 应用中的动画：

- 使用[情节提要动画](storyboarded-animations.md)，如 XAML 构造或 _* ThemeTransition_和 _* ThemeAnimation_ [Windows.UI.Xaml.Media.Animation](/uwp/api/windows.ui.xaml.media.animation)命名空间中的类。
- [使用可视化层与 XAML](../../composition/using-the-visual-layer-with-xaml.md)中所述，请使用合成动画。

使用可视化层提供更好的性能比使用 XAML 构造。 但使用[ElementCompositionPreview](/uwp/api/Windows.UI.Xaml.Hosting.ElementCompositionPreview)获取元素的基础合成[视觉](/uwp/api/windows.ui.composition.visual)对象，然后创建动画使用合成动画的可视且更复杂，若要使用。

从开始 Windows 10 版本 1809，你可以进行动画处理直接使用合成动画而不要求以获取基础合成视觉对象的 UIElement 上的属性。

> [!NOTE]
> 若要在 UIElement 上使用这些属性，你的 UWP 项目目标版本必须为 1809年或更高版本。 有关配置你的项目版本的详细信息，请参阅[版本自适应应用](../../debug-test-perf/version-adaptive-apps.md)。

## <a name="new-rendering-properties-replace-old-rendering-properties"></a>新呈现属性替换旧呈现属性

此表显示了可用于修改 UIElement，还可使用一个[CompositionAnimation](/uwp/api/windows.ui.composition.compositionanimation)进行动画处理的呈现的属性。

| 属性 | 类型 | 说明 |
| -- | -- | -- |
| [Opacity](/uwp/api/windows.ui.xaml.uielement.opacity) | 双面 | 此对象的不透明度 |
| [Translation](/uwp/api/windows.ui.xaml.uielement.translation) | Vector3 | Shift 元素的 X/Y/Z 位置 |
| [TransformMatrix](/uwp/api/windows.ui.xaml.uielement.transformmatrix) | Matrix4x4 | 要应用于元素的转换矩阵 |
| [Scale](/uwp/api/windows.ui.xaml.uielement.scale) | Vector3 | 缩放的元素，集中于中心点 |
| [旋转](/uwp/api/windows.ui.xaml.uielement.rotation) | Float | 旋转围绕 RotationAxis 和中心点的元素 |
| [RotationAxis](/uwp/api/windows.ui.xaml.uielement.rotationaxis) | Vector3 | 旋转轴 |
| [CenterPoint](/uwp/api/windows.ui.xaml.uielement.centerpoint) | Vector3 | 缩放和旋转中心点 |

TransformMatrix 属性值结合的缩放、 旋转和平移的属性，按以下顺序： TransformMatrix、 缩放、 旋转、 转换。

这些属性不会影响该元素的布局，因此修改这些属性不会导致新[Measure](/uwp/api/windows.ui.xaml.uielement.measure)/[排列](/uwp/api/windows.ui.xaml.uielement.arrange)传递。

这些属性作为合成[视觉](/uwp/api/windows.ui.composition.visual)类 （除了转换，这不是在视觉上） 上的类似于名为属性具有相同的用途和行为。

### <a name="example-setting-the-scale-property"></a>示例： 设置缩放属性

此示例显示了如何设置按钮上的比例属性。

```xaml
<Button Scale="2,2,1" Content="I am a large button" />
```

```csharp
var button = new Button();
button.Content = "I am a large button";
button.Scale = new Vector3(2.0f,2.0f,1.0f);
```

### <a name="mutual-exclusivity-between-new-and-old-properties"></a>新的和旧属性之间相互独有性子

> [!NOTE]
> **Opacity**属性不强制执行此部分中所述的相互唯一性。 是否使用 XAML 或合成动画，你可以使用相同的 Opacity 属性。

可使用一个 CompositionAnimation 进行动画处理的属性是几个现有的 UIElement 属性替换：

- [RenderTransform](/uwp/api/windows.ui.xaml.uielement.rendertransform)
- [RenderTransformOrigin](/uwp/api/windows.ui.xaml.uielement.rendertransformorigin)
- [投影](/uwp/api/windows.ui.xaml.uielement.projection)
- [Transform3D](/uwp/api/windows.ui.xaml.uielement.transform3d)

当你设置 （或进行动画处理） 的任何新属性时，你无法使用的旧属性。 相反，如果你设置 （或进行动画处理） 的任何旧属性，则无法使用新的属性。

如果你使用 ElementCompositionPreview 获取和管理视觉对象自行使用这些方法，你还不能使用的新属性：

- [ElementCompositionPreview.GetElementVisual](/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview.getelementvisual)
- [ElementCompositionPreview.SetIsTranslationEnabled](/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview.setistranslationenabled)

> [!IMPORTANT]
> 尝试混合使用两个属性集将导致 API 调用失败，且会产生一条错误消息。

很可能但不建议为简单起见，从一组属性通过清除它们，切换。 如果该属性受 DependencyProperty （例如，UIElement.Projection 受 UIElement.ProjectionProperty），然后调用 ClearValue 以将其还原到其"未使用"状态。 （例如，缩放属性），否则将属性设置为其默认值。

## <a name="animating-uielement-properties-with-compositionanimation"></a>与 CompositionAnimation UIElement 属性进行动画处理

可以使用一个 CompositionAnimation 表中列出的呈现属性进行动画处理。 也可以通过[ExpressionAnimation](/uwp/api/windows.ui.composition.expressionanimation)引用这些属性。

使用在 UIElement 上的[StartAnimation](/uwp/api/windows.ui.xaml.uielement.startanimation)和[StopAnimation](/uwp/api/windows.ui.xaml.uielement.stopanimation)方法 UIElement 属性进行动画处理。

### <a name="example-animating-the-scale-property-with-a-vector3keyframeanimation"></a>示例： 将使用 Vector3KeyFrameAnimation 比例属性进行动画处理

此示例显示了如何进行动画处理按钮的比例。

```csharp
var compositor = Window.Current.Compositor;
var animation = compositor.CreateVector3KeyFrameAnimation();

animation.InsertKeyFrame(1.0f, new Vector3(2.0f,2.0f,1.0f));
animation.Duration = TimeSpan.FromSeconds(1);
animation.Target = "Scale";

button.StartAnimation(animation);
```

### <a name="example-animating-the-scale-property-with-an-expressionanimation"></a>示例： 将 expressionanimation 比例属性进行动画处理

页面具有两个按钮。 第二个按钮进行动画处理为两倍大 （通过 scale) 作为第一个按钮。

```xaml
<Button x:Name="sourceButton" Content="Source"/>
<Button x:Name="destinationButton" Content="Destination"/>
```

```csharp
var compositor = Window.Current.Compositor;
var animation = compositor.CreateExpressionAnimation("sourceButton.Scale*2");
animation.SetExpressionReferenceParameter("sourceButton", sourceButton);
animation.Target = "Scale";
destinationButton.StartAnimation(animation);
```

## <a name="related-topics"></a>相关主题

- [情节提要动画](storyboarded-animations.md)
- [将可视化层与 XAML 结合使用](../../composition/using-the-visual-layer-with-xaml.md)
- [变形概述](../layout/transforms.md)