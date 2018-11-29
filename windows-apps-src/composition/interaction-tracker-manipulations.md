---
title: 使用 InteractionTracker 创建自定义操作
description: 使用 InteractionTracker API 来创建自定义操作体验。
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, 动画
ms.localizationpriority: medium
ms.openlocfilehash: 9d2c965bcfbf81efe73ce8aff93cdb8b31163fbd
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/29/2018
ms.locfileid: "8191213"
---
# <a name="custom-manipulation-experiences-with-interactiontracker"></a>InteractionTracker 的自定义操作体验

在本文中，我们将介绍如何使用 InteractionTracker 来创建自定义操作体验。

## <a name="prerequisites"></a>先决条件

我们在此假设你熟悉这些文章中所述的概念：

- [输入驱动的动画](input-driven-animations.md)
- [基于关系的动画](relation-animations.md)

## <a name="why-create-custom-manipulation-experiences"></a>为什么要创建自定义操作体验？

在大多数情况下，利用预先构建的操作控件就足以创建 UI 体验。 但是，如果需要与常见控件有所不同，该怎么办？ 如果需要创建输入驱动的特定体验，或在 UI 中光有传统操作运动还不够，该怎么办？ 这就是创建自定义体验可以发挥作用之处了。 应用开发人员和设计人员可以通过自定义体验变得更加有创造力 - 带来真实的运动体验，更好地展现他们独有的自定义设计语言。 在整个过程中，你都会获得全面自定义操作体验所需要的相应构建基块：从运动应该如何响应手指接触和移开屏幕，到吸附点和输入链。

下面是一些创建自定义操作体验的常见示例：

- 添加自定义的轻扫、删除/消除行为
- 输入驱动的效果（平移导致内容模糊）
- 具有定制的操作运动（自定义 ListView、ScrollViewer 等）的自定义控件

![轻扫滚动条示例](images/animation/swipe-scroller.gif)

![拉动操作动画示例](images/animation/pull-to-animate.gif)

## <a name="why-use-interactiontracker"></a>为何要使用 InteractionTracker？

10586 SDK 版本的 Windows.UI.Composition.Interactions 命名空间中引入了 InteractionTracker。 InteractionTracker 能够提供：

- **全面的灵活性** – 希望你能够从各个方面自定义和定制操作体验；具体来说，就是在输入期间以及在对输入的响应中发生的精确运动。 在使用 InteractionTracker 生成自定义操作体验时，所需的所有操作按键都可随意使用。
- **流畅的性能** – 操作体验的一项挑战是其性能要依赖于 UI 线程。 如果 UI 繁忙，这就可能对任何操作体验造成负面影响。 InteractionTracker 可以利用新的动画引擎，该引擎可以 60 FPS的帧速用独立线程运行，因而能提供平滑的运动。

## <a name="overview-interactiontracker"></a>概述：InteractionTracker

创建自定义操作体验时，要与两个主要组件进行交互。 我们先讨论第一个：

- [InteractionTracker](https://docs.microsoft.com/uwp/api/windows.ui.composition.interactions.interactiontracker) – 维护其属性由活动的用户输入或直接更新和动画驱动的状态机的核心对象。 然后，它应绑定到一个 CompositionAnimation 以创建自定义操作运动。
- [VisualInteractionSource](https://docs.microsoft.com/uwp/api/windows.ui.composition.interactions.visualinteractionsource) – 一个补充对象，定义何时以及在什么情况下将输入发送给 InteractionTracker。 它定义用于命中测试的 CompositionVisual 以及其他输入配置属性。

作为状态机中，InteractionTracker 的属性可由以下任何一项驱动：

- 直接用户交互 – 最终用户在 VisualInteractionSource 命中测试区域内直接操作
- 惯性 – 源自编程速度或用户手势，InteractionTracker 的属性在一条惯性曲线下展现动画效果
- CustomAnimation – 直接针对 InteractionTracker 的属性的自定义动画

### <a name="interactiontracker-state-machine"></a>InteractionTracker 状态机

如前面所述，InteractionTracker 是具有 4 种状态 – 其中每个都可过渡到任何其他 fourstates 的状态机。 (有关 InteractionTracker 如何在这些状态之间过渡的详细信息，请参阅 [InteractionTracker](https://docs.microsoft.com/uwp/api/windows.ui.composition.interactions.interactiontracker) 类文档。)

| 状态 | 描述 |
|-------|-------------|
| Idle | 不活动状态，驱动输入或动画 |
| Interacting | 检测到活动用户输入 |
| Inertia | 因活动输入或编程速度导致的活动运动 |
| CustomAnimation | 因自定义动画导致的活动运动 |

只要 InteractionTracker 状态发生更改，就会生成可以侦听的事件（或回调）。 要让这些事件可以侦听，它们必须实现 [IInteractionTrackerOwner](https://docs.microsoft.com/uwp/api/windows.ui.composition.interactions.iinteractiontrackerowner) 接口，并使用 CreateWithOwner 方法创建其 InteractionTracker 对象。 下图也简要介绍了不同事件在何时触发。

![InteractionTracker 状态机](images/animation/interaction-tracker-diagram.png)

## <a name="using-the-visualinteractionsource"></a>使用 VisualInteractionSource

要通过输入驱动 InteractionTracker，需要将 VisualInteractionSource (VIS) 与它连接。 使用 CompositionVisual 创建 VIS 作为补充对象，以定义：

1. 将在其中跟踪输入并检测坐标空间手势的命中测试区域
1. 将检测和路由的输入的配置，其中包括以下几种：
    - 可检测到的手势：位置 X 和 Y（水平和垂直平移）、伸缩（缩放）
    - 惯性
    - 轨道和链
    - 重定向模式：哪些输入数据将自动重定向到 InteractionTracker

> [!NOTE]
> 因为将基于命中测试位置和 Visual 坐标空间创建 VisualInteractionSource，建议不要使用 Visual，因为它可能处于运动状态或在变换位置。

> [!NOTE]
> 如果有多个命中测试区域，可以对多个 VisualInteractionSource 实例使用相同 InteractionTracker。 但是，最常见的情况是只使用一个 VIS。

VisualInteractionSource 也负责管理用不同形式（触控板、PTP、触控笔）输入的输入数据何时路由到 InteractionTracker。 此行为由 ManipulationRedirectionMode 属性定义。 默认情况下，所有指针输入都发送到 UI 线程，而精确式触摸板输入则传递到 VisualInteractionSource 和 InteractionTracker。

因此，如果需要用触摸板和触控笔（创作者更新）来通过 VisualInteractionSource 和 InteractionTracker 驱动操作，则必须调用 VisualInteractionSource.TryRedirectForManipulation 方法。 在下面这个来自一个 XAML 应用的简短代码段中，当最顶部的 UIElement Grid 发生按下触控板的事件时，调用了该方法：

```csharp
private void root_PointerPressed(object sender, PointerRoutedEventArgs e)
{
    if (e.Pointer.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Touch)
    {
        _source.TryRedirectForManipulation(e.GetCurrentPoint(root));
    }
}
```

## <a name="tie-in-with-expressionanimations"></a>与 ExpressionAnimation 搭配

利用 InteractionTracker 驱动操作体验时，主要与 Scale 和 Position 属性进行交互。 与其他 CompositionObject 属性一样，这些属性可以在 CompositionAnimation（最常见的是 ExpressionAnimation）中作为动画处理目标和进行引用。

要在 Expression 内使用 InteractionTracker，请如以下示例一样引用跟踪器的 Position（或 Scale）属性。 当 InteractionTracker 的属性由于上述任何情况而被修改时，Expression 输出会也更改。

```csharp
// With Strings
var opacityExp = _compositor.CreateExpressionAnimation("-tracker.Position");
opacityExp.SetReferenceParameter("tracker", _tracker);

// With ExpressionBuilder
var opacityExp = -_tracker.GetReference().Position;
```

> [!NOTE]
> 在 Expression 中引用 InteractionTracker 的 Position 时，必须将该值取反，生成的 Expression 才能转为正确的方向。 这是因为 InteractionTracker 的进度在图表中以到原点的距离表示，这样可以将 InteractionTracker 的进度视为“真实世界”坐标，如相距原点的距离。

## <a name="get-started"></a>即刻体验

开始使用 InteractionTracker 创建自定义操作体验：

1. 使用 InteractionTracker.Create 或 InteractionTracker.CreateWithOwner 创建 InteractionTracker 对象。
    - （如果使用 CreateWithOwner，请确保实现 IInteractionTrackerOwner 接口。）
1. 将新建的 InteractionTracker 设置为 Min 和 Max 位置。
1. 使用 CompositionVisual 创建 VisualInteractionSource。
    - 确保传入的 visual 具有非零值大小。 否则，无法对它正确进行命中测试。
1. 设置 VisualInteractionSource 的属性。
    - VisualInteractionSourceRedirectionMode
    - PositionXSourceMode、PositionYSourceMode、ScaleSourceMode
    - 轨道和链
1. 使用 InteractionTracker.InteractionSources.Add 将 VisualInteractionSource 添加到 InteractionTracker。
1. 为检测到触控板和触控笔输入的情况设置 TryRedirectForManipulation。
    - 对于 XAML，这通常在发生 UIElement PointerPressed 事件时执行。
1. 使用引用 InteractionTracker 的 Position 属性的 ExpressionAnimation，并将 CompositionObject 的属性作为目标。

以下代码段说明了以上 1-5 的操作：

```csharp
private void InteractionTrackerSetup(Compositor compositor, Visual hitTestRoot)
{ 
    // #1 Create InteractionTracker object
    var tracker = InteractionTracker.Create(compositor);

    // #2 Set Min and Max positions
    tracker.MinPosition = new Vector3(-1000f);
    tracker.MaxPosition = new Vector3(1000f);

    // #3 Setup the VisualInteractionSourc
    var source = VisualInteractionSource.Create(hitTestRoot);

    // #4 Set the properties for the VisualInteractionSource
    source.ManipulationRedirectionMode =
        VisualInteractionSourceRedirectionMode.CapableTouchpadOnly;
    source.PositionXSourceMode = InteractionSourceMode.EnabledWithInertia;
    source.PositionYSourceMode = InteractionSourceMode.EnabledWithInertia;

    // #5 Add the VisualInteractionSource to InteractionTracker
    tracker.InteractionSources.Add(source);
}
```

有关 InteractionTracker 的更多高级用法，请参阅下面的文章：

- [使用 InertiaModifier 创建吸附点](inertia-modifiers.md)
- [使用 SourceModifier 的下拉刷新](source-modifiers.md)
