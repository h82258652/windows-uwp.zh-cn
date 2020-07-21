---
ms.assetid: 386faf59-8f22-2e7c-abc9-d04216e78894
title: 合成动画
description: 许多合成对象和效果属性均可使用关键帧和表达式动画设置动画，以便 UI 元素的属性可以随时间变化或基于计算发生变化。
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5991b5f9f3c63a25a7476cc35621488c6cb60b2c
ms.sourcegitcommit: 445320ff0ee7323d823194d4ec9cfa6e710ed85d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/11/2019
ms.locfileid: "72281831"
---
# <a name="composition-animations"></a>合成动画

Windows.UI.Composition API 允许你在统一的 API 层中创建、设置动画、转换和操作合成器对象。 合成动画提供一种在应用程序 UI 中运行动画的强大且有效的方法。 它们从一开始就设计为确保你的动画可以独立于 UI 线程并以 60 FPS 的频率运行，并让你不仅可以使用时间，而且还可以使用输入和其他属性来灵活生成令人惊叹的体验以驱动动画。

## <a name="motion-in-windows"></a>Windows 中的动作

想一想电影中的运动设计。 运动的无缝过渡让你能够专注于故事，为你带来真实体验。 我们可以将这种感觉融入我们的设计，引导人们在观影过程中能够轻松从一个任务跳转到另一个任务。 运动通常是用户界面和用户体验之间的与众不同因素。

作为 Windows UI 平台的基本构建基块，CompositionAnimations 提供了一种强大而高效的方法，可在应用程序的 UI 中创建运动体验。 动画引擎已从头开始设计，以确保动作以 60 FPS 的速度运行，独立于 UI 线程。 这些动画旨在提供基于时间、输入和其他属性构建创新运动体验的灵活性。

### <a name="examples-of-motion"></a>动作示例

下面是应用中的一些动作示例。

在此，应用使用连贯动画来为一个正在“继续”变成下一页标题中一部分的项目图片创建动画。 该效果有助于在转换过程维持用户上下文。

![连接动画示例](images/animation/connected-animation-example.gif)

在此，当 UI 滚动或平移时，视差视觉效果将以不同的速率移动不同的对象，打造一种深度、透视和移动感。

![一个列表和背景图像的视差示例](images/animation/parallax-example.gif)

## <a name="using-compositionanimations-to-create-motion"></a>使用 CompositionAnimations 创建运动

若要在 UI 中生成动作，开发人员可以在 XAML 或可视层中访问动画。 可视化层上的动画为开发人员提供了一系列优势：

- 性能-而不是传统的 UI 线程绑定动画，Windows UI 平台上的动画在 60 FPS 的独立线程上运行，从而实现平滑运动体验。
- 模板化模型– Windows UI 层中的动画都是模板，这意味着可以在多个对象上使用单个动画并调整属性或参数，而无需担心上一次使用。
- 自定义– Windows UI 层不仅可以轻松制作精美的 UI，而且还可以使用各种动画类型来创建具有自定义渐变的全新体验

作为开发人员在 Windows UI 层创建体验的开发人员，你可以访问各种动画概念，使你的设计成为现实。 可以使用以下任何概念对任何 CompositionObject 的属性或 subchannel 组件（如果适用）进行动画处理。

> [!NOTE]
> 并非 CompositionObject 的所有属性都是动画处理。 请参阅各个 CompositionObject 的文档，以确定属性是否为动画处理。

> [!NOTE]
> 字词_subchannel_引用属性的组件形式。 例如，System.numerics.vector2 偏移量属性的 X 或 XY subchannel。

| 动画概念 | 描述 |
| ----------------- | ----------- |
| [KeyFrameAnimations 的基于时间的运动](time-animations.md)  | KeyFrameAnimations 用于直接控制一段时间内的全部运动体验。 以传统 keyframed 方式描述运动的开始、结束、内插和持续时间的开发人员。 |
| [ExpressionAnimations 的相对运动](relation-animations.md)  | ExpressionAnimations 用于描述如何将一个对象的属性的运动相对于另一个对象的属性。 开发人员定义定义基于引用的关系的数学公式。 |
| ImplicitAnimations | 这些动画基于触发器，并独立于核心应用逻辑定义。 ImplicitAnimations 用于说明动画发生的方式和时间，作为对直接属性更改的响应。 |
| [输入动画的输入驱动动作](input-driven-animations.md)  | 输入动画包含一组方案，使开发人员能够通过触摸或其他输入情态描述基于操作的动作。 这些动画基于活动用户输入或手势驱动。 |
| [通过 NaturalMotionAnimations 的基于物理学的运动](natural-animations.md)  | NaturalMotionAnimations 用于基于实际的强制驱动运动描述自然和熟悉的运动体验。 开发人员不定义时间，而是定义运动的特征（例如弹簧的阻尼比率） |