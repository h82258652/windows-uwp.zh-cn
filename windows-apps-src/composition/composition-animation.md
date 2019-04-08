---
ms.assetid: 386faf59-8f22-2e7c-abc9-d04216e78894
title: 合成动画
description: 许多合成对象和效果属性均可使用关键帧和表达式动画设置动画，以便 UI 元素的属性可以随时间变化或基于计算发生变化。
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b94f14b32c5dd74e0aefb9b9a99f64bbd905a05d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57616702"
---
# <a name="composition-animations"></a>合成动画

Windows.UI.Composition API 允许你在统一的 API 层中创建、设置动画、转换和操作合成器对象。 合成动画提供一种在应用程序 UI 中运行动画的强大且有效的方法。 它们从一开始就设计为确保你的动画可以独立于 UI 线程并以 60 FPS 的频率运行，并让你不仅可以使用时间，而且还可以使用输入和其他属性来灵活生成令人惊叹的体验以驱动动画。

## <a name="motion-in-windows"></a>在 Windows 中的动作

想一想电影中的运动设计。 运动的无缝过渡让你能够专注于故事，为你带来真实体验。 我们可以将这种感觉融入我们的设计，引导人们在观影过程中能够轻松从一个任务跳转到另一个任务。 动作通常是用户界面和用户体验之间的区别因素。

为 Windows UI 平台的基本构建基块，CompositionAnimations 提供一种强大有效的方法在应用程序的 UI 中创建移动体验。 动画引擎旨在从一开始会确保你动作在 60 FPS，独立于 UI 线程时运行。 这些动画设计提供灵活性以构建创新的运动体验基于时间、 输入和其他属性。

### <a name="examples-of-motion"></a>动作示例

下面是应用中的一些动作示例。

在此，应用使用连贯动画来为一个正在“继续”变成下一页标题中一部分的项目图片创建动画。 该效果有助于在转换过程维持用户上下文。

![举例说明如何连接动画](images/animation/connected-animation-example.gif)

在此，当 UI 滚动或平移时，视差视觉效果将以不同的速率移动不同的对象，打造一种深度、透视和移动感。

![一个列表和背景图像的视差示例](images/animation/parallax-example.gif)

## <a name="using-compositionanimations-to-create-motion"></a>使用 CompositionAnimations 创建运动

若要在 UI 中生成运动，开发人员可以访问 XAML （链接到情节提要此处） 或可视化层中的动画。 在可视化层的动画提供了开发人员提供一系列的好处：

- 性能-而不是传统的 UI 线程绑定动画，在 Windows UI 平台上的动画在 60 FPS，实现平滑运动体验在独立线程上运行。
- 模板化模型 – Windows UI 层中的动画是模板、 含义可以在多个对象上使用单个动画和调整属性或参数而无需担心不阻止以前的使用。
- 自定义 – Windows UI 层不仅可以轻松地进行美观的 UI，但与一系列完整的动画类型，可以通过创建新的和令人惊叹的体验是渐变的自定义项

作为开发人员创建在 Windows UI 层上的体验，你有权访问各种动画概念将您的设计转换为现实。 可以使用任何这些概念来对属性进行动画处理或 subchannel 任何 CompositionObject 组件 （如果适用）。

> [!NOTE]
> 并非所有属性 CompositionObject 都是可进行动画处理。 请参阅各个 CompositionObject 来确定属性是否可进行动画处理的文档。

> [!NOTE]
> 术语_subchannel_指组件窗体的属性。 例如，X 或 XY subchannel 的 Vector3 偏移量属性。

| 动画概念 | 描述 |
| ----------------- | ----------- |
| [基于时间的动作与 KeyFrameAnimations](time-animations.md)  | KeyFrameAnimations 用于直接控制对整个一段时间内的动作体验。 开发人员介绍 motion 的开始、 结束、 在之间的内插和持续时间以传统关键的方式。 |
| [使用 ExpressionAnimations 相对运动](relation-animations.md)  | ExpressionAnimations 用于描述应如何相对于另一个对象的属性驱动一个对象的属性之间的运动。 开发人员定义数学等式，用于定义基于引用的关系。 |
| ImplicitAnimations | 这些动画是基于触发器的并且单独定义和核心应用程序逻辑。 ImplicitAnimations 用于描述动画如何以及何时是作为对直接属性更改的响应。 |
| [输入驱动输入动画的动作](input-driven-animations.md)  | 输入的动画介绍了一组方案，使开发人员来描述通过触摸或其他输入的形式基于操作的动作。 在活动的用户输入或手势，是由于这些动画。 |
| [使用 NaturalMotionAnimations 基于物理运动](natural-animations.md)  | NaturalMotionAnimations 用于描述简单自然且熟悉运动体验基于实际强制驱动的运动。 而不是定义时，开发人员定义的特性的动作 （例如为弹簧阻止比率） |