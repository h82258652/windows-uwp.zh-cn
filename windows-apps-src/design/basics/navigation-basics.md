---
Description: 通用 Windows 平台 (UWP) 应用中的导航基于一个导航结构、导航元素和系统级功能的灵活模型。
title: UWP 应用的导航基础知识
ms.assetid: B65D33BA-AAFE-434D-B6D5-1A0C49F59664
label: Navigation design basics
template: detail.hbs
op-migration-status: ready
ms.date: 07/16/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 1c764eeb57ec8046a93e7fb58e156fa68daea8df
ms.sourcegitcommit: 13fe5d04bdb43c75d0fc4de18c2c3d4ae58ff982
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/28/2019
ms.locfileid: "64564522"
---
# <a name="navigation-design-basics-for-uwp-apps"></a>UWP 应用的导航设计基础知识

![导航基础知识标题](images/nav/navigation-basics-header.jpg)

如果将应用看作是一个页面集合，那么*导航* 描述的是在页面之间和页面内的移动行为。 它是用户体验的起点，也是用户查找感兴趣的内容和功能的方式。 它非常重要，但可能很难做到理想化。

关于导航，我们面临着大量的选择。 我们可以：

:::row:::
    :::column:::
        ![navigation example 1](images/nav/nav-1.svg)

要求用户完成一系列页面的顺序。
    :::column-end:::
    :::column:::
        ![navigation example 2](images/nav/nav-2.svg)

提供一个菜单，使用户可以直接跳转到任何页面。
    :::column-end:::
    :::column:::
        ![navigation example 3](images/nav/nav-3.svg)

将所有内容放在单个页面上，并提供筛选机制可用于查看内容。
    :::column-end:::
:::row-end:::

尽管没有哪一个导航设计能够适合所有应用，但有一些原则和指南可帮助你为应用确定合适的设计。

## <a name="principles-of-good-navigation"></a>良好导航原则

让我们开始了解良好导航设计的基本原则：

- **一致性：** 满足用户的期望。
- **简单：** 不需要多做。
- **清晰性：** 提供清晰的路径和选项。

### <a name="consistency"></a>一致性

导航应该与用户期望一致。 使用[标准控件](#use-the-right-controls)用户是图标，位置，熟悉和以下标准约定和样式设置将使导航易于预测和直观的用户。

![页面组件图像](images/nav/page-components.svg)

> 用户希望在标准位置找到某些 UI 元素。

### <a name="simplicity"></a>简易性

减少导航项将为用户简化决策。 提供对重要目的地的轻松访问和隐藏不太重要的项目将帮助用户更快地获得所需的内容。

:::row:::
    :::column:::
        ![do example](images/nav/do.svg)

        ![navview good](images/nav/navview-good.svg)

在熟悉的导航菜单中提供导航项。
    :::column-end:::
    :::column:::
        ![don't example](images/nav/dont.svg)

        ![navview bad](images/nav/navview-bad.svg)

严重影响具有许多导航选项的用户。
    :::column-end:::
:::row-end:::

### <a name="clarity"></a>清晰

清晰的路径使用户能够进行逻辑导航。 让导航选项显而易见并阐明页面之间的关系可防止用户迷失。

![良好示例](images/nav/clarity-image.svg)

> 清楚地标记目的地，使用户知道身在何处。

## <a name="general-recommendations"></a>常规建议

现在，让我们记下我们的设计原则（一致、简单和清晰）并利用这几个原则来得出一些一般建议。

1. 考虑用户的想法。 找出他们可能用来访问你的应用的典型路径；对于每个页面，思考为何用户在那里以及他们可能想去哪里。

2. 避免使用深层导航层次结构。 如果导航级别超过三个，你将面临让用户置身于深度分层、难以脱身的风险。

3. 避免“弹跳”。 “弹跳”会在以下情况下发生：存在相关内容，但导航到该内容需要用户先转到上一级，然后再返回。

## <a name="use-the-right-structure"></a>使用正确的结构

既然你不熟悉一般导航原则，你如何确定应用的结构？ 有两个一般结构：平面和分层。

:::row:::
    :::column:::
        ![Pages arranged in a flat structure](images/nav/flat-lateral-structure.svg)
    :::column-end:::
    :::column span="2":::
        ### Flat/lateral

在平面/横向结构中，页面是并排的。 你可以按任意顺序从一个页面转到另一个页面。

我们建议在以下情况下使用平面结构：

- 可以按任意顺序查看页面。
- 页面之间明显不同，并且不具有明显的父/子关系。
- 在组中有少于 8 个页。 <br>
（当存在多个页面时，用户可能难以区分页面或难以弄清它们当前在组中的位置。 如果你认为这对你的应用不构成问题，请继续将页面作为对等方平行排列。 否则，请考虑使用层次结构将页面分为两个或更多的组。）

    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![Pages arranged in a hierarchy](images/nav/hierarchical-structure.svg)
    :::column-end:::
    :::column span="2":::
        ### Hierarchical

在分层结构中，页面将组织为树状结构。 每个子页面有一个父页面，但一个父页面可以有一个或多个子页面。 若要访问子页面，必须经过父页面。

分层结构适用于组织跨很多页面的复杂内容。 缺点是存在一些导航开销：结构越深入，在页面之间切换所需要的点击次数越多。

我们建议一种分层结构时：
        
- 应该以特定的顺序遍历页面。
- 页面之间存在清晰的父-子关系。
- 组中的页面多于 7 个。
        
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![an app with a hybrid structure](images/nav/combining-structures.svg)
    :::column-end:::
    :::column span="2":::
        ### Combining structures

您不具有选择一个结构或另一种;许多设计良好的应用程序同时使用。 应用对可以以任何顺序查看的顶层页面使用平面结构，对具有更复杂关系的页面使用分层结构。

如果你的导航结构具有多个级别，我们建议使对等导航元素仅链接到其当前子树内的对等方。 请考虑相邻图显示了具有两个级别的导航结构：

- 在级别 1，对等导航元素应该使用户可以访问页面 A、B、C 和 D。
- 在级别 2 上，A2 页面的对等导航元素应该仅链接到其他 A2 页面。 它们不应链接到 C 子树中的级别 2 页面。
    :::column-end:::
:::row-end:::

## <a name="use-the-right-controls"></a>使用正确的控件

对页面结构作出决定后，你需要决定用户如何在这些页面之间导航。 UWP 提供了各种导航控件来帮助确保在你的应用中实现一致且可靠的导航体验。

:::row:::
    :::column:::
        ![Frame image](images/nav/thumbnail-frame.svg)
    :::column-end:::
    :::column span="2":::
        [**Frame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame)

除少数例外情况，所有具有多个页面的应用均使用了一个帧。 通常，应用有一个包含帧和主导航元素的主页面，如导航视图控件。 当用户选择页面时，帧将加载并显示此页面。
:::row-end:::

:::row:::
    :::column:::
        ![tabs and pivot image](images/nav/thumbnail-tabs-pivot.svg)
    :::column-end:::
    :::column span="2":::
        [**Top navigation and tabs**](../controls-and-patterns/navigationview.md)

显示指向同级别页面的水平链接列表。 [NavigationView](../controls-and-patterns/navigationview.md)控件实现顶部导航栏和选项卡模式。
        
使用顶部导航时：

- 你想要显示在屏幕上的所有导航选项。
- 所需应用程序的内容的更多的空间。
- 图标无法清楚地描述您导航的类别。
        
使用选项卡时：

- 你想要保留导航历史记录和页面状态。
- 你希望用户用户经常选项卡之间切换。

:::row-end:::

:::row:::
    :::column:::
         ![tabs and pivot image](images/nav/thumbnail-tabs-pivot.svg)
    :::column-end:::
        :::column span="2":::
    [**Pivot**](../controls-and-patterns/pivot.md)
    
类似于[导航视图](../controls-and-patterns/navigationview.md)，但其他支持的触摸和略有不同的导航行为。
    
使用数据透视表时：
- 希望您的应用程序允许触摸轻扫类别之间
- 要导航到轮播 infintely 选项
- 不需要全面控制两个分类之间的导航行为

:::row-end:::

:::row:::
    :::column:::
        ![navview image](images/nav/thumbnail-navview.svg)
    :::column-end:::
    :::column span="2":::
        [**Left navigation**](../controls-and-patterns/navigationview.md)

显示指向顶层页面的垂直链接列表。 何时使用：
        
- 这些页面存在于顶层。
- 有许多导航项 (多个 5)
- 你预期用户不会在页面之间频繁切换。

:::row-end:::
        
:::row:::
    :::column:::
        ![Master details image](images/nav/thumbnail-master-detail.svg)
    :::column-end:::
    :::column span="2":::
        [**Master/details**](../controls-and-patterns/master-details.md)

显示项目的列表（大纲视图）。 通过选择项目，可在细节部分中显示其相应的页面。 何时使用：
        
- 你预期用户在子项目之间频繁切换。
- 你希望用户能够对单个项目或项目组执行高级别操作（例如删除或排序），并且还希望用户可以查看或更新每个项目的详细信息。

大纲/细节非常适合电子邮件收件箱、联系人列表和数据输入。
:::row-end:::

:::row:::
    :::column:::
        ![Hyperlinks and buttons image](images/nav/thumbnail-hyperlinks-buttons.svg)
    :::column-end:::
    :::column span="2":::
        [**Hyperlinks**](../controls-and-patterns/hyperlinks.md)

嵌入式导航元素可显示在页面的内容中。 与应该在页面中保持一致的其他导航元素不同，嵌入式导航元素在不同页面上是唯一的。
:::row-end:::

## <a name="next-add-navigation-code-to-your-app"></a>下一步：将导航代码添加到您的应用程序

下一篇文章是[实现基本导航](navigate-between-two-pages.md)，将介绍在应用中使用 Frame 控件支持两个页面之间的基本导航所需的代码。
