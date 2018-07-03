---
author: jwmsft
Description: Learn how Fluent motion fundamentals come together in your app.
title: 运动练习 - UWP 应用中的动画
label: Motion in practice
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 87a17d3f73887c9b1b5029e2096c5b41c9444c4e
ms.sourcegitcommit: 517c83baffd344d4c705bc644d7c6d2b1a4c7e1a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/07/2018
ms.locfileid: "1843735"
---
# <a name="bringing-it-together"></a>综合运用

> [!IMPORTANT]
> 本文介绍的功能尚未发布，在商业发行之前可能发生实质性修改。 Microsoft 对于此处提供的信息不作任何明示或默示的担保。

计时、缓动、方向性和引力协同工作，构成了 Fluent 运动的基础。 必须对上述要素进行综合考虑，然后在应用上下文中适当应用。

以下是在应用中应用 Fluent 运动基础的 3 种方法。

:::row::: :::column::: **隐式动画**

        Automatic tween and timing between values in a parameter change to achieve very simple Fluent motion using the standardized values.
    :::column-end:::
    :::column:::
        **Built-in animation**

        System components, such as common controls and shared motion, are "Fluent by default". Fundamentals have been applied in a manner consistent with their implied usage.
    :::column-end:::
    :::column:::
        **Custom animation following guidance recommendations**

        There may be times when the system does not yet provide an exact motion solution for your scenario. In those cases, use the baseline fundamental recommendations as a starting point for your experiences.
    :::column-end:::
:::row-end:::

### **<a name="transition-example"></a>切换示例**

![功能性动画](images/pageRefresh.gif)

:::row::: :::column::: <b>方向前进退出：</b><br>
        淡出：150 m；缓动：默认加速

        <b>Direction Forward In:</b><br>
        Slide up 150px: 300ms; Easing: Default Decelerate
    :::column-end:::
    :::column:::
         <b>Direction Backward Out:</b><br>
        Slide down 150px: 150ms; Easing: Default Accelerate
      
        <b>Direction Backward In:</b><br>
        Fade in: 300ms; Easing: Default Decelerate
    :::column-end:::
:::row-end:::

 ### **<a name="object-example"></a>对象示例**

 ![300 ms 运动](images/control.gif)
 
:::row::: :::column::: <b>方向展开：</b><br>
        放大：300 ms；缓动：标准 :::column-end::: :::column::: <b>方向收缩：</b><br>
        放大：150 ms；缓动：默认加速 :::column-end::: :::row-end:::

## <a name="related-articles"></a>相关文章

- [运动概述](index.md)
- [计时和缓动](timing-and-easing.md)
- [方向性和引力](directionality-and-gravity.md)