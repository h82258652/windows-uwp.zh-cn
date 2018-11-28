---
title: 输入驱动的动画
description: 了解如何在应用 UI 中使用输入动画。
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, 动画
ms.localizationpriority: medium
ms.openlocfilehash: 94d15fc7f2443475020aa7e134c076b833db46a8
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/27/2018
ms.locfileid: "7837927"
---
# <a name="input-driven-animations"></a>输入驱动的动画

本文介绍了 InputAnimation API，并针对如何在 UI 中使用这些类型的动画提供建议。

## <a name="prerequisites"></a>先决条件

我们在此假设你熟悉这些文章中所述的概念：

- [基于关系的动画](relation-animations.md)

## <a name="smooth-motion-driven-from-user-interactions"></a>用户交互驱动的平滑运动

在 Fluent 设计语言中，最终用户与应用之间的交互最为重要。 应用不仅需注意这个方面，还要对与之交互的用户做出自然而动态的响应。 这意味着当用户把手指放在屏幕上时，面对不同的输入角度，UI 都能完美响应；滚动应感觉流畅，并紧随手指在屏幕上的移动而移动。

构建能够动态流畅地响应用户输入的 UI 将会提高用户的参与度 - 现在，当用户与不同的 UI 体验交互时，运动不仅看起来效果不错，感觉也很自然流畅。 这让最终用户能够更轻松地与应用程序连接，让使用体验更让人难忘和愉快。

## <a name="expanding-past-just-touch"></a>只需触摸便可扩展

尽管现在触摸板是最终用户最常用来操作 UI 内容的接口之一，他们还是会使用其他不同的输入形式，如鼠标和触控笔。 在这些情况下，最终用户必须要能够感觉到 UI 动态响应其输入，无论用户选择何种输入形式。 在设计输入驱动的运动体验时，你应该已了解不同的输入形式。

## <a name="different-input-driven-motion-experiences"></a>不同输入驱动的运动体验

InputAnimation 空间提供几种不同的创建动态响应运动的体验。 就像 Windows UI 动画系统的其余部分一样，这些输入驱动的动画针对独立线程进行操作，这有助于改进动态运动体验。 但是，在某些情况下，这种体验利用了现有的 XAML 控件和组件，而其中的部分体验仍然受 UI 线程限制。

构建输入驱动的动态运动时，会创建三种核心体验：

1. 增强的现有 ScrollView 体验 - 支持 XAML ScrollViewer 位置驱动动态动画体验。
1. 指针位置驱动的体验 - 利用位于命中测试的 UIElement 上的光标位置来驱动动态的动画体验。
1. InteractionTracker 的自定义操作体验 – 使用 InteractionTracker 打造完全自定义的、线程外操作体验（如滚动/缩放画布）。

## <a name="enhancing-existing-scrollviewer-experiences"></a>增强现有的 ScrollViewer 体验

创建更多动态体验的一种常见方法是基于现有 XAML ScrollViewer 控件构建。 在这些情况下，可以充分利用 ScrollViewer 的滚动位置来创建额外的 UI 组件，使简单的滚动体验变得更加动态。 例如粘滞/害羞标头和视差。

![含视差效果的列表视图](images/animation/parallax.gif)

![害羞标头](images/animation/shy-header.gif)

在创建这些类型的体验时，有一个需要遵循的常规准则：

1. 访问需要驱动动画的 XAML ScrollViewer 的 ScrollManipulationPropertySet。
    - 通过 ElementCompositionPreview.GetScrollViewerManipulationPropertySet(UIElement element) API 来完成
    - 返回包含名为 **Translation** 的属性的 CompositionPropertySet
1. 使用引用 Translation 属性的等式来创建 Composition ExpressionAnimation。
1. 针对 CompositionObject 属性启动动画。

有关生成这些体验的详细信息，请参阅[增强现有 ScrollViewer 体验](scroll-input-animations.md)。

## <a name="pointer-position-driven-experiences"></a>指针位置驱动的体验

另一种与输入有关的常见动态体验是基于指针（如鼠标光标）位置的动画。 在这些情况下，开发人员就能够在 XAML UIElement 中的命中测试使得创建 Spotlight Reveal 等体验成为可能时，利用光标位置。

![指针聚焦示例](images/animation/spotlight-reveal.gif)

![指针旋转示例](images/animation/pointer-rotate.gif)

在创建这些类型的体验时，有一个需要遵循的常规准则：

1. 访问进行命中测试时需要知道光标位置的 XAML UIElement 的 PointerPositionPropertySet。
    - 通过 ElementCompositionPreview.GetPointerPositionPropertySet(UIElement element) API 来完成
    - 返回包含名为 **Position** 的属性的 CompositionPropertySet
1. 使用引用 Position 属性的等式来创建 CompositionExpressionAnimation。
1. 针对 CompositionObject 的属性启动动画。

## <a name="custom-manipulation-experiences-with-interactiontracker"></a>InteractionTracker 的自定义操作体验

利用 XAML ScrollViewer 的挑战之一是它受 UI 线程限制。 因此，如果 UI 线程变得繁忙并导致令人不太愉快的体验，滚动和缩放体验经常会迟滞并抖动。 此外，无法自定义 ScrollViewer 体验的很多方面。 InteractionTracker 旨在解决这两个问题，具体来说，它提供一组构建基块来创建在独立线程上运行的自定义操作体验。

![3D 交互示例](images/animation/interactions-3d.gif)

![拉动操作动画示例](images/animation/pull-to-animate.gif)

在使用 InteractionTracker 创建这些类型的体验时，有一个需要遵循的常规准则：

1. 创建 InteractionTracker 对象并定义其属性。
1. 对于应捕获输入以供 InteractionTracker 使用的任何 CompositionVisual 创建 VisualInteractionSource。
1. 使用引用 InteractionTracker 的 Position 属性的等式来创建 Composition ExpressionAnimation。
1. 对你希望通过 InteractionTracker 驱动的 Composition Visual 属性启动动画。
