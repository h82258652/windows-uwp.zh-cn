---
title: 弹簧动画
description: 了解如何使用弹簧自然运动动画。
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, 动画
ms.localizationpriority: medium
ms.openlocfilehash: f86ab8b3e55b7680c5ba3e47c37d1cda8c42cebb
ms.sourcegitcommit: 05be6929cd380a9dd241cc1298fd53f11c93d774
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2019
ms.locfileid: "73062001"
---
# <a name="spring-animations"></a>弹簧动画

介绍了如何使用弹簧 NaturalMotionAnimation。

## <a name="prerequisites"></a>必备条件

我们在此假设你熟悉这些文章中所述的概念：

- [自然运动动画](natural-animations.md)

## <a name="why-springs"></a>为什么要使用弹簧？

弹簧是一种常见的运动体验，我们都曾经在生活中经历过；从螺旋弹簧玩具到包含弹簧块的物理教室体验。 弹簧的运动不稳定，这通常会为观察它的人带来有趣而令人愉快的情绪反应。 因此，对于那些渴求创建更为生动的运动体验的人来说，弹簧的运动在应用程序 UI 中得到了很好的运用：“弹出”给最终用户，而不是传统的贝塞尔曲线。 在这些情况下，弹簧运动不仅能产生更为生动的运动体验，还能帮助吸引人们注意新的或目前正在显示动画效果的内容。 根据应用程序的品牌或运动语言，弹簧的振荡更加显然而显眼，但在其他情况下，它的运动不明显。

带有弹簧动画的 ![运动](images/animation/offset-spring.gif)
带有三次方贝塞尔动画的![动作](images/animation/offset-cubic-bezier.gif)

## <a name="using-springs-in-your-ui"></a>在 UI 中使用弹簧

如上文所述，弹簧可能是很有用的运动，可集成到应用中，给人们带来非常熟悉又有趣的 UI 体验。 UI 中的常见弹簧用法：

| 弹簧用法描述 | 视觉示例 |
| ------------------------ | -------------- |
| 让运动体验“弹出”并且看起来更生动 （伸缩动画处理） | ![使用弹簧动画的伸缩运动](images/animation/scale-spring.gif) |
| 使运动体验感觉起来更加有力（将 Offset 进行动画处理） | ![使用弹簧动画的 Offset 运动](images/animation/offset-spring.gif) |

在以上每一种情况下，弹簧运动都可通过“弹到”和围绕一个新值振荡或围绕当前值振荡（有初始速度）来触发。

![弹簧动画振荡](images/animation/spring-animation-diagram.png)

## <a name="defining-your-spring-motion"></a>定义弹簧运动

可以通过使用 NaturalMotionAnimation API 创建弹簧体验。 具体来说，基于 Compositor 使用 Create* 方法创建 SpringNaturalMotionAnimation。 然后，可以定义以下运动属性：

- DampingRatio – 表示在动画中使用的弹簧运动的阻尼级别。

| 阻尼比值 | 描述 |
| ------------------- | ----------- |
| DampingRatio = 0 | Undamped – 弹簧将振荡很长时间 |
| 0 < DampingRatio < 1 | Underdamped – 弹簧将由少变多增加振荡幅度。 |
| DampingRatio = 1 | Criticallydamped – 弹簧不会振荡。 |
| DampingRation > 1 | Overdamped – 弹簧将通过突然减速而不振荡快速到达其目的地 |

- Period – 弹簧执行单次振荡所花费的时间。
- Final/Starting Value – 定义弹簧运动的开始和结束位置（如果未定义，则开始值和/或最终值将是当前值）。
- 初始速度 – 运动的编程初始速度。

你还可以定义一组与 KeyFrameAnimation 相同的运动属性：

- DelayTime/DelayBehavior
- StopBehavior

在对 Offset 和 Scale/Size 进行动画处理的常见情况下，Windows 设计团队为不同类型的弹簧的 DampingRatio 和 Period 推荐了以下值：

| 属性 | 普通弹簧 | 带阻尼弹簧 | 较小阻尼弹簧 |
| -------- | ------------- | --------------- | -------------------- |
| 偏移量 | Damping Ratio = 0.8 <br/> Period = 50 ms | Damping Ratio = 0.85 <br/> Period = 50 ms | Damping Ratio = 0.65 <br/> Period = 60 ms |
| Scale/Size | Damping Ratio = 0.7 <br/> Period = 50 ms | Damping Ratio = 0.8 <br/> Period = 50 ms | Damping Ratio = 0.6 <br/> Period = 60 ms |

定义好这些属性之后，可以将弹簧 NaturalMotionAnimation 传入 CompositionObject 的 StartAnimation 或 InteractionTracker InertiaModifier 的 Motion 属性。

## <a name="example"></a>示例

在本示例中，创建导航和画布 UI 体验：当用户单击展开按钮时，导航窗格以震荡运动方式用动画效果弹出。

![单击时的弹簧动画](images/animation/spring-animation-on-click.gif)

首先，在导航窗格显示时在所单击事件内定义弹簧动画。 然后使用 InitialValueExpression 功能通过 Expression 定义 FinalValue，定义动画属性。 此外，还要跟踪窗格是否打开，准备就绪后，启动动画。

```csharp
private void Button_Clicked(object sender, RoutedEventArgs e)
{
 _springAnimation = _compositor.CreateSpringScalarAnimation();
 _springAnimation.DampingRatio = 0.75f;
 _springAnimation.Period = TimeSpan.FromSeconds(0.5);

 if (!_expanded)
 {
 _expanded = true;
 _propSet.InsertBoolean("expanded", true);
 _springAnimation.InitialValueExpression["FinalValue"] = "this.StartingValue + 250";
 } else
 {
 _expanded = false;
 _propSet.InsertBoolean("expanded", false);
_springAnimation.InitialValueExpression["FinalValue"] = "this.StartingValue - 250";
 }
 _naviPane.StartAnimation("Offset.X", _springAnimation);
}
```

现在，如果需要将此运动与输入关联，该怎么办？ 如果最终用户轻扫，窗格是否以弹簧运动出现？ 更重要的是，如果用户更用力或更快地轻扫，运动能够根据最终用户的速度相应调节适应。

![轻扫时的弹簧动画](images/animation/spring-animation-on-swipe.gif)

为此，可以使用此处的弹簧动画，将它传入 InteractionTracker 的 InertiaModifier。 有关 InputAnimation 和 InteractionTracker 的详细信息，请参阅 [InteractionTracker 的自定义操作体验](interaction-tracker-manipulations.md)。 假定本示例代码已经设置了 InteractionTracker 和 VisualInteractionSource。 我们主要介绍如何创建用在 NaturalMotionAnimation 中的 InertiaModifier，本例中是弹簧。

```csharp
// InteractionTracker and the VisualInteractionSource previously setup
// The open and close ScalarSpringAnimations defined earlier
private void SetupInput()
{
 // Define the InertiaModifier to manage the open motion
 var openMotionModifer = InteractionTrackerInertiaNaturalMotion.Create(compositor);

 // Condition defines to use open animation if panes in non-expanded view
 // Property set value to track if open or closed is managed in other part of code
 openMotionModifer.Condition = _compositor.CreateExpressionAnimation(
"propset.expanded == false");
 openMotionModifer.Condition.SetReferenceParameter("propSet", _propSet);
 openMotionModifer.NaturalMotion = _openSpringAnimation;

 // Define the InertiaModifer to manage the close motion
 var closeMotionModifier = InteractionTrackerInertiaNaturalMotion.Create(_compositor);

 // Condition defines to use close animation if panes in expanded view
 // Property set value to track if open or closed is managed in other part of code
 closeMotionModifier.Condition = 
_compositor.CreateExpressionAnimation("propSet.expanded == true");
 closeMotionModifier.Condition.SetReferenceParameter("propSet", _propSet);
 closeMotionModifier.NaturalMotion = _closeSpringAnimation;

 _tracker.ConfigurePositionXInertiaModifiers(new 
InteractionTrackerInertiaNaturalMotion[] { openMotionModifer, closeMotionModifier});

 // Take output of InteractionTracker and assign to the pane
 var exp = _compositor.CreateExpressionAnimation("-tracker.Position.X");
 exp.SetReferenceParameter("tracker", _tracker);
 ElementCompositionPreview.GetElementVisual(pageNavigation).
StartAnimation("Translation.X", exp);
}
```

现在可以在 UI 中实现编程弹簧动画和输入驱动的弹簧动画！

总之，在应用中使用弹簧动画的步骤是：

1. 基于 Compositor 创建 SpringAnimation。
1. 如果需要非默认值，请定义 SpringAnimation 的属性：
    - DampingRatio
    - Period
    - Final Value
    - Initial Value
    - Initial Velocity
1. 分配给目标。
    - 如果要对 CompositionObject 属性进行动画处理，请将 SpringAnimation 作为参数传入 StartAnimation。
    - 如果需要使用输入，则将 InertiaModifier 的 NaturalMotion 属性设置为 SpringAnimation。

