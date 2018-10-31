---
author: Jwmsft
Description: Navigation in Universal Windows Platform (UWP) apps is based on a flexible model of navigation structures, navigation elements, and system-level features.
title: UWP 应用的导航基础知识
ms.assetid: B65D33BA-AAFE-434D-B6D5-1A0C49F59664
label: Navigation design basics
template: detail.hbs
op-migration-status: ready
ms.author: jimwalk
ms.date: 7/16/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 82623a86548866a78f56385ee0a535bfcb822c46
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2018
ms.locfileid: "5827066"
---
# <a name="navigation-design-basics-for-uwp-apps"></a>UWP 应用的导航设计基础知识

![导航基础知识标题](images/nav/navigation-basics-header.jpg)

如果将应用看作是一个页面集合，那么*导航* 描述的是在页面之间和页面内的移动行为。 它是用户体验的起点，也是用户查找感兴趣的内容和功能的方式。 它非常重要，但可能很难做到理想化。

关于导航，我们面临着大量的选择。 我们可以：

:::row:::
    :::column:::
        ![navigation example 1](images/nav/nav-1.svg)

        Require users to go through a series of pages in order.
    :::column-end:::
    :::column:::
        ![navigation example 2](images/nav/nav-2.svg)

        Provide a menu that allows users to jump directly to any page.
    :::column-end:::
    :::column:::
        ![navigation example 3](images/nav/nav-3.svg)

        Place everything on a single page and provide filtering mechanisms for viewing content.
    :::column-end:::
:::row-end:::

尽管没有哪一个导航设计能够适合所有应用，但有一些原则和指南可帮助你为应用确定合适的设计。

## <a name="principles-of-good-navigation"></a>良好导航原则

让我们开始了解良好导航设计的基本原则：

- **一致：** 达到用户期望。
- **简单：** 按需行事，不要过度。
- **清晰：** 提供清楚的路径和选项。

### <a name="consistency"></a>一致

导航应该与用户期望一致。 使用[标准控件](#use-the-right-controls)，用户所熟悉并且以下标准约定图标、 位置和样式设置用户将觉得导航可预测且直观。

![页面组件图像](images/nav/page-components.svg)

> 用户希望在标准位置找到某些 UI 元素。

### <a name="simplicity"></a>简单

减少导航项将为用户简化决策。 提供对重要目的地的轻松访问和隐藏不太重要的项目将帮助用户更快地获得所需的内容。

:::row:::
    :::column:::
        ![do example](images/nav/do.svg)

        ![navview good](images/nav/navview-good.svg)

        Present navigation items in a familiar navigation menu.
    :::column-end:::
    :::column:::
        ![don't example](images/nav/dont.svg)

        ![navview bad](images/nav/navview-bad.svg)

        Overwhelm users with many navigation options.
    :::column-end:::
:::row-end:::

### <a name="clarity"></a>清晰

清晰的路径使用户能够进行逻辑导航。 让导航选项显而易见并阐明页面之间的关系可防止用户迷失。

![良好示例](images/nav/clarity-image.svg)

> 清楚地标记目的地，使用户知道身在何处。

## <a name="general-recommendations"></a>常规建议

现在，让我们记下我们的设计原则（一致、简单和清晰）并利用这几个原则来得出一些一般建议。

1. 考虑用户的想法。 找出他们可能用来访问你的应用的典型路径；对于每个页面，思考为何用户在那里以及他们可能想去哪里。

2. 避免深入导航层次结构。 如果导航级别超过三个，你将面临让用户置身于深度分层、难以脱身的风险。

3. 避免“弹跳”。 “弹跳”会在以下情况下发生：存在相关内容，但导航到该内容需要用户先转到上一级，然后再返回。

## <a name="use-the-right-structure"></a>使用正确的结构

既然你不熟悉一般导航原则，你如何确定应用的结构？ 有两个一般结构：平面和分层。

:::row:::
    :::column:::
        ![Pages arranged in a flat structure](images/nav/flat-lateral-structure.svg)
    :::column-end:::
    :::column span="2":::
        ### Flat/lateral

        In a flat/lateral structure, pages exist side-by-side. You can go from one page to another in any order.

        We recommend using a flat structure when:

        - 可以按任意顺序查看页面。
        - 页面之间明显不同，并且不具有明显的父/子关系。
        - 组中有页面少于 8。 <br>
        （当存在多个页面时，用户可能难以区分页面或难以弄清它们当前在组中的位置。 如果你认为这对你的应用不构成问题，请继续将页面作为对等方平行排列。 否则，请考虑使用层次结构将页面分为两个或更多的组。）

    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![Pages arranged in a hierarchy](images/nav/hierarchical-structure.svg)
    :::column-end:::
    :::column span="2":::
        ### Hierarchical

        In a hierarchical structure, pages are organized into a tree-like structure. Each child page has one parent, but a parent can have one or more child pages. To reach a child page, you travel through the parent.

        Hierarchical structures are good for organizing complex content that spans lots of pages. The downside is some navigation overhead: the deeper the structure, the more clicks it takes to get from page to page.

        We recommend a hierarchical structure when:
        
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

        You don't have choose to one structure or the other; many well-design apps use both. An app can use flat structures for top-level pages that can be viewed in any order, and hierarchical structures for pages that have more complex relationships.

        If your navigation structure has multiple levels, we recommend that peer-to-peer navigation elements only link to the peers within their current subtree. Consider the adjacent illustration, which shows a navigation structure that has two levels:

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

        With few exceptions, any app that has multiple pages uses a frame. Typically, an app has a main page that contains the frame and a primary navigation element, such as a navigation view control. When the user selects a page, the frame loads and displays it.
:::row-end:::

:::row:::
    :::column:::
        ![tabs and pivot image](images/nav/thumbnail-tabs-pivot.svg)
    :::column-end:::
    :::column span="2":::
        [**Top navigation and tabs**](../controls-and-patterns/navigationview.md)

        Displays a horizontal list of links to pages at the same level. The [NavigationView](../controls-and-patterns/navigationview.md) control implements the top navigation and tabs patterns.
        
        Use top navigation when:

        - 你想要显示在屏幕上的所有导航选项。
        - 你希望为你的应用的内容的更多空间。
        - 图标不能清楚地描述你导航类别。
        
        使用 tab 键时：

        - 你想要保留导航历史记录和页面状态。
        - 你预期用户经常标签之间进行切换。

:::row-end:::

:::row:::
    :::column:::
        ![navview image](images/nav/thumbnail-navview.svg)
    :::column-end:::
    :::column span="2":::
        [**Left navigation**](../controls-and-patterns/navigationview.md)

        Displays a vertical list of links to top-level pages. Use when:
        
        - 这些页面存在于顶层。
        - 有很多导航项 （多于 5 个）
        - 你预期用户不会在页面之间频繁切换。
        
:::row-end:::

:::row:::
    :::column:::
        ![Master details image](images/nav/thumbnail-master-detail.svg)
    :::column-end:::
    :::column span="2":::
        [**Master/details**](../controls-and-patterns/master-details.md)

        Displays a list (master view) of items. Selecting an item displays its corresponding page in the details section. Use when:
        
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

        Embedded navigation elements can appear in a page's content. Unlike other navigation elements, which should be consistent across the pages, content-embedded navigation elements are unique from page to page.
:::row-end:::

## <a name="next-add-navigation-code-to-your-app"></a>下一步：向应用添加导航代码

下一篇文章是[实现基本导航](navigate-between-two-pages.md)，将介绍在应用中使用 Frame 控件支持两个页面之间的基本导航所需的代码。