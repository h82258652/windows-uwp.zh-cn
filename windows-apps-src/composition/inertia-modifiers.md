---
title: 使用 Inertia Modifier 创建吸附点
description: 了解如何使用 InteractionTracker 的 InertiaModifier 功能创建吸附到指定点的运动体验。
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, 动画
ms.localizationpriority: medium
ms.openlocfilehash: f99ebc4b98c87a4bc6d77fd2c626f481563e50c5
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2018
ms.locfileid: "8477985"
---
# <a name="create-snap-points-with-inertia-modifiers"></a>使用 Inertia Modifier 创建吸附点

本文将深入探讨如何使用 InteractionTracker 的 InertiaModifier 功能创建吸附到指定点的运动体验。

## <a name="prerequisites"></a>先决条件

我们在此假设你熟悉这些文章中所述的概念：

- [输入驱动的动画](input-driven-animations.md)
- [InteractionTracker 的自定义操作体验](interaction-tracker-manipulations.md)
- [基于关系的动画](relation-animations.md)

## <a name="what-are-snap-points-and-why-are-they-useful"></a>什么是吸附点？为什么说它们很有用？

生成自定义操作体验时，有时它对在可滚动/可缩放的画布内创建专门的_位置点_（InteractionTracker 始终会存放在此）很有用。 这些点通常称为_吸附点_。

请注意，在下面的示例中，滚动时 UI 可能停留在两幅不同图像之间一个尴尬的位置：

![无吸附点的滚动](images/animation/snap-points-none.gif)

如果添加吸附点，则在两个图像之间停止滚动时，它们会“吸附”到指定位置。 使用吸附点，图像的滚动体验就利落得多，响应也更迅速。

![使用一个吸附点的滚动](images/animation/snap-points-single.gif)

## <a name="interactiontracker-and-inertiamodifiers"></a>InteractionTracker 和 InertiaModifier

使用 InteractionTracker 生成自定义的操作体验时，可以通过利用 InertiaModifier 创建吸附点运动体验。 InertiaModifier 实际上是你可用来定义 InteractionTracker 在进入 Inertia 状态时从何处或如何到达其目的地的一种方法。 可以应用 InertiaModifier 影响 InteractionTracker 的 X 或 Y 位置或 Scale 属性。

存在 3 种类型的 InertiaModifier：

- InteractionTrackerInertiaRestingValue – 一种用于修改在一次交互或以编程速度运动后的最终停止位置的方法。 一个预定义的运动将使 InteractionTracker 到达该位置。
- InteractionTrackerInertiaMotion – 一种用于定义 InteractionTracker 将在一次交互或以编程速度运动后执行的特定运动的方法。 最终位置将基于此运动产生。
- InteractionTrackerInertiaNaturalMotion – 一种定义在一次互动或以编程速度运动（但有基于动画的物理特性，即 NaturalMotionAnimation）后的最终停止位置的方法。

进入 Inertia 状态时, InteractionTracker 会计算分配给它的每个 InertiaModifier，并确定其中是否有任何一个 InertiaModifier 适用。 这就是说，你也可以创建多个 InertiaModifier 并将其分配给 InteractionTracker，但是，在定义每个 InertiaModifier 时，都需要执行以下操作：

1. 定义条件 – 一个表达式，它定义何时应使用此特定 InertiaModifier 的条件语句。 这通常需要查看 InteractionTracker 的 NaturalRestingPosition（采用默认 Inertia 时的目的地）。
1. 定义 RestingValue/Motion/NaturalMotion – 定义条件满足时的实际 Resting Value Expression、Motion Expression 或 NaturalMotionAnimation。

> [!NOTE]
> 当 InteractionTracker 进入 Inertia 状态时，仅对 InertiaModifier 条件进行一次计算。 但是，仅对于 InertiaMotion，对于条件为真的修饰符，每一帧都会对该 Motion Expression 求值。

## <a name="example"></a>示例

现在我们看看可以如何使用 InertiaModifier 创建吸附点体验，以重新创建图像的滚动画布。 在本示例中，每个操作都旨在潜在移动一幅图像，这通常称为单强制性吸附点。

让我们首先来设置将利用 InteractionTracker 位置的 InteractionTracker、VisualInteractionSource 以及 Expression。

```csharp
private void SetupInput()
{
    _tracker = InteractionTracker.Create(_compositor);
    _tracker.MinPosition = new Vector3(0f);
    _tracker.MaxPosition = new Vector3(3000f);

    _source = VisualInteractionSource.Create(_root);
    _source.ManipulationRedirectionMode =
        VisualInteractionSourceRedirectionMode.CapableTouchpadOnly;
    _source.PositionYSourceMode = InteractionSourceMode.EnabledWithInertia;
    _tracker.InteractionSources.Add(_source);

    var scrollExp = _compositor.CreateExpressionAnimation("-tracker.Position.Y");
    scrollExp.SetReferenceParameter("tracker", _tracker);
    ElementCompositionPreview.GetElementVisual(scrollPanel).StartAnimation("Offset.Y", scrollExp);
}
```

接下来，因为单强制性吸附点行为将上下移动内容，你需要两种不同的 Inertia Modifier：一个将可滚动内容上移，一个将其下移。

```csharp
// Snap-Point to move the content up
var snapUpModifier = InteractionTrackerInertiaRestingValue.Create(_compositor);
// Snap-Point to move the content down
var snapDownModifier = InteractionTrackerInertiaRestingValue.Create(_compositor);
```

要向上还是向下吸附取决于 InteractionTracker 会自然着落于相对吸附距离的何处 - 这个距离即吸附位置之间的距离。 如果超过中点，则向下吸附，否则向上吸附。 （本示例中，在 PropertySet 中存储吸附距离）

```csharp
// Is NaturalRestingPosition less than the halfway point between Snap Points?
snapUpModifier.Condition = _compositor.CreateExpressionAnimation(
"this.Target.NaturalRestingPosition.y < (this.StartingValue – ” + “mod(this.StartingValue, prop.snapDistance) + prop.snapDistance / 2)");
snapUpModifier.Condition.SetReferenceParameter("prop", _propSet);
// Is NaturalRestingPosition greater than the halfway point between Snap Points?
snapDownModifier.Condition = _compositor.CreateExpressionAnimation(
"this.Target.NaturalRestingPosition.y >= (this.StartingValue – ” + “mod(this.StartingValue, prop.snapDistance) + prop.snapDistance / 2)");
snapDownModifier.Condition.SetReferenceParameter("prop", _propSet);
```

此图直观地描述了所发生的事情：

![Inertia Modifier 图](images/animation/inertia-modifier-diagram.png)

现在只需为每个 InertiaModifier 定义 RestingValue：将 InteractionTracker 的位置移到上一个或下一个吸附位置。

```csharp
snapUpModifier.RestingValue = _compositor.CreateExpressionAnimation(
"this.StartingValue - mod(this.StartingValue, prop.snapDistance)");
snapUpModifier.RestingValue.SetReferenceParameter("prop", _propSet);
snapForwardModifier.RestingValue = _compositor.CreateExpressionAnimation(
"this.StartingValue + prop.snapDistance - mod(this.StartingValue, ” + “prop.snapDistance)");
snapForwardModifier.RestingValue.SetReferenceParameter("prop", _propSet);
```

最后，将 InertiaModifier 添加到 InteractionTracker。 现在，当 InteractionTracker 进入 InertiaState 时，它将检查 InertiaModifier 以查看其位置是否应修改。

```csharp
var modifiers = new InteractionTrackerInertiaRestingValue[] { 
snapUpModifier, snapDownModifier };
_tracker.ConfigurePositionYInertiaModifiers(modifiers);
```