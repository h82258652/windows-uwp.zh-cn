---
author: serenaz
Description: Navigation in Universal Windows Platform (UWP) apps is based on a flexible model of navigation structures, navigation elements, and system-level features.
title: UWP 应用的导航基础知识
ms.assetid: B65D33BA-AAFE-434D-B6D5-1A0C49F59664
label: Navigation design basics
template: detail.hbs
op-migration-status: ready
ms.author: sezhen
ms.date: 11/27/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3edb7dc28561b5d316a908df951e3052eafc995b
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
ms.locfileid: "1654266"
---
# <a name="navigation-design-basics-for-uwp-apps"></a>UWP 应用的导航设计基础知识

如果你将应用看作是一个页面集合，那么*导航*描述的是在页面之间和页面内的移动行为。 它是用户体验的起点，也是用户查找感兴趣的内容和功能的方式。 它非常重要，但可能很难做到理想化。

困难的原因在于，作为应用设计人员，我们需要进行大量选择。 我们可能需要用户按顺序浏览一系列页面。 或者，我们可以提供一个菜单，使用户能够直接跳到任何页面。 或者，我们可以将所有内容放在单个页面上，并提供查看内容的筛选机制。
 
尽管没有哪一个导航设计能够适合所有应用，但有一些原则和指南可帮助你为应用确定合适的设计。 

<figure class="wdg-figure">
  <img alt="Navigation diagram for an app" src="images/navigation_diagram.png" />
  <figcaption>应用的导航的图示。</figcaption>
</figure> 

## <a name="principles-of-good-navigation"></a>良好导航原则
让我们开始了解良好导航设计的基本原则： 

* 一致：达到用户期望。
* 简单：按需行事，不要过度。
* 清晰：提供清楚的路径和选项。

### <a name="consistency"></a>一致
导航应该与用户期望一致。 通过使用用户熟悉的[标准控件](#use-the-right-controls)并遵循对图标、位置和样式的标准约定，用户将觉得导航可预测且直观。

<figure class="wdg-figure">
<img src="images/nav/nav-component-layout.png" alt="Preferred location for navigation elements"/>
  <figcaption>用户希望在标准位置找到某些 UI 元素。</figcaption>
</figure> 

### <a name="simplicity"></a>简单
减少导航项将为用户简化决策。 提供对重要目的地的轻松访问和隐藏不太重要的项目将帮助用户更快地获得所需的内容。

<figure class="wdg-figure">
<img src="images/nav/nav-simple-menus.png" alt="A simple versus a complex menu"/>
  <figcaption> 左侧的菜单对用户来说更容易理解和利用，因为它的项目更少。
</figcaption>
</figure> 

### <a name="clarity"></a>清晰
清晰的路径使用户能够进行逻辑导航。 让导航选项显而易见并阐明页面之间的关系可防止用户迷失。

<figure class="wdg-figure">
<img src="images/nav/nav-pages.png" alt="Clear paths and destinations"/>
  <figcaption> 清楚地标记目的地，使用户知道身在何处。
</figcaption>
</figure> 

## <a name="general-recommendations"></a>常规建议
现在，让我们记下我们的设计原则（一致、简单和清晰）并利用这几个原则来得出一些一般建议。

1. 考虑用户的想法。 找出他们可能用来访问你的应用的典型路径；对于每个页面，思考为何用户在那里以及他们可能想去哪里。 

2. 避免过多的导航分层。 如果导航级别超过三个，你将面临让用户置身于深度分层、难以脱身的风险。

3. 避免“弹跳”。 “弹跳”会在以下情况下发生：存在相关内容，但导航到该内容需要用户先转到上一级，然后再返回。

## <a name="use-the-right-structure"></a>使用正确的结构 
既然你不熟悉一般导航原则，你如何确定应用的结构？ 有两个一般结构：平面和分层。 

### <a name="flatlateral"></a>平面/横向
![以平面结构排列的页面](images/nav/nav-pages-peer.png)

在平面/横向结构中，页面是并排的。 你可以按任意顺序从一个页面转到另一个页面。 

我们建议在以下情况下使用平面结构： 
<ul>
<li>可以按任意顺序查看页面。</li>
<li>页面之间明显不同，并且不具有明显的父/子关系。</li>
<li>组中的页面少于 8 个。<br/>
（当存在多个页面时，用户可能难以区分页面或难以弄清它们当前在组中的位置。 如果你认为这对你的应用不构成问题，请继续将页面作为对等方平行排列。 否则，请考虑使用层次结构将页面分为两个或更多的组。）</li>
</ul>

### <a name="hierarchical"></a>分层
![分层排列的页面](images/nav/nav-pages-hiearchy.png)

在分层结构中，页面将组织为树状结构。 每个子页面有一个父页面，但一个父页面可以有一个或多个子页面。 若要访问子页面，必须经过父页面。

分层结构适用于组织跨很多页面的复杂内容。 缺点是存在一些导航开销：结构越深入，在页面之间切换所需要的点击次数越多。 

我们建议在以下情况下使用分层结构： 
<ul>
<li>应该以特定的顺序遍历页面。</li>
<li>页面之间存在清晰的父-子关系。</li>
<li>组中的页面多于 7 个。</li>
</ul>

### <a name="combining-structures"></a>组合结构
![带有混合结构的应用](images/nav/nav-hybridstructure.png.png)

你不一定在两种结构中非要二选一；许多设计合理的应用同时使用两者。 应用对可以以任何顺序查看的顶层页面使用平面结构，对具有更复杂关系的页面使用分层结构。 

如果你的导航结构具有多个级别，我们建议使对等导航元素仅链接到其当前子树内的对等方。 请考虑以下图示，该图显示了具有三个级别的导航结构：

![具有两个子树的应用](images/nav/nav-subtrees.png)
- 在级别 1，对等导航元素应该使用户可以访问页面 A、B、C 和 D。
- 在级别 2 上，A2 页面的对等导航元素应该仅链接到其他 A2 页面。 它们不应链接到 C 子树中的级别 2 页面。

![具有两个子树的应用](images/nav/nav-subtrees2.png)

## <a name="use-the-right-controls"></a>使用正确的控件
对页面结构作出决定后，你需要决定用户如何在这些页面之间导航。 UWP 提供了各种导航控件来帮助确保在你的应用中实现一致且可靠的导航体验。 

我们建议根据你的应用中的导航元素数选择导航控件。 如果你有五个或更少的导航项，则使用顶层导航，如[表和透视表](../controls-and-patterns/tabs-pivot.md)。 如果你有六个或更多的导航项，则使用左侧导航，如[导航视图](../controls-and-patterns/navigationview.md)或[大纲/细节](../controls-and-patterns/master-details.md)。

<div class="mx-responsive-img">

<table>
<tr>
    <th>控件</th>
    <th>描述</th>
</tr>
<tr>
    <td><a href="https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.Frame">帧</a><br/><br/>
    <img src="images/frame.png" alt="Frame" /></td>
    <td>显示页面。 <p>除少数例外情况，所有具有多个页面的应用均使用了一个帧。 通常，应用有一个包含帧和主导航元素的主页面，如导航视图控件。 当用户选择页面时，帧将加载并显示此页面。</p></td>
</tr>
<tr>
    <td><a href="../controls-and-patterns/tabs-pivot.md">表和透视表</a><br/><br/>
    <img src="images/nav/nav-tabs-sm-300.png" alt="Tab-based navigation" /></td>
    <td>显示指向同级别页面的水平链接列表。
<p>何时使用：</p>
<ul>
<li>存在 2 到 5 个页面。 （可以在多于 5 个页面时使用选项卡/透视表，但是屏幕上可能无法完整显示所有选项卡/透视表。）</li>
<li>你预期用户会在页面之间频繁切换。</li>
</ul></td>
</tr>
<tr>
    <td><a href="../controls-and-patterns/navigationview.md">导航视图</a><br/><br/>
    <img src="images/nav/nav-navpane-4page-thumb.png" alt="A navigation pane" /></td>
    <td>显示指向顶层页面的垂直链接列表。
<p>何时使用：</p>
<ul>
<li>这些页面存在于顶层。</li>
<li>有很多导航项（多于 5 个）。</li>
<li>你预期用户不会在页面之间频繁切换。</li>

</ul></td>
</tr>
<tr>
<td><a href="../controls-and-patterns/master-details.md">大纲/细节</a><br/><br/>
<img src="images/higsecone-masterdetail-thumb.png" alt="Master/details" /></td>
<td>显示项目的列表（大纲视图）。 通过选择项目，可在细节部分中显示其相应的页面。
<p>何时使用：</p>
<ul>
<li>你预期用户在子项目之间频繁切换。</li>
<li>你希望用户能够对单个项目或项目组执行高级别操作（例如删除或排序），并且还希望用户可以查看或更新每个项目的详细信息。</li>
</ul>
<p>大纲/细节非常适合电子邮件收件箱、联系人列表和数据输入。</p>
</td>
<tr>
<td><a href="../controls-and-patterns/hub.md">中心</a><br/><br/>
<img src="images/hub.png" alt="Hub" /></td>
<td> 在网格中显示订购的商品的部分。 
<p>何时使用：</p>
<ul>
<li>你希望创建具有 hero 图像的视觉导航。</li>
</ul>
<p>中心非常适合浏览、查看或购买体验。</p>
</td>
</tr>
<tr>
<td><a href="../controls-and-patterns/hyperlinks.md">超链接</a>和<a href="../controls-and-patterns/buttons.md">按钮</a></td>
<td> 嵌入式导航元素可显示在页面的内容中。 与应该在页面中保持一致的其他导航元素不同，嵌入式导航元素在不同页面上是唯一的。</td>
</tr>
</table>
</div>

## <a name="next-add-navigation-code-to-your-app"></a>下一步：向你的应用添加导航代码
下一篇文章是[实现基本导航](navigate-between-two-pages.md)，将介绍在应用中使用帧控件支持两个页面之间的基本导航所需的代码。 