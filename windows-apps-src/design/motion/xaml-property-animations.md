---
title: XAML 属性动画
description: 使用组合动画的 XAML 元素具有动画效果。
ms.date: 09/13/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 183a5433553ff6fdfcb09f6960f6a642f2c8bc08
ms.sourcegitcommit: cc0ef75f314658b14376eb60ef8e5bb4d7726e04
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/08/2019
ms.locfileid: "65444151"
---
# <a name="animating-xaml-elements-with-composition-animations"></a>使用组合动画的 XAML 元素具有动画效果

本文介绍了新属性，可对 XAML UIElement 组合动画的性能和易用性设置 XAML 属性进行动画处理。

在 Windows 10，版本 1809 之前, 有 2 个选项来构建 UWP 应用程序中的动画：

- 使用 XAML 构造，如[可形成演示图板动画](storyboarded-animations.md)，或 _* ThemeTransition_并 _* ThemeAnimation_中的类[Windows.UI.Xaml.Media.Animation](/uwp/api/windows.ui.xaml.media.animation)命名空间。
- 使用组合动画中所述[可视化层中使用 XAML](../../composition/using-the-visual-layer-with-xaml.md)。

使用可视化层提供更好的性能比使用 XAML 构造。 不过，使用[ElementCompositionPreview](/uwp/api/Windows.UI.Xaml.Hosting.ElementCompositionPreview)若要获取的元素的基本组合[Visual](/uwp/api/windows.ui.composition.visual)对象，然后再进行动画处理的视觉对象组合动画是更复杂，无法使用。

从 Windows 10，版本 1809，开始可以动态显示 UIElement 直接使用组合动画，而无需获取基础组合视觉对象上的属性。

> [!NOTE]
> 若要使用这些属性 UIElement，UWP 项目目标版本必须是 1809年或更高版本。 有关配置项目版本的详细信息，请参阅[版本自适应应用](../../debug-test-perf/version-adaptive-apps.md)。

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>如果有<strong style="font-weight: semi-bold">XAML 控件库</strong>应用程序安装，请单击此处<a href="xamlcontrolsgallery:/item/XamlCompInterop">打开应用，请参阅操作中的动画互操作</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="new-rendering-properties-replace-old-rendering-properties"></a>新呈现属性替换旧呈现属性

此表显示可用于修改 UIElement，还可使用进行动画处理的呈现的属性[CompositionAnimation](/uwp/api/windows.ui.composition.compositionanimation)。

| 属性 | 在任务栏的搜索框中键入 | 描述 |
| -- | -- | -- |
| [不透明度](/uwp/api/windows.ui.xaml.uielement.opacity) | Double | 对象的不透明度 |
| [翻译](/uwp/api/windows.ui.xaml.uielement.translation) | Vector3 | 移动元素的 X/Y/Z 位置 |
| [TransformMatrix](/uwp/api/windows.ui.xaml.uielement.transformmatrix) | Matrix4x4 | 要应用于元素的转换矩阵 |
| [缩放](/uwp/api/windows.ui.xaml.uielement.scale) | Vector3 | 缩放以中心点为中心的元素 |
| [旋转](/uwp/api/windows.ui.xaml.uielement.rotation) | 浮点 | 将元素周围的 RotationAxis 和中心点旋转 |
| [RotationAxis](/uwp/api/windows.ui.xaml.uielement.rotationaxis) | Vector3 | 旋转轴 |
| [CenterPoint](/uwp/api/windows.ui.xaml.uielement.centerpoint) | Vector3 | 缩放和旋转的中心点 |

TransformMatrix 属性值是属性组合在一起缩放、 旋转和转换按以下顺序：TransformMatrix，缩放、 旋转、 转换。

这些属性不会影响该元素的布局，因此修改这些属性不会导致新[度量值](/uwp/api/windows.ui.xaml.uielement.measure)/[排列](/uwp/api/windows.ui.xaml.uielement.arrange)传递。

这些属性作为组合的名称相似的属性具有相同的用途和行为[Visual](/uwp/api/windows.ui.composition.visual)类 （除了转换，这不是视觉对象）。

### <a name="example-setting-the-scale-property"></a>例如：设置规模属性

此示例演示如何在按钮上设置的位数属性。

```xaml
<Button Scale="2,2,1" Content="I am a large button" />
```

```csharp
var button = new Button();
button.Content = "I am a large button";
button.Scale = new Vector3(2.0f,2.0f,1.0f);
```

### <a name="mutual-exclusivity-between-new-and-old-properties"></a>有新的和旧属性之间互斥性

> [!NOTE]
> **不透明度**属性不会强制在本部分中所述有互斥性。 使用 XAML 或组合动画是否使用相同的 Opacity 属性。

可使用 CompositionAnimation 进行动画处理的属性为替换为多个现有 UIElement 属性：

- [RenderTransform](/uwp/api/windows.ui.xaml.uielement.rendertransform)
- [RenderTransformOrigin](/uwp/api/windows.ui.xaml.uielement.rendertransformorigin)
- [Projection](/uwp/api/windows.ui.xaml.uielement.projection)
- [Transform3D](/uwp/api/windows.ui.xaml.uielement.transform3d)

在您设置 （或进行动画处理） 的任何新属性，不能使用旧的属性。 相反，如果你设置 （或进行动画处理） 的任何旧的属性，则无法使用新的属性。

如果使用 ElementCompositionPreview 来获取和使用这些方法自行管理视觉对象，也不能使用新属性：

- [ElementCompositionPreview.GetElementVisual](/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview.getelementvisual)
- [ElementCompositionPreview.SetIsTranslationEnabled](/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview.setistranslationenabled)

> [!IMPORTANT]
> 尝试混合使用两种属性集将会导致 API 调用失败并生成一条错误消息。

就可以切换是一组属性通过清除它们，但不建议为简单起见。 如果该属性由 DependencyProperty 提供支持 （例如，UIElement.Projection 由 UIElement.ProjectionProperty），然后调用 ClearValue 还原到其"未使用"状态。 （例如，规模属性），否则，则将属性设置为其默认值。

## <a name="animating-uielement-properties-with-compositionanimation"></a>使用 CompositionAnimation UIElement 属性进行动画处理

可以使用 CompositionAnimation 表中列出的呈现属性进行动画处理。 此外可以通过引用这些属性[ExpressionAnimation](/uwp/api/windows.ui.composition.expressionanimation)。

使用[StartAnimation](/uwp/api/windows.ui.xaml.uielement.startanimation)并[StopAnimation](/uwp/api/windows.ui.xaml.uielement.stopanimation) UIElement UIElement 属性进行动画处理的方法。

### <a name="example-animating-the-scale-property-with-a-vector3keyframeanimation"></a>例如：对具有 Vector3KeyFrameAnimation 的缩放属性进行动画处理

此示例演示如何进行动画处理按钮的小数位数。

```csharp
var compositor = Window.Current.Compositor;
var animation = compositor.CreateVector3KeyFrameAnimation();

animation.InsertKeyFrame(1.0f, new Vector3(2.0f,2.0f,1.0f));
animation.Duration = TimeSpan.FromSeconds(1);
animation.Target = "Scale";

button.StartAnimation(animation);
```

### <a name="example-animating-the-scale-property-with-an-expressionanimation"></a>例如：对具有 ExpressionAnimation 的缩放属性进行动画处理

页有两个按钮。 第二个按钮进行动画处理为两次 （通过缩放） 最大为第一个按钮。

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

- [可形成演示图板动画](storyboarded-animations.md)
- [可视化层中使用 XAML](../../composition/using-the-visual-layer-with-xaml.md)
- [转换概述](../layout/transforms.md)
