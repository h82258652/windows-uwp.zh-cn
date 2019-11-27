---
Description: 列表显示并启用与基于集合的内容交互。
title: 集合和列表
ms.assetid: C73125E8-3768-46A5-B078-FDDF42AB1077
label: Collections and Lists
template: detail.hbs
ms.date: 10/08/2019
ms.topic: article
keywords: windows 10, uwp
pm-contact: anawish
design-contact: kimsea
dev-contact: ranjeshj
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 4605f759c554c12368325a7c1e42143319eddede
ms.sourcegitcommit: 503fa613c65236660350794b4f066eccebe9ac8e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2019
ms.locfileid: "74162354"
---
# <a name="collections-and-lists"></a>集合和列表

集合和列表都是指多个显示在一起的相关数据项的表示形式。 集合可以由不同的集合控件（也可称为集合视图）通过多种方式来表示。 集合控件显示并启用与基于集合的内容（例如联系人列表、日期列表、图像集合等）的交互。

> **重要的 API**：[ListView 类](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView)、[GridView 类](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView)、[FlipView 类](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.flipview)、[TreeView 类](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.treeview)、[ItemsRepeater 类](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.itemsrepeater?view=winui-2.2)

本文介绍的控件包括：

- 列表视图，主要用于显示以文字为主的内容集合
- 网格视图，主要用于显示以图像为主的内容集合
- 翻转视图，主要用于显示以图像为主的内容集合，这些集合要求一次刚好聚焦一个项
- 树视图，主要用于按特定层次结构显示以文字为主的内容集合
- ItemsRepeater，一个可自定义的构建基块，用于创建自定义集合控件

下面为每种控件提供了设计指南、功能和示例。

这些控件（ItemsRepeater 除外）都提供内置的样式设置和交互。 但是，若要进一步自定义集合视图以及其中的项的视觉外观，需使用 [DataTemplate](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DataTemplate)。 若要详细了解数据模板以及如何自定义集合视图的外观，可参阅[项目容器和模板](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/item-containers-templates)页。

这些控件（ItemsRepeater 除外）都还有内置的行为，允许选择单个或多个项。 请参阅 [Selection modes overview](selection-modes.md)（选择模式概述）以了解详细信息。

> **Windows 10 Fall Creators Update - 行为更改** 默认情况下，主动笔现在可在 UWP 应用中滚动/平移列表，而不是进行选择（与触摸、触摸板和被动笔一样）。
> 如果你的应用取决于以前的行为，你可以替代笔滚动，并还原为以前的行为。 有关详细信息，请参阅 [ScrollViewer 类](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer)的 API 参考主题。

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果已安装 <strong style="font-weight: semi-bold">XAML 控件库</strong>应用，请查看实际操作中的 <a href="xamlcontrolsgallery:/item/ListView">ListView</a>、<a href="xamlcontrolsgallery:/item/GridView">GridView</a>、<a href="xamlcontrolsgallery:/item/FlipView">FlipView</a>、<a href="xamlcontrolsgallery:/item/TreeView">TreeView</a> 和 <a href="xamlcontrolsgalley:/item/ItemsRepeater">ItemsRepeater</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="list-views"></a>列表视图

列表视图用于表示以文字为主的项，这些项通常采用单列垂直堆叠布局。 借助列表视图，你可以对项进行分类并为之分配组标头，还可以拖放项、规划内容以及重新对项进行排序。

### <a name="is-this-the-right-control"></a>这是正确的控件吗？

使用列表视图可执行以下操作：

- 显示一个主要由基于文本的项组成的集合，集合中的所有项应该有相同的视觉和交互行为。
- 表示单个或已分类的内容集合。
- 囊括各种用例，包括以下常见用例：
    - 创建消息或消息日志的列表。
    - 创建联系人列表。
    - 在[大纲/细节模式](master-details.md)下创建大纲窗格。 大纲/细节模式常用于电子邮件应用中，其中一个窗格（大纲）具有一个包含可选项的列表，而另一个窗格（细节）具有一个包含选定项的详细视图。
    

### <a name="examples"></a>示例

下面是一个简单的列表视图。此视图显示一个联系人列表，并按字母顺序对数据项分组。 也可自定义组标头（在此示例中为字母表的每个字母），使之保持“粘滞性”。例如，在滚动时，它们会始终显示在 ListView 顶部。

![具有分组数据的列表视图](images/listview-grouped-example-resized-final.png)

下面是一个反转的用于显示消息日志的 ListView，最新的消息显示在底部。 ListView 反转时，会通过内置的动画将项显示在屏幕底部。

![反转的列表视图](images/listview-inverted-2.png)

### <a name="related-articles"></a>相关文章
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">主题</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="listview-and-gridview.md">列表视图和网格视图</a></p></td>
<td align="left"><p>了解在应用中使用列表视图或网格视图的基本知识。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="item-containers-templates.md">项目容器和模板</a></p></td>
<td align="left"><p>在列表或网格视图中显示的项可能在应用的整体外观中扮演重要的角色。 可以通过修改控件模板和数据模板来自定义集合项的外观，使应用拥有精美的外观。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="item-templates-listview.md">列表视图项模板</a></p></td>
<td align="left"><p>使用这些 ListView 的示例项模板可获得常见应用类型的外观。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="inverted-lists.md">倒排列表</a></p></td>
<td align="left"><p>反转列表在底部添加新项目，例如在聊天应用中。 按此文的指南操作，在应用中使用反转列表。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="pull-to-refresh.md">下拉刷新</a></p></td>
<td align="left"><p>下拉刷新机制允许用户使用触控来下拉数据列表，以便检索更多数据。 按此文的要求在列表视图中实现下拉刷新。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="nested-ui.md">嵌套 UI</a></p></td>
<td align="left"><p>嵌套 UI 是公开包含在容器内的可操作控件的用户界面，并且用户同样可以对该容器执行操作。 例如，你可能有包含一个按钮的列表视图项，并且用户可以选择该列表项，或按嵌套在其中的按钮。 按照这些最佳做法为用户提供最佳的嵌套 UI 体验。</p></td>
</tr>
</tbody>
</table>

## <a name="grid-views"></a>网格视图

网格视图适用于排列和浏览基于图像的内容集合。 网格视图布局沿垂直方向滚动，而沿水平方向平移。 项采用自动换行布局，按从左到右、从上到下的阅读顺序显示。

### <a name="is-this-the-right-control"></a>这是正确的控件吗？

使用网格视图可执行以下操作：

- 显示一个内容集合，其中的每个项的焦点是一个图像，且每个项应该有相同的视觉和交互行为。
- 显示内容库。
- 设置与[语义式缩放](semantic-zoom.md)相关联的两个内容视图的格式。
- 囊括各种用例，包括以下常见用例：
    - 店面类型的用户界面（例如，浏览应用、歌曲、产品）
    - 交互式照片库

### <a name="examples"></a>示例

本示例显示了一个典型的网格视图布局，在该示例中用于浏览应用。 网格视图项目的元数据通常限制为几行文本和某一项目分级。

![网格视图布局示例](images/controls_gridview_example02.png)

对于常用于显示图片和视频等媒体的内容库而言，网格视图为理想之选。 在内容库中，用户希望能够通过点击项目来激活某项操作。

![内容库示例](images/gridview-simple-example-final.png)

### <a name="related-articles"></a>相关文章
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">主题</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="listview-and-gridview.md">列表视图和网格视图</a></p></td>
<td align="left"><p>了解在应用中使用列表视图或网格视图的基本知识。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="item-containers-templates.md">项目容器和模板</a></p></td>
<td align="left"><p>在列表或网格视图中显示的项可能在应用的整体外观中扮演重要的角色。 可以通过修改控件模板和数据模板来自定义集合项的外观，使应用拥有精美的外观。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="item-templates-gridview.md">网格视图项模板</a></p></td>
<td align="left"><p>使用这些 GridView 的示例项模板可获得常见应用类型的外观。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="nested-ui.md">嵌套 UI</a></p></td>
<td align="left"><p>嵌套 UI 是公开包含在容器内的可操作控件的用户界面，并且用户同样可以对该容器执行操作。 例如，可以让网格视图项包含一个按钮，这样用户就可以选择该网格项或者按嵌套在其中的按钮。 按照这些最佳做法为用户提供最佳的嵌套 UI 体验。</p></td>
</tr>
</tbody>
</table>

## <a name="flip-views"></a>翻转视图

翻转视图适用于浏览基于图像的内容集合，具体说来就是要求一次只显示一个图像的情况。 翻转视图允许用户移动或“翻转”集合项（不管是以垂直方式还是以水平方式），在用户进行交互后一次显示一个项。

### <a name="is-this-the-right-control"></a>这是正确的控件吗？

使用翻转视图可执行以下操作：

- 显示小到中型集合（少于 25 个项），集合中的图像没有元数据或元数据很少。
- 一次显示一个项，允许最终用户随意翻转这些项。
- 囊括各种用例，包括以下常见用例：
    - 照片库
    - 产品库或展示

### <a name="examples"></a>示例

以下两个示例显示的 FlipView 分别为水平翻转和垂直翻转。

![水平翻转视图](images/controls_flipview_horizonal.jpg)

![垂直翻转视图](images/controls_flipview_vertical.jpg)

### <a name="related-articles"></a>相关文章
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">主题</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="flipview.md">翻转视图</a></p></td>
<td align="left"><p>了解在应用中使用翻转视图以及在翻转视图中自定义项的外观的基本知识。</p></td>
</tr>
</tbody>
</table>

## <a name="tree-views"></a>树视图

树视图适用于显示基于文本的集合，此类集合有需要展示的重要层次结构。 树视图项可以折叠/展开，在可视化层次结构中显示，可以带图标，并且可以在树视图之间拖放。 树视图允许 N 级嵌套。

### <a name="is-this-the-right-control"></a>这是正确的控件吗？

使用树视图可执行以下操作：

- 显示嵌套项的集合，这些项的上下文和含义取决于某个层次结构或特定的组织链。
- 囊括各种用例，包括以下常见用例：
    - 文件浏览器
    - 公司组织结构图

### <a name="examples"></a>示例

下面是树视图的示例。此树视图表示一个文件资源管理器，显示多个不同的带图标的嵌套项。

![带图标的树视图](images/treeview-icons.png)

### <a name="related-articles"></a>相关文章
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">主题</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="tree-view.md">树视图</a></p></td>
<td align="left"><p>了解在应用中使用树视图以及在树视图中自定义项的外观和交互行为的基本知识。</p></td>
</tr>
</tbody>
</table>

## <a name="itemsrepeater"></a>ItemsRepeater

ItemsRepeater 不同于此页上显示的其他集合控件，因为它不提供任何现成的样式设置或交互。也就是说，如果将它直接置于页面上，则不会定义任何属性。 ItemsRepeater 更像是一个构建基块，可供开发人员用来创建自己的自定义集合控件，具体说来就是那种不能使用本文中的其他控件实现的控件。 ItemsRepeater 是一种数据驱动型高性能面板，可以按具体需要定制。

### <a name="is-this-the-right-control"></a>这是正确的控件吗？

在以下情况下使用 ItemsRepeater：

- 已构思具体的用户界面和用户体验，但该界面和体验无法使用现有的集合控件来实现。
- 已经有适用于项的现有数据源（例如从 Internet 拉取的数据、数据库，或者代码隐藏中预先存在的集合）。

### <a name="examples"></a>示例

下面的三个示例都是 ItemsRepeater 控件，这些控件绑定到同一数据源（数字集合）。 该数字集合以三种方式表示，下面的每个 ItemsRepeater 使用不同的自定义 [Layout](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.layout) 和不同的自定义 [ItemTemplate](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate?view=winui-2.2)。

![使用水平条形表示法的 ItemsRepeater](images/itemsrepeater-1.png)
![使用垂直条形表示法的 ItemsRepeater](images/itemsrepeater-2.png)
![使用圆形表示法的 ItemsRepeater](images/itemsrepeater-3.png)

### <a name="related-articles"></a>相关文章
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">主题</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="items-repeater.md">ItemsRepeater</a></p></td>
<td align="left"><p>了解在应用中使用 ItemsRepeater 以及为集合视图实现所有必需的交互和可视化组件的基本知识。</p></td>
</tr>
</tbody>
</table>


## <a name="globalization-and-localization-checklist"></a>全球化和本地化清单

<table>
<tr>
<th>总结</th><td>允许两行列表标签。</td>
</tr>
<tr>
<th>水平扩展</th><td>确保字段可以适应文本扩展，并且可以滚动。</td>
</tr>
<tr>
<th>垂直间距</th><td>对垂直间距使用非拉丁字符，确保非拉丁脚本能够正确显示。</td>
</tr>
</table>

## <a name="get-the-sample-code"></a>获取示例代码

- [XAML 控件库示例](https://github.com/Microsoft/Xaml-Controls-Gallery) - 以交互式格式查看所有 XAML 控件。

## <a name="related-articles"></a>相关文章

**设计和 UX 指南**
- [大纲/细节](master-details.md)
- [导航窗格](navigationview.md)
- [语义式缩放](semantic-zoom.md)
- [拖放](https://docs.microsoft.com/windows/uwp/app-to-app/drag-and-drop)
- [缩略图图像](../../files/thumbnails.md)

**API 参考**
- [ListView 类](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView)
- [GridView 类](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView)
- [ComboBox 类](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox)
- [ListBox 类](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListBox)
