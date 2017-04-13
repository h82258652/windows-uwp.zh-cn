---
author: Jwmsft
ms.assetid: F46D5E18-10A3-4F7B-AD67-76437C77E4BC
title: "转换概述"
description: "了解如何通过更改 UI 中元素的相对坐标系，在 Windows 运行时&amp;\\#160;API 中使用转换。"
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 2643606fa3dcbf1c95669bb09bef0aae49f86529
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="transforms-overview"></a>转换概述

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


了解如何通过更改 UI 中元素的相对坐标系，在 Windows 运行时 API 中使用转换。 此方法可用于调整各个 XAML 元素的外观，比如缩放、旋转或转换在 x-y 空间中的位置。

## <a name="span-idwhatisatransformspanspan-idwhatisatransformspanspan-idwhatisatransformspanwhat-is-a-transform"></a><span id="What_is_a_transform_"></span><span id="what_is_a_transform_"></span><span id="WHAT_IS_A_TRANSFORM_"></span>什么是转换？

*转换*定义了如何将各种点从一个坐标空间映射或转换到另一个坐标空间。 将转换应用到 UI 元素时，就更改了 UI 元素作为 UI 一部分在屏幕中呈现的方式。

将转换视为四个广义的类别：转换、旋转、缩放和倾斜（或扭曲）。 出于使用图形 API 更改 UI 元素外观的目的，通常最简单的做法是创建每次只定义一个操作的转换。 因此 Windows 运行时为每一种转换分类定义了一个离散类。

-   [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027)：通过为 [**X**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.translatetransform.x.aspx) 和 [**Y**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.translatetransform.y) 设置值，在 x-y 空间转换元素。
-   [**ScaleTransform**](https://msdn.microsoft.com/library/windows/apps/BR242940)：通过为 [**CenterX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.scaletransform.centerx.aspx)、[**CenterY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.scaletransform.centery.aspx)、[**ScaleX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.scaletransform.scalex.aspx) 和 [**ScaleY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.scaletransform.scaleyproperty) 设置值，基于中心点缩放转换内容。
-   [**RotateTransform**](https://msdn.microsoft.com/library/windows/apps/BR242932)：通过为 [**Angle**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.rotatetransform.angle.aspx)、[**CenterX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.rotatetransform.centerx.aspx) 和 [**CenterY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.rotatetransform.centery) 设置值，在 x-y 空间旋转。
-   [**SkewTransform**](https://msdn.microsoft.com/library/windows/apps/BR242950)：通过为 [**AngleX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.skewtransform.anglex.aspx)、[**AngleY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.skewtransform.angley.aspx)、[**CenterX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.skewtransform.centerx.aspx) 和 [**CenterY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.scaletransform.centeryproperty)设置值，在 x-y 空间倾斜或扭曲。

在这其中，你可能最常为 UI 方案使用 [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027) 和 [**ScaleTransform**](https://msdn.microsoft.com/library/windows/apps/BR242940)。

你可以合并转换，有两种 Windows 运行时类支持此操作：[**CompositeTransform**](https://msdn.microsoft.com/library/windows/apps/BR228105) 和 [**TransformGroup**](https://msdn.microsoft.com/library/windows/apps/BR243022)。 在 **CompositeTransform** 中，按照以下顺序应用转换：缩放、倾斜、旋转、转换。 如果你希望以不同的顺序应用转换，请使用 **TransformGroup** 而不是 **CompositeTransform**。 有关详细信息，请参阅 [**CompositeTransform**](https://msdn.microsoft.com/library/windows/apps/BR228105)。

## <a name="span-idtransformsandlayoutspanspan-idtransformsandlayoutspanspan-idtransformsandlayoutspantransforms-and-layout"></a><span id="Transforms_and_layout"></span><span id="transforms_and_layout"></span><span id="TRANSFORMS_AND_LAYOUT"></span>转换和布局

在 XAML 布局中，将在布局过程完成后应用转换，因此有关可用空间的计算和其他布局决定均在应用转换之前执行。 因为布局优先，如果转换 [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704) 单元或可在布局时分配空间的相似布局容器，有时你会获得意外的结果。 转换过的元素可能被截断或遮盖，因为该元素尝试绘入的区域在其父容器划分空间时未计算转换后的维度。 你可能需要试验不同的转换结果并调整某些设置。 例如，你可能需要更改 **Center** 属性或声明用于布局空间的固定像素度量，而不依赖于自适应布局和比例缩放，以确保父容器分配了足够的空间。

**迁移说明：**Windows Presentation Foundation (WPF) 具有 **LayoutTransform** 属性，该属性将在布局传递之前应用转换。 但 Windows 运行时 XAML 不支持 **LayoutTransform** 属性。 （Microsoft Silverlight 也没有此属性。）

## <a name="span-idapplyingatransformtoauielementspanspan-idapplyingatransformtoauielementspanspan-idapplyingatransformtoauielementspanapplying-a-transform-to-a-ui-element"></a><span id="Applying_a_transform_to_a_UI_element"></span><span id="applying_a_transform_to_a_ui_element"></span><span id="APPLYING_A_TRANSFORM_TO_A_UI_ELEMENT"></span>将转换应用到 UI 元素

将转换应用到对象时，通常进行该操作以设置属性 [**UIElement.RenderTransform**](https://msdn.microsoft.com/library/windows/apps/BR208980)。 设置该属性并不是按字面意思逐个像素地更改对象。 该属性真正的作用是将转换应用到存在该对象的本地坐标空间内。 然后呈现逻辑和操作（布局后）将会呈现合并的坐标空间，使对象外观以及布局位置（如果应用了 [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027)，位置可能更改）看起来发生了更改。

默认情况下，每个呈现转换将以目标对象本地坐标系的原点（它的 (0,0)）为中心。 唯一的例外是 [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027)，它不具有可设置的居中属性，因为无论位于何处，转换效果都相同。 但是其他的每一个转换都具有设置 **CenterX** 和 **CenterY** 值的属性。

将转换与 [**UIElement.RenderTransform**](https://msdn.microsoft.com/library/windows/apps/BR208980) 配合使用时，请记住在 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911) 中有另一个影响转换行为的属性：[**RenderTransformOrigin**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.rendertransformorigin.aspx)。 **RenderTransformOrigin** 所声明的是整个转换应用到元素的默认点 (0,0)，还是应用到某个该元素相对坐标空间内的其他原点。 对于典型的元素，(0,0) 将转换放置到左上角。 你可能会选择更改 **RenderTransformOrigin** 而不是在转换上对 **CenterX** 和 **CenterY** 值进行调整，这取决于你需要的效果。 请注意，如果同时应用 **RenderTransformOrigin** 和 **CenterX** / **CenterY** 值，结果可能会相当混乱，当你为这些值中的任意一个设置动画时尤其如此。

出于命中测试的目的，将应用转换的对象会继续以与其 x-y 空间上的视觉外观一致的预期方式响应输入。 比如，如果你使用了 [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027) 将 [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371) 在 UI 中横向移动 400 个像素，则当该用户按下该 **Rectangle** 在视觉上出现的点时，**Rectangle** 可以响应 [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.pointerpressed.aspx) 事件。 如果用户按下转换前 **Rectangle** 所在的区域，将不会获得 false 事件。 对于任何影响命中测试的 Z 索引注意事项，应用转换不会产生任何差异；Z 索引仍按照容器中声明的子顺序进行评估，该索引可管理哪个元素可处理 x-y 空间中的点的输入事件。 尽管就 [**Canvas**](https://msdn.microsoft.com/library/windows/apps/BR209267) 对象的子元素而言，可以通过对子元素应用 [**Canvas.ZIndex**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.canvas.zindex.aspx) 附加属性来调整顺序，但该顺序通常与在 XAML 中声明元素的顺序相同。

## <a name="span-idothertransformpropertiesspanspan-idothertransformpropertiesspanspan-idothertransformpropertiesspanother-transform-properties"></a><span id="Other_transform_properties"></span><span id="other_transform_properties"></span><span id="OTHER_TRANSFORM_PROPERTIES"></span>其他转换属性

-   [**Brush.Transform**](https://msdn.microsoft.com/library/windows/apps/BR228082)、[**Brush.RelativeTransform**](https://msdn.microsoft.com/library/windows/apps/BR228080)：这些属性影响 [**Brush**](https://msdn.microsoft.com/library/windows/apps/BR228076) 如何在应用了 **Brush** 的区域内使用坐标空间设置视觉属性，如前景和背景。 虽然这些转换与最常用的画笔不相关（它们通常使用 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) 设置纯色），但在使用 [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/BR210101) 或 [**LinearGradientBrush**](https://msdn.microsoft.com/library/windows/apps/BR210108) 绘制区域时可能偶尔有用。
-   [**Geometry.Transform**](https://msdn.microsoft.com/library/windows/apps/BR210066)：你可使用此属性对几何图形应用转换，然后将该几何图形用于 [**Path.Data**](https://msdn.microsoft.com/library/windows/apps/BR243356) 属性值。

## <a name="span-idanimatingatransformspanspan-idanimatingatransformspanspan-idanimatingatransformspananimating-a-transform"></a><span id="Animating_a_transform"></span><span id="animating_a_transform"></span><span id="ANIMATING_A_TRANSFORM"></span>为转换设置动画

可以创建 [**Transform**](https://msdn.microsoft.com/library/windows/apps/BR243006) 对象动画。 若要创建 **Transform** 动画，将兼容类型的动画应用到要创建动画的属性。 这通常意味着你使用 [**DoubleAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243136) 或 [**DoubleAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.doubleanimationusingkeyframes) 对象定义动画，因为所有转换属性都属于 [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx) 类型。 即使它们的持续时间不为零，对用于 [**UIElement.RenderTransform**](https://msdn.microsoft.com/library/windows/apps/BR208980) 值的转换产生影响的动画也不视为从属动画。 有关从属动画的详细信息，请参阅[情节提要动画](storyboarded-animations.md)。

如果要创建属性动画以对网视觉外观产生类似于转换的效果（例如，为 [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/BR208706) 的 [**Width**](https://msdn.microsoft.com/library/windows/apps/BR208751) 和 [**Height**](https://msdn.microsoft.com/library/windows/apps/BR208718) 创建动画，而不是应用 [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027)），此类动画几乎总被视为从属动画。 你必须启用动画，该动画可能会有较大的性能问题，尝试在创建对象动画时支持用户交互尤其如此。 因此更好的方式是使用转换并为它设置动画，而不是为任何将动画视作从属动画的属性设置动画。

为了将转换设为目标，必须将现有的 [**Transform**](https://msdn.microsoft.com/library/windows/apps/BR243006) 作为 [**RenderTransform**](https://msdn.microsoft.com/library/windows/apps/BR208980) 的值。 通常会将合适的转换类型放入初始的 XAML，有时并不为该转换设置属性。

通常使用间接的目标设定技术以将动画应用到转换的属性。 有关间接目标设定语法的详细信息，请参阅[情节提要动画](storyboarded-animations.md)和[属性路径语法](https://msdn.microsoft.com/library/windows/apps/Mt185586)。

控件的默认样式有时将转换的动画定义为视觉状态行为的一部分。 比如，适用于 [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/BR227538) 的视觉状态使用动画处理过的 [**RotateTransform**](https://msdn.microsoft.com/library/windows/apps/BR242932) 值以使圆环中的点“旋转”。

以下是如何为转换设置动画的一个简单示例： 在本示例中，为一个 [**RotateTransform**](https://msdn.microsoft.com/library/windows/apps/BR242932) 的 [**Angle**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.rotatetransform.angle.aspx) 创建动画，以使 [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371) 围绕它的视觉中心旋转。 本示例为 **RotateTransform** 命名，因此不需要间接动画目标设定，但你也可以使转换保持未命名状态，而命名应用了转换的元素，并使用间接目标设定，比如 `(UIElement.RenderTransform).(RotateTransform.Angle)`。

```xml
<StackPanel Margin="15">
  <StackPanel.Resources>
    <Storyboard x:Name="myStoryboard">
      <DoubleAnimation
       Storyboard.TargetName="myTransform"
       Storyboard.TargetProperty="Angle"
       From="0" To="360" Duration="0:0:5" 
       RepeatBehavior="Forever" />
    </Storyboard>
  </StackPanel.Resources>
  <Rectangle Width="50" Height="50" Fill="RoyalBlue"
   PointerPressed="StartAnimation">
    <Rectangle.RenderTransform>
      <RotateTransform x:Name="myTransform" Angle="45" CenterX="25" CenterY="25" />
    </Rectangle.RenderTransform>
  </Rectangle>
</StackPanel>
```

```xml
void StartAnimation (object sender, RoutedEventArgs e) {
    myStoryboard.Begin();
}
```

## <a name="span-idaccountingforcoordinateframesofreferenceatruntimespanspan-idaccountingforcoordinateframesofreferenceatruntimespanspan-idaccountingforcoordinateframesofreferenceatruntimespanaccounting-for-coordinate-frames-of-reference-at-run-time"></a><span id="Accounting_for_coordinate_frames_of_reference_at_run_time"></span><span id="accounting_for_coordinate_frames_of_reference_at_run_time"></span><span id="ACCOUNTING_FOR_COORDINATE_FRAMES_OF_REFERENCE_AT_RUN_TIME"></span>运行时的参考坐标系的说明

[**UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911) 具有一个名为 [**TransformToVisual**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.transformtovisual.aspx) 的方法，它可以生成关联两个 UI 元素的参考坐标系的 [**Transform**](https://msdn.microsoft.com/library/windows/apps/BR243006)。 如果将根视觉对象作为第一个参数传递，则可以使用它将元素与应用的默认参考坐标系进行对比。 当从不同元素捕获了输入事件时，或尝试在未实际请求布局传递的情况下预测布局行为时，这可能很有用。

从指针事件获取的事件数据将提供 [**GetCurrentPoint**](https://msdn.microsoft.com/library/windows/apps/BR212141) 方法的访问权限，可以在该方法中指定 *relativeTo* 参数以更改特定元素而非应用默认元素的参考坐标系。 该方法仅仅在内部应用转换，并在创建返回的 [**PointerPoint**](https://msdn.microsoft.com/library/windows/apps/BR242038) 对象时，为你转换 x-y 坐标数据。

## <a name="span-iddescribingatransformmathematicallyspanspan-iddescribingatransformmathematicallyspanspan-iddescribingatransformmathematicallyspandescribing-a-transform-mathematically"></a><span id="Describing_a_transform_mathematically"></span><span id="describing_a_transform_mathematically"></span><span id="DESCRIBING_A_TRANSFORM_MATHEMATICALLY"></span>以数学方式描述转换

转换可以从转换矩阵的方式进行描述。 3×3 矩阵用于描述 2D x-y 平面的转换。 仿射转换矩阵可以相乘，以形成任意数量的线性转换，比如旋转和倾斜（扭曲）并随之转换。 由于仿射转换矩阵的最后一列等于 (0, 0, 1)，因此你仅需要在数学描述中指定前两列的成员。

如果你有数学背景或熟悉图形编程技术（同样使用矩阵描述坐标空间的转换），转换的数学描述就可能对你有用。 有一个 [**Transform**](https://msdn.microsoft.com/library/windows/apps/BR243006) 派生类，可使你能够直接以它的 3×3 矩阵方式表达转换：[**MatrixTransform**](https://msdn.microsoft.com/library/windows/apps/BR210137)。 **MatrixTransform** 具有一个 [**Matrix**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.matrixtransform.matrix.aspx) 属性，该属性保留具有六项属性的结构：[**M11**](https://msdn.microsoft.com/library/windows/apps/Hh673847)、[**M12**](https://msdn.microsoft.com/library/windows/apps/Hh673853)、[**M21**](https://msdn.microsoft.com/library/windows/apps/Hh673851)、[**M22**](https://msdn.microsoft.com/library/windows/apps/Hh673849)、[**OffsetX**](https://msdn.microsoft.com/library/windows/apps/Hh673810) 和 [**OffsetY**](https://msdn.microsoft.com/library/windows/apps/Hh673816)。 每个 [**Matrix**](https://msdn.microsoft.com/library/windows/apps/BR210127) 属性使用 **Double** 值，并且对应于仿射转换矩阵的六个相关值（列 1 和列 2）。

|                                             |                                             |     |
|---------------------------------------------|---------------------------------------------|-----|
| [**M11**](https://msdn.microsoft.com/library/windows/apps/Hh673847)         | [**M21**](https://msdn.microsoft.com/library/windows/apps/Hh673851)         | 0   |
| [**M12**](https://msdn.microsoft.com/library/windows/apps/Hh673853)         | [**M22**](https://msdn.microsoft.com/library/windows/apps/Hh673849)         | 0   |
| [**OffsetX**](https://msdn.microsoft.com/library/windows/apps/Hh673810) | [**OffsetY**](https://msdn.microsoft.com/library/windows/apps/Hh673816) | 1   |

 

任何能够使用 [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027)、[**ScaleTransform**](https://msdn.microsoft.com/library/windows/apps/BR242940)、[**RotateTransform**](https://msdn.microsoft.com/library/windows/apps/BR242932) 或 [**SkewTransform**](https://msdn.microsoft.com/library/windows/apps/BR242950) 对象描述的转换都可以通过 [**MatrixTransform**](https://msdn.microsoft.com/library/windows/apps/BR210137) 使用 [**Matrix**](https://msdn.microsoft.com/library/windows/apps/BR210127) 值等价地描述。 但是通常只使用 **TranslateTransform** 和其他转换类，因为将那些转换类中的属性概念化比设置 **Matrix** 中的矢量组件更简单。 为各种离散的转换属性创建动画也更简单；**Matrix** 事实上是一种结构而不是 [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/BR242356)，所以无法支持创建有动画的个别值。

某些支持你应用转换操作的 XAML 设计工具会将结果序列化为 [**MatrixTransform**](https://msdn.microsoft.com/library/windows/apps/BR210137)。 在此情况下，最好再次使用同一设计工具更改转换效果并重新将 XAML 序列化，而不要尝试直接在 XAML 中自行对 [**Matrix**](https://msdn.microsoft.com/library/windows/apps/BR210127) 值进行操纵。

## <a name="span-id3-dtransformsspanspan-id3-dtransformsspanspan-id3-dtransformsspan3-d-transforms"></a><span id="3-D_transforms"></span><span id="3-d_transforms"></span><span id="3-D_TRANSFORMS"></span>3D 变换

在 Windows 10 中，XAML 引入了可用于与 UI 一起创建 3D 效果的新属性 [**UIElement.Transform3D**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.transform3d.aspx)。 若要执行此操作，请使用 [**PerspectiveTransform3D**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.media3d.perspectivetransform3d.aspx) 将共享的 3D 透视或“相机”添加到场景，然后使用 [**CompositeTransform3D**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.media3d.compositetransform3d.aspx) 在 3D 空间中转换元素，就像使用 [**CompositeTransform**](https://msdn.microsoft.com/library/windows/apps/BR228105) 那样。 有关如何实现 3D 转换的讨论，请参阅 [**UIElement.Transform3D**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.transform3d.aspx)。

 对于仅应用于单个对象的较简单的 3D 效果，可以使用 [**UIElement.Projection**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.projection) 属性。 将 [**PlaneProjection**](https://msdn.microsoft.com/library/windows/apps/br210192) 用作此属性的值等同于将固定视角转换以及一个或多个 3D 转换应用到元素。 [XAML UI 的 3D 透视效果](3-d-perspective-effects.md)中更详细地介绍了此类型的转换。

## <a name="span-idrelatedtopicsspanrelated-topics"></a><span id="related_topics"></span>相关主题

* [绘制形状](drawing-shapes.md)
* [**UIElement.Transform3D**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.transform3d.aspx)
* [XAML UI 的 3D 透视效果](3-d-perspective-effects.md)
* [**转换**](https://msdn.microsoft.com/library/windows/apps/BR243006)
 

 





