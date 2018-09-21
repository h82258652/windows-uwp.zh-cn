---
author: QuinnRadich
Description: Navigation in Universal Windows Platform (UWP) apps is based on a flexible model of navigation structures, navigation elements, and system-level features.
title: UWP 应用的导航基础知识
ms.assetid: B65D33BA-AAFE-434D-B6D5-1A0C49F59664
label: Navigation design basics
template: detail.hbs
op-migration-status: ready
ms.author: quradic
ms.date: 7/16/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b731910f53a6152554b74e946374234b827f4a86
ms.sourcegitcommit: a160b91a554f8352de963d9fa37f7df89f8a0e23
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/21/2018
ms.locfileid: "4122894"
---
# <a name="navigation-design-basics-for-uwp-apps"></a>UWP 应用的导航设计基础知识

![导航基础知识标题](images/nav/navigation-basics-header.jpg)

如果将应用看作是一个页面集合，那么*导航* 描述的是在页面之间和页面内的移动行为。 它是用户体验的起点，也是用户查找感兴趣的内容和功能的方式。 它非常重要，但可能很难做到理想化。

关于导航，我们面临着大量的选择。 我们可以：

:::row:::
    :::column:::
        ![导航示例 1](images/nav/nav-1.svg)

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
        ![良好示例](images/nav/do.svg)

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
        ![以平面结构排列的页面](images/nav/flat-lateral-structure.svg)
    :::column-end:::
    ::: 列范围 ="2":::
        ### Flat/lateral

        In a flat/lateral structure, pages exist side-by-side. You can go from one page to another in any order.

        We recommend using a flat structure when:

        - The pages can be viewed in any order.
        - The pages are clearly distinct from each other and don't have an obvious parent/child relationship.
        - There are less than 8 pages in the group. <br>
        (When there are more pages, it might be difficult for users to understand how the pages are unique or to understand their current location within the group. If you don't think that's an issue for your app, go ahead and make the pages peers. Otherwise, consider using a hierarchical structure to break the pages into two or more smaller groups.)

    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![分层排列的页面](images/nav/hierarchical-structure.svg)
    :::column-end:::
    ::: 列范围 ="2":::
        ### Hierarchical

        In a hierarchical structure, pages are organized into a tree-like structure. Each child page has one parent, but a parent can have one or more child pages. To reach a child page, you travel through the parent.

        Hierarchical structures are good for organizing complex content that spans lots of pages. The downside is some navigation overhead: the deeper the structure, the more clicks it takes to get from page to page.

        We recommend a hierarchical structure when:
        
        - Pages should be traversed in a specific order.
        - There is a clear parent-child relationship between pages.
        - There are more than 7 pages in the group.
        
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![带有混合结构的应用](images/nav/combining-structures.svg)
    :::column-end:::
    ::: 列范围 ="2":::
        ### Combining structures

        You don't have choose to one structure or the other; many well-design apps use both. An app can use flat structures for top-level pages that can be viewed in any order, and hierarchical structures for pages that have more complex relationships.

        If your navigation structure has multiple levels, we recommend that peer-to-peer navigation elements only link to the peers within their current subtree. Consider the adjacent illustration, which shows a navigation structure that has two levels:

        - At level 1, the peer-to-peer navigation element should provide access to pages A, B, C, and D.
        - At level 2, the peer-to-peer navigation elements for the A2 pages should only link to the other A2 pages. They should not link to level 2 pages in the C subtree.
    :::column-end:::
:::row-end:::

## <a name="use-the-right-controls"></a>使用正确的控件

对页面结构作出决定后，你需要决定用户如何在这些页面之间导航。 UWP 提供了各种导航控件来帮助确保在你的应用中实现一致且可靠的导航体验。

:::row:::
    :::column:::
        ![帧图像](images/nav/thumbnail-frame.svg)
    :::column-end:::
    ::: 列范围 ="2":::[**帧**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame)

        With few exceptions, any app that has multiple pages uses a frame. Typically, an app has a main page that contains the frame and a primary navigation element, such as a navigation view control. When the user selects a page, the frame loads and displays it.
:::row-end:::

:::row:::
    :::column:::
        ![表和透视表图像](images/nav/thumbnail-tabs-pivot.svg)
    :::column-end:::
    ::: 列范围 ="2":::[**顶部导航和选项卡**](../controls-and-patterns/navigationview.md)

        Displays a horizontal list of links to pages at the same level. The [NavigationView](../controls-and-patterns/navigationview.md) control implements the top navigation and tabs patterns.
        
        Use top navigation when:

        - You want to show all navigation options on the screen.
        - You desire more space for your app's content.
        - Icons cannot clearly describe your navigation categories.
        
        Use tabs when:

        - You want to preserve navigation history and page state.
        - You expect users to switch between tabs frequently.

:::row-end:::

:::row:::
    :::column:::
        ![navview 图像](images/nav/thumbnail-navview.svg)
    :::column-end:::
    ::: 列范围 ="2":::[**左侧的导航**](../controls-and-patterns/navigationview.md)

        Displays a vertical list of links to top-level pages. Use when:
        
        - The pages exist at the top level.
        - There are many navigation items (more than 5)
        - You don't expect users to switch between pages frequently.
        
:::row-end:::

:::row:::
    :::column:::
        ![大纲细节图像](images/nav/thumbnail-master-detail.svg)
    :::column-end:::
    ::: 列范围 ="2":::[**大纲/细节**](../controls-and-patterns/master-details.md)

        Displays a list (master view) of items. Selecting an item displays its corresponding page in the details section. Use when:
        
        - You expect users to switch between child items frequently.
        - You want to enable the user to perform high-level operations, such as deleting or sorting, on individual items or groups of items, and also want to enable the user to view or update the details for each item.

        Master/details is well suited for email inboxes, contact lists, and data entry.
:::row-end:::

:::row:::
    :::column:::
        ![超链接和按钮的图像](images/nav/thumbnail-hyperlinks-buttons.svg)
    :::column-end:::
    ::: 列范围 ="2":::[**超链接**](../controls-and-patterns/hyperlinks.md)

        Embedded navigation elements can appear in a page's content. Unlike other navigation elements, which should be consistent across the pages, content-embedded navigation elements are unique from page to page.
:::row-end:::

## <a name="next-add-navigation-code-to-your-app"></a>下一步：向应用添加导航代码

下一篇文章是[实现基本导航](navigate-between-two-pages.md)，将介绍在应用中使用 Frame 控件支持两个页面之间的基本导航所需的代码。