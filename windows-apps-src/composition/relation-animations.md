---
author: jwmsft
title: 基于关系的动画
description: 基于其他对象的属性创建运动
ms.author: jimwalk
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, 动画
ms.localizationpriority: medium
ms.openlocfilehash: cde3868d1a554396bfda7c13ea0c71bd037416bc
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2018
ms.locfileid: "6975978"
---
# <a name="relation-based-animations"></a>基于关系的动画

本文简要概述了如何使用 Composition ExpressionAnimation 制作基于关系的动画。

## <a name="dynamic-relation-based-experiences"></a>基于关系的动态体验

在应用中构建运动体验时，有时运动不基于时间，而是依赖于另一个对象的属性。 KeyFrameAnimation 无法非常轻松表示这些类型的运动体验。 在这些特定实例中，运动不再需要是离散和预定义的。 运动可以根据与其他对象属性的关系动态调整。 例如，可以基于对象的水平位置对其不透明度进行动画处理。 其他示例包括粘滞标头和视差等运动体验。

这些类型的运动体验可用于创建让人感觉更加关联的 UI，而不是感到单一和独立。 对用户来说，会给他们留下动态 UI 体验的印象。

![轨道圈](images/animation/orbit.gif)

![含视差效果的列表视图](images/animation/parallax.gif)

## <a name="using-expressionanimations"></a>使用 ExpressionAnimation

要创建基于关系的运动体验，可以使用 ExpressionAnimation 类型。 ExpressionAnimation（或简称 Expression）是一种新动画，可以用来表示数学关系 - 系统使用这种关系来计算动画属性每一帧的值。 换言之，Expression 只是用来定义每帧的所需动画属性值的数学等式。 Expression 是一种用途多样化的组件，它可以在各种各样的情况下使用，包括：

- 相对大小、偏移量动画。
- 粘滞标头，ScrollViewer 视差。 （请参阅[增强现有 ScrollViewer 体验](scroll-input-animations.md)。）
- InertiaModifiers 和 InteractionTracker 吸附点。 （请参阅[使用 Inertia Modifier 创建吸附点](inertia-modifiers.md)。）

在使用 ExpressionAnimation 时，有几点需要提前说明：

- 永不结束 – 与 KeyFrameAnimation 不同，Expression 没有有限的持续时间。 由于 Expression 是数学关系，它们是不断“运行”的动画。 你可以选择停止这些动画。
- 一直运行，但不会始终计算 – 对于不断运行的动画，性能一直都是人们所关心的问题。 但是无需担忧，该系统非常智能，Expression 将仅在有任何输入或参数发生变化的情况下才重新计算。
- 解析为正确的对象类型 - 因为 Expression 是数学关系，所以一定要确保定义 Expression 的等式将解析为作为动画目标的相同属性类型。 例如，如果对 Offset 进行动画处理，Expression 应该解析为 Vector3 类型。

### <a name="components-of-an-expression"></a>Expression 组成部分

构建 Expression 的数学关系时，需要包含几个核心的部分：

- 参数 – 表示常量值或对其他 Composition 对象的引用的值。
- 算术运算符 – 将各个参数连接起来形成等式的典型算术运算符：加号 (+)、减号 (-)、乘号 (*)、除号(/)。 还包含条件运算符，如大于号 (>)、等于号 (==)、三元运算符 (condition ? ifTrue ：ifFalse) 等。
- 数学函数 – 基于 System.Numeric 的数学函数/快捷键。 有关受支持函数的完整列表，请参阅 [ExpressionAnimation](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.ExpressionAnimation)。

Expression 还支持一系列关键字 - 一种特殊的短语，仅在 ExpressionAnimation 系统中有不同含义。 这些关键字（以及数学函数的完整列表）都在 [ExpressionAnimation](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.ExpressionAnimation) 文档中列出。

### <a name="creating-expressions-with-expressionbuilder"></a>使用 ExpressionBuilder 创建 Expression

在 UWP 应用中生成 Expression 有两种选项：

1. 通过官方的公共 API 以字符串形式构建等式。
1. 通过开源 ExpressionBuilder 工具在类型安全的对象模型中生成等式。 请参阅 [Github 源代码和文档](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/ExpressionBuilder)。

为使该文档表述清晰，我们将使用 ExpressionBuilder 定义我们的 Expression。

### <a name="parameters"></a>参数

参数构成了 Expression 的核心。 有两种类型的参数：

- 常量：以下是表示键入的 System.Numeric 变量的参数。 动画启动时，这些参数将获得值。
- 引用：这些参数表示对 CompositionObject 的引用 – 在动画启动之后，这些参数不断更新其值。

一般情况下，引用是 Expression 输出可能动态变化的主要方面。 在这些引用发生更改时，Expression 的输出会随之更改。 如果使用 String 来创建 Expression 或在模板应用场景中使用它们（使用 Expression 将多个 CompositionObject 作为目标），则需要为参数提供名称并设置其值。 请参阅“示例”部分了解更多信息。

### <a name="working-with-keyframeanimations"></a>处理 KeyFrameAnimation

Expression 还可用于 KeyFrameAnimation。 在这些情况下，有时需要使用 Expression 来定义 KeyFrame 的值，这些类型的 KeyFrame 称为 ExpressionKeyFrame。

```csharp
KeyFrameAnimation.InsertExpressionKeyFrame(Single, String)
KeyFrameAnimation.InsertExpressionKeyFrame(Single, ExpressionNode)
```

但是，与 ExpressionAnimation 不同，ExpressionKeyFrame 只在 KeyFrameAnimation 启动时计算一次。 请注意，不能将 ExpressionAnimation 作为 KeyFrame 的值传入，而应该传入字符串（如果使用 ExpressionBuilder，则是 ExpressionNode）。

## <a name="example"></a>示例

现在，逐步查看一个使用 Expression 的示例，具体而言，是 Windows UI 示例库的 PropertySet 示例。 我们将看看管理蓝球轨道运动行为的 Expression。

![轨道圈](images/animation/orbit.gif)

有三个组件为整体的体验做贡献：

1. KeyFrameAnimation，将红球的 Y Offset 进行动画处理。
1. 包含 **Rotation** 属性的 PropertySet 将帮助驱动轨道，由另一个 KeyFrameAnimation 进行动画处理。
1. 驱动蓝球 Offset 的 ExpressionAnimation，引用红球的 Offset 和 Rotation 属性来维持完美运行的轨道。

我们将着重介绍第三点中所定义的 ExpressionAnimation。 我们还将使用 ExpressionBuilder 类来构建此 Expression。 在结尾处列出了用来通过 String 打造这种体验的代码的副本。

在此等式中有两个需要从 PropertySet 引用的属性；一个是中心点偏移，另一个是旋转。

```
var propSetCenterPoint =
_propertySet.GetReference().GetVector3Property("CenterPointOffset");

// This rotation value will animate via KFA from 0 -> 360 degrees
var propSetRotation = _propertySet.GetReference().GetScalarProperty("Rotation");
```

接下来，需要为实际轨道旋转定义 Vector3 组件。

```
var orbitRotation = EF.Vector3(
    EF.Cos(EF.ToRadians(propSetRotation)) * 150,
    EF.Sin(EF.ToRadians(propSetRotation)) * 75, 0);
```

> [!NOTE]
> `EF` 是一个速记使用符号，用于定义 ExpressionBuilder.ExpressionFunction。

最后，将这些部分组合起来，引用红球的位置，定义数学关系。

```
var orbitExpression = redSprite.GetReference().Offset + propSetCenterPoint + orbitRotation;
blueSprite.StartAnimation("Offset", orbitExpression);
```

在假设情况下，如果需要使用这个相同的 Expression，但使用两个其他的 Visual，则意味着会有两套轨道圈。 借助 CompositionAnimation，可以重新使用该动画，并以多个 CompositionObject 作为目标。 对其他轨道情况使用此 Expression 时，只需要更改对 Visual 的引用。 我们称为利用模板。

在本例中，要修改之前生成的 Expression。 你不是“获取”对 CompositionObject 的引用，而是使用一个名称创建引用，然后对它分配不同的值：

```
var orbitExpression = ExpressionValues.Reference.CreateVisualReference("orbitRoundVisual");
orbitExpression.SetReferenceParameter("orbitRoundVisual", redSprite);
blueSprite.StartAnimation("Offset", orbitExpression);
// Later on … use same Expression to assign to another orbiting Visual
orbitExpression.SetReferenceParameter("orbitRoundVisual", yellowSprite);
greenSprite.StartAnimation("Offset", orbitExpression);
```

如果通过公共 API 使用字符串定义 Expression，下面提供了代码。

```
ExpressionAnimation expressionAnimation =
compositor.CreateExpressionAnimation("visual.Offset + " +
"propertySet.CenterPointOffset + " +
"Vector3(cos(ToRadians(propertySet.Rotation)) * 150," + "sin(ToRadians(propertySet.Rotation)) * 75, 0)");
 var propSetCenterPoint = _propertySet.GetReference().GetVector3Property("CenterPointOffset");
 var propSetRotation = _propertySet.GetReference().GetScalarProperty("Rotation");
expressionAnimation.SetReferenceParameter("propertySet", _propertySet);
expressionAnimation.SetReferenceParameter("visual", redSprite);
```
