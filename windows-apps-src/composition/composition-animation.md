---
author: jwmsft
ms.assetid: 386faf59-8f22-2e7c-abc9-d04216e78894
title: 合成动画
description: 许多合成对象和效果属性均可使用关键帧和表达式动画设置动画，以便 UI 元素的属性可以随时间变化或基于计算发生变化。
ms.author: jimwalk
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 38f9d0daf230007d1d32a7d2187d54baa90986e5
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/02/2018
ms.locfileid: "5988082"
---
# <a name="composition-animations"></a>合成动画

Windows.UI.Composition API 允许你在统一的 API 层中创建、设置动画、转换和操作合成器对象。 合成动画提供一种在应用程序 UI 中运行动画的强大且有效的方法。 它们从一开始就设计为确保你的动画可以独立于 UI 线程并以 60 FPS 的频率运行，并让你不仅可以使用时间，而且还可以使用输入和其他属性来灵活生成令人惊叹的体验以驱动动画。

## <a name="motion-in-windows"></a>Windows 中的动态

想一想电影中的运动设计。 运动的无缝过渡让你能够专注于故事，为你带来真实体验。 我们可以邀请该感觉融入设计，引导人们从一个任务到下一个轻松。 动作通常是用户界面和用户体验之间的区别性因子。

为 Windows UI 平台的基本构建基块，Compositionanimation 提供一种强大且有效的方法来创建你的应用程序 UI 中的运动体验。 动画引擎已设计具有从头设置以确保你的运动独立于 UI 线程的 60 FPS 的频率运行。 这些动画旨在提供灵活生成创新的运动体验，具体取决于时间、 输入和其他属性。

### <a name="examples-of-motion"></a>动作示例

下面是应用中的一些动作示例。

在此，应用使用连贯动画来为一个正在“继续”变成下一页标题中一部分的项目图片创建动画。 该效果有助于在转换过程维持用户上下文。

![连接动画示例](images/animation/connected-animation-example.gif)

在此，当 UI 滚动或平移时，视差视觉效果将以不同的速率移动不同的对象，打造一种深度、透视和移动感。

![附带列表和背景图像的一个视差示例](images/animation/parallax-example.gif)

## <a name="using-compositionanimations-to-create-motion"></a>使用 Compositionanimation 以创建运动

若要在 UI 中生成运动，开发人员可以访问 XAML （链接到情节提要下面） 或可视化层中的动画。 在可视化层的动画为开发人员提供一系列的好处：

- 性能 – 而不是传统 UI 线程绑定动画，在 Windows UI 的平台上的动画针对独立线程 60 FPS，启用平滑的运动体验时进行操作。
- 模板化模型 – Windows UI 层中的动画都是模板，意味着可以使用多个对象上的单个动画，并调整属性或参数，而无需担心阻碍以前使用。
- 自定义 – Windows UI 层不仅可以轻松进行美观的 UI，但是与动画类型，一个完整范围，可以创建新的和令人惊叹的体验是自定义的渐变

作为开发人员在 Windows UI 层中创建体验，你有权访问大量的动画概念来使你的设计变的栩栩如生。 你可以使用任何这些概念来创建属性的动画或 subchannel 的任何 CompositionObject 的组件 （如果适用）。

> [!NOTE]
> 并非所有的 CompositionObject 的属性是可进行动画处理。 请参阅来确定是否可进行动画处理的属性的单个 CompositionObject 的文档。

> [!NOTE]
> 术语_通道_是指组件形式的属性。 例如，X 或 XY subchannel 的 Vector3 Offset 属性。

| 动画概念 | 说明 |
| ----------------- | ----------- |
| [使用 KeyFrameAnimations 基于时间的动画](time-animations.md)  | KeyFrameAnimations 用于直接控制的时间段内的运动体验全部。 开发人员描述运动的开始菜单、 结束、 在之间的内插和持续时间以一种传统关键的方式。 |
| [在使用 Expressionanimation 相对运动](relation-animations.md)  | Expressionanimation 用于描述一个对象的属性的运动驱动相对于另一个对象的属性的方式。 开发人员定义定义引用基于关系的数学等式。 |
| ImplicitAnimations | 这些动画是基于触发器和定义独立于核心应用逻辑。 ImplicitAnimations 用于描述如何以及何时动画是作为响应直接属性更改。 |
| [输入驱动的运动与输入动画](input-driven-animations.md)  | 输入的动画介绍了一组使开发人员可以描述通过触摸或其他输入的形式操作基于运动的方案。 在活动用户输入或手势，都驱动这些动画。 |
| [Naturalmotionanimation 与基于物理学的运动](natural-animations.md)  | Naturalmotionanimation 用于描述的自然和熟悉的运动体验，具体取决于真实强制驱动的运动。 而不是定义时，开发人员定义特征 (例如为弹簧 damping ratio) 的运动 |