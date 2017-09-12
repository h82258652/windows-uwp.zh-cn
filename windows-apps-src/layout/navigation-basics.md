---
author: mijacobs
Description: "通用 Windows 平台 (UWP) 应用中的导航基于一个导航结构、导航元素和系统级功能的灵活模型。"
title: "UWP 应用的导航基础知识"
ms.assetid: B65D33BA-AAFE-434D-B6D5-1A0C49F59664
label: Navigation design basics
template: detail.hbs
op-migration-status: ready
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: a944e02ea116c0e054918397d9d46d366d43622a
ms.sourcegitcommit: 10d6736a0827fe813c3c6e8d26d67b20ff110f6c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/22/2017
---
# <a name="navigation-design-basics-for-uwp-apps"></a>UWP 应用的导航设计基础知识

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

如果你将应用看作是一个页面集合，那么术语*导航*描述的是在页面之间和页面内的移动行为。 它是用户体验的起点。 表明了用户如何查找感兴趣的内容和功能。 它非常重要，而且可能很难做到完全合适。 

> **重要 API**：[Frame](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.Frame)、[Pivot 类](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.Pivot)、[NavigationView 类](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.NavigationView)

难以达到完全合适的原因之一在于，作为应用设计人员，我们需要进行大量选择。 如果我们设计的是一本书，我们的选择会很简单：使用什么顺序加入章节。 使用应用，我们可以创建一个模拟书的导航体验，需要用户按顺序浏览一组页面。 或者我们可以提供一个菜单，让用户直接跳转到他或她希望看到的任何页面 - 但如果页面太多，我们可能会让用户陷入选择难题。 或者，我们可以将所有内容放在单个页面上，并提供查看内容的筛选机制。 

虽然没有哪一个导航设计能够适合所有应用，但你可以遵循一组原则和指南来帮助你为你的应用确定合适的设计。 

## <a name="principles-of-good-design"></a>良好设计原则 
我们从基本原则开始，研究发现遵循这些原则能够为良好的导航设计奠定基础： 

* 保持一致：达到用户期望。
* 保持简单：根据需要，不要过度。
* 保持简洁：不要让导航功能对用户造成干扰。

### <a name="be-consistent"></a>保持一致 
导航应该与用户期望保持一致，基于图标、位置和样式的标准约定。 

例如，在下图中，你可以看到用户通常希望能够在该处找到功能的位置，如导航窗格和命令栏。 不同的设备系列拥有自己的导航元素约定。 例如，平板电脑的导航窗格通常显示在屏幕左侧，而移动设备则是在最上方。

<figure class="wdg-figure">
  ![导航元素的首选位置](images/nav/nav-component-layout.png)
  <figcaption>用户希望在标准位置找到某些 UI 元素。</figcaption>
</figure> 

### <a name="keep-it-simple"></a>保持简单
导航设计的另一个重要因素是希克海曼定律，经常在提及导航选项时被引用。 这个定律鼓励我们少在菜单中添加选项。 选项添加的越多，用户与之交互的速度越慢，尤其是用户在探索新应用时。 

<figure class="wdg-figure">
  ![简单菜单与复杂菜单对比](images/nav/nav-simple-menus.png)
  <figcaption> 在左侧，我们注意到，有很少的几个选项供用户选择，而在右侧则有若干个。 希克海曼定律表明左侧的菜单对于用户来说可以更轻松地理解和利用。
</figcaption>
</figure> 

### <a name="keep-it-clean"></a>保持简洁
导航的最后一个关键特点是简洁交互，这是指用户与各个上下文的导航交互的物理方式。 遵循这个原则时，将你自己放在用户位置上会对你的设计带来积极影响。 尝试了解你的用户和他们的行为。 例如，如果是设计烹饪应用，你可以考虑为购物单和计时器提供轻松访问。 

## <a name="three-general-rules"></a>三个一般规则
现在我们记下我们的设计原则 - 一致、简单、简洁交互 - 并使用这几个原则来得出一些一般规则。 与任何经验法则一样，将它们作为出发点，并根据需要调整。 

1. 避免过多的导航分层。 多少导航级别最适合你的用户？ 一个顶层导航再加一级通常就够了。 如果导航超出三个级别，那么你打破了简单原则。 更糟的是，你面临着让用户置身于深度分层、难以脱身的风险。

2. 避免过多导航选项。 每个级别三到六个导航元素是最常见的。 如果你的导航需要更多元素，尤其是在层次结构的顶层，那么你可以考虑将应用拆分为多个应用，不然你可能需要在一个位置进行太多尝试。 应用中的导航元素太多通常会导致目标不一致、不相关。

3. 避免“弹跳”。 “弹跳”会在以下情况下发生：存在相关内容，但导航到该内容需要用户先转到上一级，然后再返回。 “弹跳”违反了简洁交互原则，因为需要不必要的单击或交互才能到达明显的目标 - 这种情况下，参看一下同系列文章的相关内容。 （此规则的例外情况是搜索和浏览，这种时候“弹跳”可能是提供所需的多样性和深度的唯一途径。）
<figure class="wdg-figure">
  ![“弹跳”示例](images/nav/nav-pogo-sticking-1.png)
  <figcaption> “弹跳”着在应用中导航 - 用户必须返回（绿色后退箭头）到主页，才能够导航到“项目”选项卡。
</figcaption>
</figure> 
<figure class="wdg-figure">
  ![通过轻扫实现的横向轻扫修复了“弹跳”问题](images/nav/nav-pogo-sticking-2.png)
  <figcaption>你可以使用一个图标（注意绿色的轻扫手势）来解决一些“弹跳”问题。
</figcaption>
</figure> 


## <a name="use-the-right-structure"></a>使用正确的结构 
既然你已经熟悉了一般导航原则和规则，现在应该做出最重要的导航决定了：你应该如何构建应用结构？ 有两个一般结构：平面和分层。 

### <a name="flatlateral"></a>平面/横向
在平面/横向结构中，页面是并排的。 你可以按任意顺序从一个页面转到另一个页面。 
<figure class="wdg-figure">
  <img src="images/nav/nav-pages-peer.png" alt="Pages arranged in a flat structure" />
<figcaption>以平面结构排列的页面</figcaption>
</figure> 

平面结构有很多优点：简单且容易理解，用户可以直接跳转到特定页面，而无需越过中间页面。  一般情况下，平面结构都能有出色表现 - 但并不适用于每个应用。 如果你的应用有很多页面，平面列表可能难以应对。 如果需要按照特定顺序查看页面，平面结构则无法胜任。 

我们建议在以下情况下使用平面结构： 
<ul>
<li>可以按任意顺序查看页面。</li>
<li>页面之间明显不同，并且不具有明显的父/子关系。</li>
<li>组中的页面少于 8 个。<br/>
当组中的页面多于 7 个时，用户可能难以区分页面或难以弄清他们当前在组中的位置。 如果你认为这对你的应用不构成问题，请继续将页面作为对等方平行排列。 否则，请考虑使用层次结构将页面分为两个或更多的组。 （中心控件可帮助你将页面按类别分组。）</li>
</ul>


### <a name="hierarchical"></a>分层

在分层结构中，页面将组织为树状结构。 每个子页面只有一个父页面，但是一个父页面可以具有一个或多个子页面。 若要访问子页面，必须经过父页面。

<figure class="wdg-figure">
  <img src="images/nav/nav-pages-hiearchy.png" alt="Pages arranged in a hierarchy" />
<figcaption>分层排列的页面</figcaption>
</figure>

分层结构非常适用于组织跨很多页面的复杂内容，或应按照特定顺序查看页面的情况。 缺点是分层页面产生了一些导航开销：结构越深入，用户在页面之间切换所需要的点击次数越多。 

我们建议在以下情况下使用分层结构： 
<ul>
<li>你预期用户以特定的顺序遍历页面。 按层次结构排列以强制执行该顺序。</li>
<li>组中的一个页面和其他页面之间有明确的父子关系。</li>
<li>组中的页面多于 7 个。<br/>
当组中的页面多于 7 个时，用户可能难以区分页面或难以弄清他们当前在组中的位置。 如果你认为这对你的应用不构成问题，请继续将页面作为对等方平行排列。 否则，请考虑使用层次结构将页面分为两个或更多的组。 （中心控件可帮助你将页面按类别分组。）</li>
</ul>

### <a name="combining-structures"></a>组合结构
你不一定非要二选一；许多设计合理的应用同时使用平面结构和分层结构：

![带有混合结构的应用](images/nav/nav-hybridstructure.png.png)

这些应用为可以以任何顺序查看的顶层页面使用平面结构，为具有更复杂关系的页面使用分层结构。 

如果你的导航结构具有多个级别，我们建议使对等导航元素仅链接到其当前子树内的对等方。 请考虑以下图示，该图显示了具有三个级别的导航结构：

![具有两个子树的应用](images/nav/nav-subtrees.png)
-   对于级别 1，对等导航元素应该使用户可以访问页面 A、B、C 和 D。
-   在级别 2 上，A2 页面的对等导航元素应该仅链接到其他 A2 页面。 它们不应链接到 C 子树中的级别 2 页面。

![具有两个子树的应用](images/nav/nav-subtrees2.png)
 

## <a name="use-the-right-controls"></a>使用正确的控件

对页面结构作出决定后，你需要决定用户如何在这些页面之间导航。 UWP 提供了各种导航控件来为你提供帮助。 由于这些控件可用于每个 UWP 应用，使用它们有助于确保一致且可靠的导航体验。 


<table>
<tr>
    <th>控件</th>
    <th>描述</th>
</tr>
<tr>
    <td>[帧](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.Frame)</td>
    <td>除少数例外情况，所有具有多个页面的应用均使用帧。 在典型设置中，应用将有一个包含帧和主导航元素的主页面，如导航视图控件。 当用户选择页面时，帧将加载并显示此页面。</td>
</tr>
<tr>
    <td>[表和透视表](../controls-and-patterns/tabs-pivot.md)<br/><br/>
    <img src="images/nav/nav-tabs-sm-300.png" alt="Tab-based navigation" /></td>
    <td>显示指向同级别页面的链接的列表。
<p>在以下情况下使用选项卡/透视表：</p>
<ul>
<li><p>存在 2 到 5 个页面。</p>
<p>（可以在多于 5 个页面时使用选项卡/透视表，但是屏幕上可能无法完整显示所有选项卡/透视表。）</p></li>
<li>你预期用户会在页面之间频繁切换。</li>
</ul></td>
</tr>
<tr>
    <td>[导航视图](../controls-and-patterns/navigationview.md)<br/><br/>
    <img src="images/nav/nav-navpane-4page-thumb.png" alt="A navigation pane" /></td>
    <td>显示指向顶层页面的链接列表。
<p>在以下情况下使用导航窗格：</p>
<ul>
<li>你预期用户不会在页面之间频繁切换。</li>
<li>你希望以减慢导航操作速度为代价节省空间。</li>
<li>这些页面存在于顶层。</li>
</ul></td>
</tr>
<tr>
<td>[大纲/细节](../controls-and-patterns/master-details.md)<br/><br/>
<img src="images/higsecone-masterdetail-thumb.png" alt="Master/details" /></td>
<td>显示项目摘要的列表（大纲视图）。 通过选择项目，可在细节部分中显示其相应的项目页面。
<p>在以下情况下使用大纲/细节元素：</p>
<ul>
<li>你预期用户在子项目之间频繁切换。</li>
<li>你希望用户能够对单个项目或项目组执行高级别操作（例如删除或排序），并且还希望用户可以查看或更新每个项目的详细信息。</li>
</ul>
<p>大纲/细节元素非常适合电子邮件收件箱、联系人列表和数据输入。</p>
</td>
</tr>
<tr>
<td s>[返回](navigation-history-and-backwards-navigation.md)</td>
<td style="vertical-align:top;">让用户遍历应用内和应用之间（具体取决于设备）的导航历史记录。 有关详细信息，请参阅[导航历史记录和向后导航文章](navigation-history-and-backwards-navigation.md)。</td>
</tr>
<tr class="odd">
<td>超链接和按钮</td>
<td>嵌入内容的导航元素显示在页面的内容中。 与应该在页面的组或子树中保持一致的其他导航元素不同，嵌入内容的导航元素在不同页面上是唯一的。</td>
</tr>
</table>

## <a name="next-add-navigation-code-to-your-app"></a>下一步：向你的应用添加导航代码
下一篇文章是[实现基本导航](navigate-between-two-pages.md)，将介绍在应用中使用帧控件支持基本导航所需的 XAML 和代码。 


<!--
## History and the back button
UWP provides a back button and a system for traversing the user's navigation hsitory within an app. This system does most of the work for you, but there are a few APIs you need to call so that it works properly. For more info and instructions, see [History and backwards navigation](navigation-history-and-backwards-navigation.md). 
-->





