---
title: 时间动画
description: 使用 KeyFrameAnimation 类可随着时间推移更改 UI。
ms.date: 12/12/2018
ms.topic: article
keywords: windows 10, uwp, 动画
ms.localizationpriority: medium
ms.openlocfilehash: 838a8c3a6dfe89de49fddefd28c53cea563408cf
ms.sourcegitcommit: dcff44885956094e0a7661b69d54a8983921ce62
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/13/2018
ms.locfileid: "8968571"
---
# <a name="time-based-animations"></a>基于时间的动画

当有组件加入，或整体用户体验改变时，最终用户通常会以两种方式观察到：随时间推移的变化或瞬时变化。 在 Windows 平台上，前者是首选后者-立即频繁更改的用户体验感到困惑和惊讶，因为他们无法马上发生了什么事情明白最终用户。 然后最终用户会觉得该体验不协调、不自然。

你可以用一段时间来慢慢改变 UI，慢慢引导最终用户，或通知他们其试用体验会有哪些改变。 在 Windows 平台上，这可通过基于时间的动画（也称为 KeyFrameAnimations）来实现。 KeyFrameAnimations 允许随时间推移更改 UI，并控制动画的每一个方面，包括动画启动的方式和时间，以及它如何达到结束状态。 例如，用 300 毫秒将一个对象以动画形式移动到一个新位置，比起“瞬间挪移”要更讨喜。 当使用动画而不是即时更改时，最终结果是更让人愉快和有吸引力。

## <a name="types-of-time-based-animations"></a>基于时间的动画的类型

在 Windows 上，可以使用两种基于时间的动画来创造卓越的用户体验。

**显式动画** - 顾名思义，显式启动动画来进行更新。
**隐式动画** - 在满足条件时，这些动画由系统代表你启动。

本文讨论如何使用 KeyFrameAnimations 创建和使用基于时间的_显式_动画。

对于显式和隐式的基于时间的动画有不同的类型，分别对应于可以进行动画处理的不同类型的 CompositionObject 属性。

- ColorKeyFrameAnimation
- QuaternionKeyFrameAnimation
- ScalarKeyFrameAnimation
- Vector2KeyFrameAnimation
- Vector3KeyFrameAnimation
- Vector4KeyFrameAnimation

## <a name="create-time-based-animations-with-keyframeanimations"></a>使用 KeyFrameAnimations 创建基于时间的动画

在开始介绍如何使用 KeyFrameAnimations 创建基于时间的显式动画之前，简单讲几个概念。

- 关键帧 - 这些是展示动画效果时将使用的单独的“快照”。
  - 定义为密钥值对。 键表示 0 到 1 之间的进度，即这个“快照”出现在动画生命周期中的位置。 另一个参数表示此时的属性值。
- KeyFrameAnimation 属性 – 可以应用来满足 UI 要求的自定义选项。
  - DelayTime – 在 StartAnimation 调用后到动画启动前这段时间。
  - Duration – 动画的时长。
  - IterationBehavior – 动画的计数或无限重复行为。
  - IterationCount – 关键帧动画将重复的有限次数。
  - KeyFrame Count – 特定关键帧动画中的关键帧读数。
  - StopBehavior – 用于在调用 StopAnimation 后指定设置动画的属性值的行为。
  - Direction – 指定动画的播放方向。
- Animation Group – 同时开始几个动画。
  - 通常在需要同时对多个属性进行动画处理时使用。

有关详细信息，请参阅 [CompositionAnimationGroup](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionanimationgroup)。

请记住这些概念，我们现在开始介绍构造 KeyFrameAnimation 的常规准则：

1. 确定需要实现的动画效果的 CompositionObject 及其对应属性。
1. 为与需要实现动画效果的属性类型匹配的 compositor 创建 KeyFrameAnimation 类型的模板。
1. 使用动画模板，开始添加关键帧和定义动画属性。
    - 至少需要有一个关键帧（100% 或 1f 关键帧）。
    - 建议同时定义持续时间。
1. 一次可以随时运行此动画然后 StartAnimation(...)，对 CompositionObject 调用面向你想要设置动画的属性。 特别是：
    - `visual.StartAnimation("targetProperty", CompositionAnimation animation);`
    - `visual.StartAnimationGroup(AnimationGroup animationGroup);`
1. 如果你有正在运行的动画，并且你想要停止动画或动画组，你可以使用这些 Api:
    - `visual.StopAnimation("targetProperty");`
    - `visual.StopAnimationGroup(AnimationGroup AnimationGroup);`

让我们来看一看示例，看看这个准则的应用情况。

## <a name="example"></a>示例

在此示例中，你想要设置动画的 < 200,0,0 > 对从 < 0,0,0 > 视觉偏移量超过 1 秒。 此外，需要看 10 次在这些位置之间的视觉动画。

![关键帧动画](images/animation/animated-rectangle.gif)

首先确定需要进行动画处理的 CompositionObject 和属性。 在本例中，红色方框用名为 `redVisual` 的 Composition Visual 表示。 从此对象启动动画。

接下来，因为要对 Offset 属性进行动画处理，所以需要创建 Vector3KeyFrameAnimation（Offset 为 Vector3 类型）。 你还可以为 KeyFrameAnimation 定义 KeyFrame。

```csharp
    Vector3KeyFrameAnimation animation = compositor.CreateVector3KeyFrameAnimation();
    animation.InsertKeyFrame(1f, new Vector3(200f, 0f, 0f));
```

然后，你定义 KeyFrameAnimation 来描述其持续时间以及行为之间的两个位置 （当前位置和 < 200,0,0 >） 10 次动画的属性。

```csharp
    animation.Duration = TimeSpan.FromSeconds(2);
    animation.Direction = Windows.UI.Composition.AnimationDirection.Alternate;
    // Run animation for 10 times
    animation.IterationCount = 10;
```

最后，为了运行动画，需要对 CompositionObject 的属性启动它。

```csharp
redVisual.StartAnimation("Offset", animation);
```

完整代码如下。

```csharp
private void AnimateSquare(Compositor compositor, SpriteVisual redVisual)
{ 
    Vector3KeyFrameAnimation animation = compositor.CreateVector3KeyFrameAnimation();
    animation.InsertKeyFrame(1f, new Vector3(200f, 0f, 0f));
    animation.Duration = TimeSpan.FromSeconds(2);
    animation.Direction = Windows.UI.Composition.AnimationDirection.Alternate;
    // Run animation for 10 times
    animation.IterationCount = 10;
    redVisual.StartAnimation("Offset", animation);
} 
```
