---
Description: 通用 Windows 平台 (UWP) 应用中的导航基于一个导航结构、导航元素和系统级功能的灵活模型。
title: 通用 Windows 平台 (UWP) 应用的导航设计基础知识
ms.assetid: B65D33BA-AAFE-434D-B6D5-1A0C49F59664
label: 导航设计基础知识
template: detail.hbs
---

#  UWP 应用的导航设计基础知识


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


通用 Windows 平台 (UWP) 应用中的导航基于一个导航结构、导航元素和系统级功能的灵活模型。 它们配合使用以提供在应用、页面和内容之间移动的各种直观用户体验。

在某些情况下，你可能可以将应用的所有内容和功能装配到单个页面，只需用户在该内容上平移而无需执行任何其他操作。 但是，大部分应用通常具有多个页面的内容和功能以供浏览、参与和交互。 当应用具有多个页面时，你需要提供正确的导航体验。

为了成功地使用户理解，UWP 应用中的多页面导航体验包括（稍后介绍详细内容）：

-   **正确的导航结构**

    创建直观的导航体验的关键是生成对用户有意义的导航结构。

-   支持所选结构的**兼容导航元素**。

    导航元素可以帮助用户访问所需的内容，并且还可让用户知道他们在应用内所处的位置。 但是，它们也会占用本来可用于内容或命令元素的空间，因此使用适合应用结构的导航元素非常重要。

-   **对系统级导航功能的适当响应（如“后退”）。**

    若要提供感觉直观的一致体验，请以可预测的方式响应系统级导航功能。

## <span id="Build_the_right_navigation_structure"> </span> <span id="build_the_right_navigation_structure"> </span> <span id="BUILD_THE_RIGHT_NAVIGATION_STRUCTURE"> </span>构建正确的导航结构


让我们将应用视为多组页面的集合，其中每个页面包含一组唯一的内容或功能。 例如，照片应用可能具有一个用于拍摄照片的页面、一个用于图像编辑的页面以及另一个用于管理图像库的页面。 你将这些页面排列成组的方式定义了应用的导航结构。 排列一组页面有两种常见方法：

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">按层次结构排列</th>
<th align="left">作为对等方</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><img src="images/nav/nav-pages-hiearchy.png" alt="Pages arranged in a hierarchy" /></p></td>
<td align="left"><p><img src="images/nav/nav-pages-peer.png" alt="Pages arranged as peers" /></p></td>
</tr>
<tr class="even">
<td align="left"><p>页面将组织为树状结构。 每个子页面只有一个父页面，但是一个父页面可以具有一个或多个子页面。 若要访问子页面，必须经过父页面。</p></td>
<td align="left"><p>页面并行存在。 你可以按任意顺序从一个页面转到另一个页面。</p></td>
</tr>
</tbody>
</table>

 

典型的应用将同时使用这两种排列方式，其中一些部分将作为对等方平行排列，另外一些部分将按层次结构排列。

![带有混合结构的应用](images/nav/nav-hybridstructure.png.png)

那么，你何时应该将页面排列为层次结构，何时应该将它们作为对等方平行排列？ 要回答该问题，我们必须考虑组中的页面数量、是否应该以特定的顺序遍历页面以及页面之间的关系。 一般情况下，扁平的结构更易于理解并且导航更快速，但是有些情况适合使用深层的层次结构。

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p>在以下情况下我们建议使用层次关系：</p>
<ul>
<li><p>你预期用户以特定的顺序遍历页面。 按层次结构排列以强制执行该顺序。</p></li>
<li><p>组中的一个页面和其他页面之间有明确的父子关系。</p></li>
<li><p>如果组中的页面多于 7 个。</p>
<p>当组中的页面多于 7 个时，用户可能难以区分页面或难以弄清他们当前在组中的位置。 如果你认为这对你的应用不构成问题，请继续将页面作为对等方平行排列。 否则，请考虑使用层次结构将页面分为两个或更多的组。 （中心控件可帮助你将页面按类别分组。）</p></li>
</ul></td>
<td align="left"><p>在以下情况下我们建议使用对等关系：</p>
<ul>
<li>可以按任意顺序查看页面。</li>
<li>页面之间明显不同，并且不具有明显的父/子关系。</li>
<li><p>组中的页面少于 8 个。</p>
<p>当组中的页面多于 7 个时，用户可能难以区分页面或难以弄清他们当前在组中的位置。 如果你认为这对你的应用不构成问题，请继续将页面作为对等方平行排列。 否则，请考虑使用层次结构将页面分为两个或更多的组。 （中心控件可帮助你将页面按类别分组。）</p></li>
</ul></td>
</tr>
</tbody>
</table>

 

## <span id="Use_the_right_navigation_elements"> </span> <span id="use_the_right_navigation_elements"> </span> <span id="USE_THE_RIGHT_NAVIGATION_ELEMENTS"> </span>使用正确的导航元素


导航元素可以提供两种服务：它们可以帮助用户访问所需的内容，而且某些元素还可让用户知道他们在应用内所处的位置。 但是，它们也会占用应用本来可用于内容或命令元素的空间，因此使用适合应用结构的导航元素非常重要。

### <span id="Peer-to-peer_navigation_elements"> </span> <span id="peer-to-peer_navigation_elements"> </span> <span id="PEER-TO-PEER_NAVIGATION_ELEMENTS"> </span>对等导航元素

对等导航元素支持在同一个子树的相同级别中的页面之间导航。

![对等导航](images/nav/nav-lateralmovement.png)

对于对等导航，我们建议使用选项卡或导航窗格。

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">导航元素</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[Tabs and pivot](../controls-and-patterns/tabs-pivot.md)</p>
<p><img src="images/nav/nav-tabs-sm-300.png" alt="Tab-based navigation" /></p></td>
<td align="left">显示指向同级别页面的持久链接列表。
<p>在以下情况下使用选项卡/透视表：</p>
<ul>
<li><p>存在 2 到 5 个页面。</p>
<p>（可以在多于 5 个页面时使用选项卡/透视表，但是屏幕上可能无法完整显示所有选项卡/透视表。）</p></li>
<li>你预期用户会在页面之间频繁切换。</li>
</ul>
<p>寻找餐馆应用的此设计使用表/透视表：</p>
<p><img src="images/food-truck-finder/uap-foodtruck-tabletphone-sbs-sm-400.png" alt="Example of an app using tabs/pivots pattern" /></p></td>
</tr>
<tr class="even">
<td align="left"><p>[Nav pane](../controls-and-patterns/nav-pane.md)</p>
<p><img src="images/nav/nav-navpane-4page-thumb.png" alt="A navigation pane" /></p></td>
<td align="left">显示指向顶级页面的链接列表。
<p>在以下情况下使用导航窗格：</p>
<ul>
<li>你预期用户不会在页面之间频繁切换。</li>
<li>你希望以减慢导航操作速度为代价节省空间。</li>
<li>这些页面存在于顶层。</li>
</ul>
<p>此智能家居应用的设计使用导航窗格：</p>
<p><img src="images/smart-home/uap-smarthome-tabletphone-sbs-sm-400.png" alt="Example of an app that uses a nav pane pattern" /></p>
<p></p></td>
</tr>
</tbody>
</table>

 

如果你的导航结构具有多个级别，我们建议使对等导航元素仅链接到其当前子树内的对等方。 请考虑以下图示，该图显示了具有三个级别的导航结构：

![具有两个子树的应用](images/nav/nav-subtrees.png)
-   对于级别 1，对等导航元素应该使用户可以访问页面 A、B、C 和 D。
-   在级别 2 上，A2 页面的对等导航元素应该仅链接到其他 A2 页面。 它们不应链接到 C 子树中的级别 2 页面。

![具有两个子树的应用](images/nav/nav-subtrees2.png)

### <span id="Hierarchical_navigation_elements"> </span> <span id="hierarchical_navigation_elements"> </span> <span id="HIERARCHICAL_NAVIGATION_ELEMENTS"> </span>分层导航元素

分层导航元素提供父页面及其子页面之间的导航。

![分层导航](images/nav/nav-verticalmovement.png)

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">导航元素</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[Hub](../controls-and-patterns/hub.md)</p>
<p><img src="images/higsecone-hub-thumb.png" alt="Hub" /></p></td>
<td align="left">中心是特殊类型的导航控件，可提供其子页面的预览/摘要。 与导航窗格或选项卡不同，它通过嵌入页面本身的链接和章节标题来提供对这些子页面的导航。
<p>在以下情况下使用中心：</p>
<ul>
<li>你预期用户希望查看子页面的某些内容，而无需导航到每个页面。</li>
</ul>
<p>中心有助于发现与探索，这使它们非常适合媒体、新闻阅读器和购物应用。</p>
<p></p></td>
</tr>
<tr class="even">
<td align="left"><p>[Master/details](../controls-and-patterns/master-details.md)</p>
<p><img src="images/higsecone-masterdetail-thumb.png" alt="Master/details" /></p></td>
<td align="left">显示项目摘要的列表（大纲视图）。 通过选择项目，可在细节部分中显示其相应的项目页面。
<p>在以下情况下使用大纲/细节元素：</p>
<ul>
<li>你预期用户在子项目之间频繁切换。</li>
<li>你希望用户能够对单个项目或项目组执行高级别操作（例如删除或排序），并且还希望用户可以查看或更新每个项目的详细信息。</li>
</ul>
<p>大纲/细节元素非常适合电子邮件收件箱、联系人列表和数据输入。</p>
<p>此股票跟踪应用的设计使用大纲/细节模式：</p>
<p><img src="images/stock-tracker/uap-finance-tabletphone-sbs-sm.png" alt="Example of a stock trading app that has a master/details pattern" /></p></td>
</tr>
</tbody>
</table>

 

### <span id="Historical_navigation_elements"> </span> <span id="historical_navigation_elements"> </span> <span id="HISTORICAL_NAVIGATION_ELEMENTS"> </span>历史导航元素

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">导航元素</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">返回</td>
<td align="left"><p>让用户遍历应用内和应用之间（具体取决于设备）的导航历史记录。 有关详细信息，请参阅本文后面显示的 [Make your app work well with system-level navigation features](#backnavigation) 部分。</p></td>
</tr>
</tbody>
</table>

 

### <span id="Content-embedded_navigation_elements"> </span> <span id="content-embedded_navigation_elements"> </span> <span id="CONTENT-EMBEDDED_NAVIGATION_ELEMENTS"> </span>嵌入内容的导航元素

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">导航元素</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">超链接和按钮</td>
<td align="left"><p>嵌入内容的导航元素显示在页面的内容中。 与应该在页面的组或子树中保持一致的其他导航元素不同，嵌入内容的导航元素在不同页面上是唯一的。</p></td>
</tr>
</tbody>
</table>

 

### <span id="Combining_navigation_elements"> </span> <span id="combining_navigation_elements"> </span> <span id="COMBINING_NAVIGATION_ELEMENTS"> </span>合并导航元素

你可以通过合并导航元素来创建适合你的应用的导航体验。 例如，你的应用可使用导航窗格来提供对顶级页面的访问，而使用选项卡来提供对二级页面的访问。


\[本文包含特定于 UWP 应用和 Windows 10 的信息。 有关 Windows 8.1 指南，请下载 [Windows 8.1 指南 PDF](https://go.microsoft.com/fwlink/p/?linkid=258743)。\]



 






<!--HONumber=Mar16_HO1-->


