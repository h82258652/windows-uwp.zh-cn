---
title: 基于指针的动画
description: 了解如何使用指针位置来创建动态的“紧随光标”体验。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 动画
ms.localizationpriority: medium
ms.openlocfilehash: 3512d47c8b3e689b0baadec26c1d8f0f510e03ef
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2018
ms.locfileid: "8704209"
---
# <a name="pointer-based-animations"></a>基于指针的动画

本文介绍了如何使用指针位置来创建动态的“紧随光标”体验。

## <a name="prerequisites"></a>先决条件

我们在此假设你熟悉这些文章中所述的概念：

- [输入驱动的动画](input-driven-animations.md)
- [基于关系的动画](relation-animations.md)

## <a name="why-create-pointer-position-driven-experiences"></a>为何要创建指针位置驱动的体验？

在 Fluent 设计语言中，触摸并不是与 UI 交互的唯一方式。 因为 UWP 支持跨多种设备，最终用户会使用其他输入形式（如鼠标和触控笔）与应用交互。 通过这些输入形式使用位置数据，可能可以让最终用户感受到与应用产生更多联系。

指针位置驱动的体验让你能够充分利用指针在屏幕上的位置，为应用创建更多的运动和 UI 体验。 这些体验通常可以为最终用户提供有关 UI 行为与结构的更多上下文和反馈。 这种体验不再是单向的信息流，而开始成为一种双向的信息流：最终用户通过他们的输入形式提供输入，应用 UI 给予响应。

一些示例如下：

- 将焦点位置进行动画处理以跟随光标

    ![指针聚焦示例](images/animation/spotlight-reveal.gif)

- 基于指针位置旋转图像

    ![指针旋转示例](images/animation/pointer-rotate.gif)

## <a name="using-pointerpositionpropertyset"></a>使用 PointerPositionPropertySet

可以使用 PointerPositionPropertySet 来创建这些体验。 此 PropertySet 将为 UIElement 创建，以便在 UIElement 进行积极命中测试时保持指针的位置。 位置值为相对于 UIElement 坐标空间的值（位置 <0,0> 在 UIElement 的左上角）。 然后，可以利用在 Animation 中的此属性集来驱动另一个属性的运动。

对于每个不同的指针输入形式，位置变化时输入可能有多种输入状态：Hover、Pressed、Pressed&Moved。 PointerPositionPropertySet 仅对鼠标和触控笔将指针保留在 Hover、Pressed 和 Pressed and Moved 状态。

一般的开始步骤：

1. 确定你希望跟踪指针位置的 UIElement。
1. 通过 ElementCompositionPreview 访问 PointerPositionPropertySet。
    - 将 UIElement 传入 ElementCompositionPreview.GetPointerPositionPropertySet 方法。
1. 创建引用 PropertySet 中的 Position 属性的 ExpressionAnimation。
    - 不要忘了设置参考参数！
1. 将 CompositionObject 的属性作为 ExpressionAnimation 的操作目标。

> [!NOTE]
> 建议将从 GetPointerPositionPropertySet 方法返回的 PropertySet 分配给类变量。 这将确保该属性集不会作为垃圾回收，因此对于引用了它的 ExpressionAnimation 不会产生任何影响。 ExpressionAnimation 不维护对等式中使用的任何对象的强引用。

## <a name="example"></a>示例

我们看一个示例，利用鼠标和触控笔输入形式的 Hover 位置动态旋转图像。

![指针旋转示例](images/animation/pointer-rotate.gif)

该图像是一个 UIElement，因此我们首先获取对 PointerPositionPropertySet 的引用

```csharp
_pointerPositionPropSet = ElementCompositionPreview.GetPointerPositionPropertySet(UIElement element);
```

在本示例中，有两个相关 Expression：

- 第一个 Expression，图像基于指针距离图像中心的距离进行旋转。 距离越远，旋转越大。
- 另一个 Expression，旋转轴基于指针位置变化。 你希望旋转轴与位置矢量垂直。

我们定义两个 Expression，其中一个以 RotationAngle 属性为目标，另一个以 RotationAxis 属性为目标。 像引用任何其他 PropertySet 一样引用 PointerPositionPropertySet。

在本示例中，使用 ExpressionBuilder 类生成 Expression。

```csharp
// || DEFINE THE EXPRESSION FOR THE ROTATION ANGLE ||
var hoverPosition = _pointerPositionPropSet.GetSpecializedReference
<PointerPositionPropertySetReferenceNode>().Position;
var angleExpressionNode =
EF.Conditional(
 hoverPosition == new Vector3(0, 0, 0),
 ExpressionValues.CurrentValue.CreateScalarCurrentValue(),
 35 * ((EF.Clamp(EF.Distance(center, hoverPosition), 0, distanceToCenter) % distanceToCenter) / distanceToCenter));
_tiltVisual.StartAnimation("RotationAngleInDegrees", angleExpressionNode);

// || DEFINE THE EXPRESSION FOR THE ROTATION AXIS ||
var axisAngleExpressionNode = EF.Vector3(
-(hoverPosition.Y - center.Y) * EF.Conditional(hoverPosition.Y == center.Y, 0, 1),
 (hoverPosition.X - center.X) * EF.Conditional(hoverPosition.X == center.X, 0, 1),
 0);
_tiltVisual.StartAnimation("RotationAxis", axisAngleExpressionNode);
```