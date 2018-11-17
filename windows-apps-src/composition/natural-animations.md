---
author: jwmsft
title: 自然运动动画
description: 了解自然运动动画以及如何在应用 UI 中使用它们。
ms.author: jimwalk
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, 动画
ms.localizationpriority: medium
ms.openlocfilehash: 537e722917f00d590428dd2b5ee2d24e023e52b6
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/16/2018
ms.locfileid: "7164224"
---
# <a name="natural-motion-animations"></a>自然运动动画

本文简要概述了 NaturalMotionAnimation 空间，并介绍了如何从概念上考虑在 UI 中使用这些类型的动画。

## <a name="making-motion-feel-familiar-and-natural"></a>使运动让人感觉熟悉而自然

出色的应用都能吸引并留住用户注意力，并能帮助用户完成任务。 运动是能够将用户界面与用户体验区分开来的重要因素 - 它在用户与他们与之交互的应用程序之间引发联系。 这种联系越紧密，最终用户的参与度和满意度都越高。

运动可帮助建立这种联系的一种方式是提供让用户从外观上感到熟悉的体验。 用户对于其在运动中感觉到的体验有一种无意识的预期，他们希望这种体验基于其实际的生活经验。 我们看到物体在地板上如何滑动、如何从桌面上跌落、如何相互碰撞，以及如何在弹簧的作用下振荡。 基于真实世界物理特性来利用这种预期的运动，在我们的眼中会显得更加自然。 运动变得更加自然，与用户的交互也更多，但更重要的是，整个体验都变得更加让人难忘和愉快。

![无动画效果的缩放运动](images/animation/scale-no-animation.gif)
![带贝塞尔曲线的缩放运动](images/animation/scale-cubic-bezier.gif)
![带弹簧动画的缩放运动](images/animation/scale-spring.gif)

最终结果是应用获得了更高的用户参与度和用户保留率。

## <a name="balancing-control-and-dynamism"></a>平衡控件和动态性

在传统 UI 中，[KeyFrameAnimation](https://docs.microsoft.com/uwp/api/windows.ui.composition.keyframeanimation) 是描述运动的主要方式。 关键帧为设计人员和开发人员提供用于定义开始、结束和内插的最多控件。 虽然这在许多情况下非常有用，但关键帧动画动态性不够；其运动没有适应性，在任何情况下都具有相同的外观。

在这个“频谱”的另一端，游戏和物理引擎中经常会看到模拟。 这些体验常常栩栩如生，并用户能够与之动态交互 - 这就能提供一种用户每天都能体会到的氛围和随意之感。 虽然这使得运动感觉更为鲜活和动态，设计人员和开发人员的控制力却较少，因此集成到传统 UI 更为困难。

![控制力频谱图](images/animation/natural-motion-diagram.png)

[NaturalMotionAnimation](https://docs.microsoft.com/uwp/api/windows.ui.composition.naturalmotionanimation) 旨在帮助衔接起这种分离的状况 – 实现控制力与运动之间的平衡，对于重要的动画元素（如开始/完成）具有控制力，又能保持运动的自然和动态。

> [!NOTE]
> NaturalMotionAnimation 并不意在替代关键帧动画 - 在 Fluent 设计语言中仍然存在建议使用关键帧之处。 NaturalMotionAnimation 意在用于需要运动，但是关键帧动画的动态性不够的地方。

## <a name="using-naturalmotionanimations"></a>使用 NaturalMotionAnimations

从 Fall Creators Update 开始，可以获得一种运动新体验：**弹簧动画**。 请参阅[弹簧动画](spring-animations.md)深入了解弹簧的操作实例。

此运动类型通过使用新 NaturalMotionAnimation 来实现 – 这是一个新的动画类型，其中心是支持开发人员在其 UI 中构造更令人熟悉和自然的运动，同时在控制力与动态性之间取得平衡。 它们公开以下功能：

- 定义开始和结束值。
- 使用 InteractionTracker 定义 InitialVelocity 并绑定到输入。
- 定义特定于运动的属性（如弹簧的 DampingRatio。）

一般的入门准则：

1. 使用一个 **Create** 方法基于 Compositor 创建 NaturalMotionAnimation。
1. 定义动画的属性。
1. 将 NaturalMotionAnimation 作为参数传递到 CompositionObject 的 StartAnimation 调用。
    - 或者设置为 InteractionTracker InertiaModifier 的 Motion 属性。

使用弹簧 NaturalMotionAnimation 为新 X Offset 位置创建可视“弹簧”的基本示例：

```csharp
_springAnimation = _compositor.CreateSpringScalarAnimation();
_springAnimation.Period = TimeSpan.FromSeconds(0.07);
_springAnimation.DelayTime = TimeSpan.FromSeconds(1);
_springAnimation.EndPoint = 500f;
sectionNav.StartAnimation("Offset.X", _springAnimation);
```
