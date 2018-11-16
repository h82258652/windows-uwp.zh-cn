---
author: jwmsft
title: 使用 SourceModifier 的下拉刷新
description: 使用 SourceModifier 创建自定义的下拉刷新控件
ms.author: jimwalk
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, 动画
ms.localizationpriority: medium
ms.openlocfilehash: 997082d2ed7375d99a7be1543901d1dd854be1a0
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2018
ms.locfileid: "6838526"
---
# <a name="pull-to-refresh-with-source-modifiers"></a>使用 SourceModifier 的下拉刷新

在本文中，我们将深入探讨如何使用 InteractionTracker 的 SourceModifier 功能，并通过创建自定义的下拉刷新控件展示其用法。

## <a name="prerequisites"></a>先决条件

我们在此假设你熟悉这些文章中所述的概念：

- [输入驱动的动画](input-driven-animations.md)
- [InteractionTracker 的自定义操作体验](interaction-tracker-manipulations.md)
- [基于关系的动画](relation-animations.md)

## <a name="what-is-a-sourcemodifier-and-why-are-they-useful"></a>什么是 SourceModifier？为什么它们很有用？

与 [InertiaModifier](inertia-modifiers.md) 一样，SourceModifier 提供对 InteractionTracker 运动更精细的控制。 但是与定义 InteractionTracker 进入 Inertia 状态后的运动的 InertiaModifier 不同，SourceModifier 定义 InteractionTracker 仍处于交互状态下时的运动。 在这些情况下，需要获得与传统的“跟随手指而动”不同的体验。

经典示例是下拉刷新体验 - 当用户拉动列表以刷新列表内容时，列表按手指移动速度平移，并在特定距离后停止，这种运动让人感觉突然而机械。 更为自然的体验将是当用户活跃地与列表交互时引入一种阻力感。 这样细小的差别有助于让最终用户与列表的整体体验变得更加动态和具有吸引力。 在“示例”部分中，我们更详细介绍了如何构建这样的细微差别。

有 2 种类型的 SourceModifier：

- DeltaPosition – 是触摸平移交互期间手指的当前帧位置与之前的帧位置之间的变化量。 此 SourceModifier 允许在将变化位置发送出去进行进一步处理之前对其进行修改。 这是 Vector3 类型的参数，开发人员可以选择它在传递到 InteractionTracker 之前修改位置的任意 X、Y 或 Z 属性。
- DeltaScale - 是在触摸缩放互动期间在当前帧大小与之前应用的帧大小之间的变化量。 此 SourceModifier 允许修改交互的缩放级别。 这是开发人员可以在传递到 InteractionTracker 之前修改的浮点类型属性。

当 InteractionTracker 处于 Interacting 状态时，它会计算分配给它的每个 SourceModifier，并确定其中是否有任何一个 SourceModifier 适用。 这意味着可以创建多个 SourceModifier 并将它们分配到一个 InteractionTracker。 但是，定义每个 SourceModifier 时，需要执行以下操作：

1. 定义条件 – 一个 Expression，它定义应何时应用此特定 SourceModifier 的条件语句。
1. 定义 DeltaPosition/DeltaScale – 在满足以上定义的条件时改变 DeltaPosition 或 DeltaScale 的 SourceModifier Expression。

## <a name="example"></a>示例

现在我们来看看如何使用 SourceModifier 来使用现有的 XAML ListView 控件创建自定义下拉刷新体验。 我们将使用 Canvas 作为“刷新面板”，它将堆叠在 XAML 之上以生成此体验。

为改进最终用户体验，我们需要在用户活跃地平移列表（通过触摸）并在其位置超出特定点后停止平移时产生“阻力”效果。

![具有下拉刷新效果的列表](images/animation/city-list.gif)

在 [GitHub 上的窗口 UI 开发实验室存储库](https://github.com/Microsoft/WindowsUIDevLabs)中可以找到实现此体验的可行代码。 下面是生成上述体验的分步演练。
在 XAML 标记代码中，有以下内容：

```xaml
<StackPanel Height="500" MaxHeight="500" x:Name="ContentPanel" HorizontalAlignment="Left" VerticalAlignment="Top" >
 <Canvas Width="400" Height="100" x:Name="RefreshPanel" >
<Image x:Name="FirstGear" Source="ms-appx:///Assets/Loading.png" Width="20" Height="20" Canvas.Left="200" Canvas.Top="70"/>
 </Canvas>
 <ListView x:Name="ThumbnailList"
 MaxWidth="400"
 Height="500"
ScrollViewer.VerticalScrollMode="Enabled" ScrollViewer.IsScrollInertiaEnabled="False" ScrollViewer.IsVerticalScrollChainingEnabled="True" >
 <ListView.ItemTemplate>
 ……
 </ListView.ItemTemplate>
 </ListView>
</StackPanel>
```

因为 ListView (`ThumbnailList`) 是已滚动的 XAML 控件，需要在它到达顶部项目不能再滚动时让滚动链接到其父级 (`ContentPanel`)。 （ContentPanel 是应用 SourceModifier 的位置。）为此，需要在 ListView 标记中将 ScrollViewer.IsVerticalScrollChainingEnabled 设置为 **true**。 你还需要在 VisualInteractionSource 上将链接模式设置为 **Always**。

需要使用 _handledEventsToo_ 将 PointerPressedEvent 处理程序设置为 **true**。 如果没有此选项，PointerPressedEvent 不会链接到 ContentPanel，因为 ListView 控件将这些事件标记为已处理，因此不会发送到视觉链上。

```csharp
//The PointerPressed handler needs to be added using AddHandler method with the //handledEventsToo boolean set to "true"
//instead of the XAML element's "PointerPressed=Window_PointerPressed",
//because the list view needs to chain PointerPressed handled events as well.
ContentPanel.AddHandler(PointerPressedEvent, new PointerEventHandler( Window_PointerPressed), true);
```

现在，可以将它与 InteractionTracker 关联。 首先设置 InteractionTracker、将利用 InteractionTracker 位置的 VisualInteractionSource 以及 Expression。

```csharp
// InteractionTracker and VisualInteractionSource setup.
_root = ElementCompositionPreview.GetElementVisual(Root);
_compositor = _root.Compositor;
_tracker = InteractionTracker.Create(_compositor);
_interactionSource = VisualInteractionSource.Create(_root);
_interactionSource.PositionYSourceMode = InteractionSourceMode.EnabledWithInertia;
_interactionSource.PositionYChainingMode = InteractionChainingMode.Always;
_tracker.InteractionSources.Add(_interactionSource);
float refreshPanelHeight = (float)RefreshPanel.ActualHeight;
_tracker.MaxPosition = new Vector3((float)Root.ActualWidth, 0, 0);
_tracker.MinPosition = new Vector3(-(float)Root.ActualWidth, -refreshPanelHeight, 0);

// Use the Tacker's Position (negated) to apply to the Offset of the Image.
// The -{refreshPanelHeight} is to hide the refresh panel
m_positionExpression = _compositor.CreateExpressionAnimation($"-tracker.Position.Y - {refreshPanelHeight} ");
m_positionExpression.SetReferenceParameter("tracker", _tracker);
_contentPanelVisual.StartAnimation("Offset.Y", m_positionExpression);
```

在设置之后，刷新面板将退出视区回到开始位置，用户看到的是在平移到达 ContentPanel 时的 ListView，将引发 PointerPressed 事件，在该事件中，要求系统使用 InteractionTracker 来驱动操作体验。

```csharp
private void Window_PointerPressed(object sender, PointerRoutedEventArgs e)
{
if (e.Pointer.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Touch) {
 // Tell the system to use the gestures from this pointer point (if it can).
 _interactionSource.TryRedirectForManipulation(e.GetCurrentPoint(null));
 }
}
```

> [!NOTE]
> 如果不需要链接 Handled 事件，添加 PointerPressedEvent 处理程序可以使用属性 (`PointerPressed="Window_PointerPressed"`) 通过 XAML 标记直接完成。

下一步是设置 SourceModifier。 将使用 2 个 SourceModifier 来获取此行为；_Resistance_ 和 _Stop_。

- Resistance – 以半速移动 DeltaPosition.Y，直至它到达 RefreshPanel 的高度。

```csharp
CompositionConditionalValue resistanceModifier = CompositionConditionalValue.Create (_compositor);
ExpressionAnimation resistanceCondition = _compositor.CreateExpressionAnimation(
 $"-tracker.Position.Y < {pullToRefreshDistance}");
resistanceCondition.SetReferenceParameter("tracker", _tracker);
ExpressionAnimation resistanceAlternateValue = _compositor.CreateExpressionAnimation(
 "source.DeltaPosition.Y / 3");
resistanceAlternateValue.SetReferenceParameter("source", _interactionSource);
resistanceModifier.Condition = resistanceCondition;
resistanceModifier.Value = resistanceAlternateValue;
```

- Stop – 在整个 RefreshPanel 已显示在屏幕上时停止移动。

```csharp
CompositionConditionalValue stoppingModifier = CompositionConditionalValue.Create (_compositor);
ExpressionAnimation stoppingCondition = _compositor.CreateExpressionAnimation(
 $"-tracker.Position.Y >= {pullToRefreshDistance}");
stoppingCondition.SetReferenceParameter("tracker", _tracker);
ExpressionAnimation stoppingAlternateValue = _compositor.CreateExpressionAnimation("0");
stoppingModifier.Condition = stoppingCondition;
stoppingModifier.Value = stoppingAlternateValue;
Now add the 2 source modifiers to the InteractionTracker.
List<CompositionConditionalValue> modifierList = new List<CompositionConditionalValue>()
{ resistanceModifier, stoppingModifier };
_interactionSource.ConfigureDeltaPositionYModifiers(modifierList);
```

此图表提供了 SourceModifiers 设置的可视化效果。

![平移图](images/animation/source-modifiers-diagram.png)

现在使用 SourceModifier，你将注意到向下平移 ListView 并到达顶部项目时，刷新面板将以平移的半速下拉，直至到达 RefreshPanel 高度，然后停止移动。

在完整示例中，在交互期间使用关键帧动画在 RefreshPanel 画布中旋转图标。 任何内容都可以就地使用，或使用 InteractionTracker 的位置单独驱动该动画效果。
