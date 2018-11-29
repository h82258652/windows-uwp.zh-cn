---
Description: Purposeful, well-designed motion brings your app to life and makes the experience feel crafted and polished. Help users understand context changes, and tie experiences together with visual transitions.
title: UWP 应用中的动作和动画
ms.assetid: 21AA1335-765E-433A-85D8-560B340AE966
label: Motion
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 2701844ccefdc5a535fa8fc20086c550cb7bc29e
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/29/2018
ms.locfileid: "8189282"
---
# <a name="motion-for-uwp-apps"></a>适用于 UWP 应用的动作

![主图](images/header-motion2.svg)

Fluent 运动在应用中提供用途。 它基于用户行为提供智能反馈、让 UI 感觉保持生动，并指导用户在你的应用中导航。 Fluent 运动在用户和其数字体验之间引起情感连接。 我们建立了用户已从物理世界中了解的自然运动的基础，并在这个基础上扩展我们的系统。

## <a name="fluent-motion-principles"></a>Fluent 运动原则

### <a name="physical"></a>物理

运动中的对象展现现实世界中的对象行为。 流畅、响应的运动打造自然的体验，从而建立起情感连接、增加个性化。

![UI 物理运动示例](images/Physical.gif)
> 在通过触控与 UI 交互时，UI 的移动与交互速度直接相关。 因为触控是直接操作，你与之交互的对象将影响其周围的对象。

### <a name="functional"></a>功能

动作提供用途并确认。 它指导用户了解复杂情况，并帮助建立层次结构。 移动为展示带来增强的效果，并通过隐藏感知延迟优化用户体验。

![UI 功能动作示例](images/functional.gif)
> 页面过渡是专门构建的。 它们提供有关页面如何彼此相关的提示。 它们以被视为快速的方式移动，即使在效果不佳的情况。

### <a name="continuous"></a>连续

点到点的流畅运动自然地绘制眼睛并指导用户操作。 它巧妙地将用户任务拼接起来，使其感觉更可用、更友好。

![UI 连续动作示例](images/continuous3.gif)
> 对象在场景与场景之间移动或在场景内变形以提供连续性，并帮助用户保持上下文。

### <a name="contextual"></a>上下文

智能动作以与用户操作 UI 相同的方式向用户提供反馈。 交互以用户为中心。 运动效果适合外形规格并围绕场景设计。 它应该让每个用户都感觉舒适。

![UI 上下文动作示例](images/Contextual.gif)
> 动画应绑定到用户交互。 上下文菜单从用户激活它的点部署。 

## <a name="motion-articles"></a>动作文章

:::row:::
    :::column:::
        ### [Timing and easing](timing-and-easing.md)
        Timing and easing are important elements that make motion feel natural for objects entering, exiting, or moving within the UI.
    :::column-end:::
    :::column:::
        ### [Directionality and gravity](directionality-and-gravity.md)
        Directional signals help provide a solid mental model of the journey a user takes across experiences. Directional movement is subject to forces like gravity, which reinforces the natural feel of the movement.
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        ### [Page transitions](page-transitions.md)
        Page transitions navigate users between pages in an app, providing feedback about the relationship between pages. They help users understand where they are in the navigation hierarchy.
    :::column-end:::
    :::column:::
        ### [Connected animation](connected-animation.md)
        Connected animations let you create a dynamic and compelling navigation experience by animating the transition of an element between two different views.
    :::column-end:::
:::row-end:::
