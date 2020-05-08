---
Description: 了解流畅运动如何使用方向性和重心。
title: 方向性和重力-Windows 应用中的动画
label: Directionality and gravity
template: detail.hbs
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: ddcfac5e36500a8fc6dc41c7c86037f5a1483203
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970642"
---
# <a name="directionality-and-gravity"></a>方向性和引力

方向信号有助于巩固用户在体验过程中建立的心理模型。 任何运动的方向都必须支持空间连续性和空间中对象的完整性。

定向运动受到力（例如引力）的影响。 对运动施加力可让运动看起来更加自然。

## <a name="examples"></a>示例

<table>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>如果已安装了 <strong style="font-weight: semi-bold">XAML 控件库</strong>应用，单击此处<a href="xamlcontrolsgallery:/category/Motion">打开此应用，查看“动作”的实际应用</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="direction-of-movement"></a>运动方向

:::row:::
    :::column:::
移动方向对应于物理运动。 就像在自然界中一样，对象可以沿任意世界轴（X、Y、Z）移动。 我们按照这种方式看待对象在屏幕上的运动。
移动对象时，请避免非自然冲突。 请记住，对象来自和转到，并始终支持可以在场景中使用的更高级别构造，如滚动方向或布局层次结构。
    :::column-end:::
    :::column:::
        ![方向后退进入](images/Direction.gif)
    :::column-end:::
:::row-end:::

## <a name="direction-of-navigation"></a>导航方向

应用中场景之间的导航方向是概念性的。 用户前进和后退导航。 场景移入和移出视图。 这些概念结合物理运动为用户提供指引。

当导航导致对象从前一个场景移动到新场景时，该对象在屏幕上进行简单的 A 到 B 移动。 为了让运动效果更加逼真，增加了标准缓动和引力感。

对于后退导航，移动是反向的（从 B 到 A）。 当用户导航回来时，他们希望尽快返回到之前的状态。 计时更快、更直接，并使用减速缓动。

下面的示例应用了上述原则 - 在前进和后退导航过程中，所选项目停留在屏幕上。

![连续动作的 UI 示例](images/continuous3.gif)

当导航导致屏幕上的项目被替换时，显示退出场景去往何方、新场景来自何处至关重要。

这样做有以下几个好处：

- 巩固用户的空间心理模型。
- 退出场景的持续时间提供更多时间来准备内容，以便为进入场景应用动画效果。
- 改善了应用的感知性能。

需要仔细考虑 4 个导航方向。

:::row:::
    :::column:::
**前进**以不与传出内容发生冲突的方式，庆祝内容进入场景。 内容减速到场景中。
    :::column-end:::
    :::column:::
        ![前进方向](images/forwardIN.gif)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
**向外扩展**内容很快就会退出。 对象在屏幕上进行加速。
    :::column-end:::
    :::column:::
        ![前进方向](images/forwardOUT.gif)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
**向后**与 Forward 相同，但反向。
    :::column-end:::
    :::column:::
        ![方向后退进入](images/backwardIN.gif)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
**向后输出**与前向外，但反过来。
    :::column-end:::
    :::column:::
        ![向后翻方向](images/backwardOUT.gif)
    :::column-end:::
:::row-end:::

## <a name="gravity"></a>引力

引力可让体验更加自然。 沿 Z 轴移动并且没有通过屏幕提供 (affordance) 锚定到场景的对象可能会受到引力的影响。 当对象摆脱场景约束但没有达到逃逸速度之前，引力会向下拉扯对象，使对象在移动时产生更自然的曲线轨迹。

引力效果通常在对象必须从一个场景跳到另一个场景时显现。 因此，连接的动画使用了引力的概念。

在下面的示例中，网格顶行中的一个元素受到引力的影响，致使其在离开原来位置时略微下降，然后才移动到前方。

![方向后退进入](images/continuity-photos.gif)

## <a name="related-articles"></a>相关文章

- [运动概述](index.md)
- [计时和缓动](timing-and-easing.md)
