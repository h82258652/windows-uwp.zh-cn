---
author: mijacobs
Description: "精心设计的有针对性的动作可以使应用变得栩栩如生，并且使体验感觉精良和完美。 帮助用户理解上下文更改，将体验与视觉转换紧密相连。"
title: "UWP 应用中的动作和动画"
ms.assetid: 21AA1335-765E-433A-85D8-560B340AE966
label: Motion
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Published
ms.openlocfilehash: d24edf5eaacb65ca28f2f2727f441cb833141dcf
ms.sourcegitcommit: 10d6736a0827fe813c3c6e8d26d67b20ff110f6c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/22/2017
---
# <a name="motion-for-uwp-apps"></a>适用于 UWP 应用的动作

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

精心设计的有针对性的动作可以使应用变得栩栩如生，并且使体验感觉精良和完美。 动作有助于用户了解上下文变更，及其在应用导航层次结构中所处的位置。 它将体验与视觉转换联系在一起。 动作可为体验添加一种节奏和维度感。

## <a name="benefits-of-motion"></a>动作的优势

动作不仅仅在于让事物移动。 动作是一种用于创建物理生态系统的工具，用户可在该系统内驻留并通过鼠标、键盘、触控和手写笔等各式各样的输入类型进行操作。 体验的质量取决于应用对用户的响应程度以及 UI 传达了怎样的个性。

请确保动作在应用中具有特定用途。 最佳通用 Windows 平台 (UWP) 应用使用动作使 UI 变的栩栩如生。 动作应该：

- 根据用户行为提供反馈。
- 指导用户如何与 UI 交互。
- 指示如何导航到之前或随后的视图。

随着用户在应用中所用的时间越来越多，或者随着应用中的任务变得更复杂，优质的动作变得越来越重要：它可用于改变用户感受认知负荷和你的应用的易用程度的方式。 动作还有许多其他直接优势：

- **动作支持交互和路径寻找。**

    动作具有方向性：它会向前和向后移动、移入和移出内容，并留下关于用户如何到达当前视图的心理“痕迹导航”线索。 转换可通过模拟用户熟悉的任务来帮助用户了解如何操作新的应用程序。

- **动作可以提供性能增强的印象。**

    当网络速度滞后或系统暂停工作时，动画可使用户感觉没有等待太长的时间。 动画可用于让用户了解应用正在处理任务、没有冻结，并且它可以被动地显示用户可能感兴趣的新信息。

- **动作可增加个性。**

    动作通常是用户经历一项体验时用于传达应用个性的一个常用线程。

- **动作可增添优雅魅力。**

    流畅、响应，动作打造自然的体验，从而建立起与体验之间的情感联结。

## <a name="examples-of-motion"></a>动作示例

下面是应用中的一些动作示例。

在此，应用使用连贯动画来为一个正在“继续”变成下一页标题中一部分的项目图片创建动画。 该效果有助于在转换过程维持用户上下文。

![连贯动画](images/connected-animations/example.gif)

在此，当 UI 滚动或平移时，视差视觉效果将以不同的速率移动不同的对象，打造一种深度、透视和移动感。

![附带列表和背景图像的一个视差示例](images/_Parallax_v2.gif)


## <a name="types-of-motion"></a>动作类型

<table>
    <tr>
        <th align="left">动作类型</th>
        <th align="left">描述</th>
    </tr>
    <tr>
        <td>[添加和删除](motion-list.md)
        </td>
        <td>列表动画可使你向集合（如相册或搜索结果列表）中插入或从中删除单个或多个项。
        </td>
    </tr>
    <tr>
        <td>[连贯动画](connected-animation.md)
        </td>
        <td>连贯动画让你可以通过为一个元素在两种不同视图之间的转换创建动画来创建动态和引入注目的导航体验。 这有助于用户维持其上下文并提供不同视图之间的连贯性。 在连贯动画中，当 UI 内容发生变化时，元素似乎在两种不同视图之间保持“连贯性”，从其在源视图中的位置掠过屏幕，到达其在新视图中的目标位置。 这强调了不同视图之间的共同内容，并创建了转换过程中美观且动态的效果。 
        </td>
    </tr>
    <tr>
        <td>[内容转换](content-transition-animations.md)
        </td>
        <td>内容转换动画可让你更改屏幕区域的内容，同时保持容器或背景不变。 新的内容将淡入。 如果存在要替换的现有内容，则该内容将淡出。
        </td>
    </tr>
    <tr>
        <td>[钻取](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.drillinthemeanimation)
        </td>
        <td>当用户在逻辑层次结构中向前导航时（如从主列表到详细信息页面），使用钻入动画。 当用户在逻辑层次结构中向后导航时（如从详细信息页面到主页），使用钻出动画。
        </td>
    </tr>
    <tr>
        <td>[淡化](motion-fade.md)
        </td>
        <td>使用淡化动画将项目引入或引出视图。 两个常见淡化动画是淡入和淡出。
        </td>
    </tr>
        <tr>
        <td>[视差](parallax.md)
        </td>
        <td>视差视觉效果有助于打造一种深度、透视和移动感。 它可以通过在 UI 滚动或平移时以不同速率移动不同对象来实现这种效果。
        </td>
    </tr> 
    <tr>
        <td>[按下反馈](motion-pointer.md)
        </td>
        <td>指针按下动画可在用户点击某个项目时向用户提供视觉反馈。 当第一次点击某个项目时，指针向下动画会略微缩小和倾斜所按的项目，并且会进行播放。 当用户释放指针时，会播放指针向上动画，这会将该项目还原到其原始位置。
        </td>
    </tr>
</table>