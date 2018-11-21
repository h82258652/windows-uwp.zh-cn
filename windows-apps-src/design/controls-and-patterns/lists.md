---
author: muhsinking
Description: Lists display and enable interaction with collection-based content.
title: 列表
ms.assetid: C73125E8-3768-46A5-B078-FDDF42AB1077
label: Lists
template: detail.hbs
ms.author: mukin
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: predavid
design-contact: kimsea
dev-contact: ranjeshj
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 3594697292d6dfe9435036b838a0ba4bd2dbfc05
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/21/2018
ms.locfileid: "7437533"
---
# <a name="lists"></a>列表

列表显示并启用与基于集合的内容交互。 本文中介绍的四种列表模式如下：

-   列表视图，主要用于显示以文字为主的内容集合
-   网格视图，主要用于显示以图像为主的内容集合
-   下拉列表，用于让用户从展开式列表中选择某一项
-   列表框，用于让用户从可滚动的框中选择一个或多个项

已为每种列表模式提供了设计指南、功能和示例。

> **重要 API**：[ListView 类](https://msdn.microsoft.com/library/windows/apps/br242878)、[GridView 类](https://msdn.microsoft.com/library/windows/apps/br242705)、[ComboBox 类](https://msdn.microsoft.com/library/windows/apps/br209348)


> <div id="main">
> <strong>Windows 10 Fall Creators Update - 行为更改</strong>
> </div>
> 默认情况下，主动笔现在可在 UWP 应用中滚动/平移列表，而不是进行选择（与触摸、触摸板和被动笔一样）。
> 如果你的应用取决于以前的行为，你可以替代笔滚动，并还原为以前的行为。 请参阅有关详细信息的[ScrollViewer 类](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer)API 参考主题。

## <a name="list-views"></a>列表视图

借助列表视图，你可以对项目进行分类并为之分配组标头、拖放项目、规划内容以及重新对项目进行排序。

### <a name="is-this-the-right-control"></a>这是正确的控件吗？

使用列表视图可执行以下操作：

-   显示主要组成部分为文本的内容集合。
-   导航单个或已分类的内容集合。
-   在[大纲/细节模式](master-details.md)下创建大纲窗格。 大纲/细节模式常用于电子邮件应用中，其中一个窗格（大纲）具有一个包含可选项的列表，而另一个窗格（细节）具有一个包含选定项的详细视图。

### <a name="examples"></a>示例

下面是在手机上显示分组数据的简单列表视图。

![具有分组数据的列表视图](images/simple-list-view-phone.png)

### <a name="recommendations"></a>建议

-   列表内的项应具有相同的行为。
-   如果你的列表划分为多个组，可以使用[语义式缩放](semantic-zoom.md)，使用户更轻松地浏览分组内容。

### <a name="list-view-articles"></a>列表视图文章
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
<td align="left"><p>你在列表或网格中显示的项目可能在应用的整体外观中扮演重要的角色。 修改控件模板和数据模板以定义项目的外观，并使应用外观精美。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="item-templates-listview.md">列表视图项模板</a></p></td>
<td align="left"><p>使用这些 ListView 的示例项模板可获得常见应用类型的外观。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="inverted-lists.md">反转列表</a></p></td>
<td align="left"><p>反转列表在底部添加新项目，例如在聊天应用中。 遵循本指南在应用中使用反转列表。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="pull-to-refresh.md">下拉刷新</a></p></td>
<td align="left"><p>下拉刷新模式允许用户使用触摸来下拉数据列表，以便检索更多数据。 使用本指南在列表视图中实现下拉刷新。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="nested-ui.md">嵌套 UI</a></p></td>
<td align="left"><p>嵌套 UI 是公开包含在容器内的可操作控件的用户界面，并且用户同样可以对该容器执行操作。 例如，你可能有包含一个按钮的列表视图项，并且用户可以选择该列表项，或按嵌套在其中的按钮。 按照这些最佳做法为用户提供最佳的嵌套 UI 体验。</p></td>
</tr>
</tbody>
</table>

## <a name="grid-views"></a>网格视图

网格视图适用于排列和浏览基于图像的内容集合。 网格视图布局沿垂直方向滚动，而沿水平方向平移。 项的布局顺序为从左到右，其阅读顺序为从上到下。

### <a name="is-this-the-right-control"></a>这是正确的控件吗？

使用列表视图可执行以下操作：

-   显示主要组成部分为图像的一个内容集合。
-   显示内容库。
-   设置与[语义式缩放](semantic-zoom.md)相关联的两个内容视图的格式。

### <a name="examples"></a>示例

本示例显示了一个典型的网格视图布局，在该示例中用于浏览应用。 网格视图项目的元数据通常限制为几行文本和某一项目分级。

![网格视图布局示例](images/controls_gridview_example02.png)

对于常用于显示图片和视频等媒体的内容库而言，网格视图为理想之选。 在内容库中，用户希望能够通过点击项目来激活某项操作。

![内容库示例](images/controls_list_contentlibrary.png)

### <a name="recommendations"></a>建议

-   列表内的项应具有相同的行为。
-   如果你的列表划分为多个组，可以使用[语义式缩放](semantic-zoom.md)，使用户更轻松地浏览分组内容。

### <a name="grid-view-articles"></a>网格视图文章
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
<td align="left"><p>你在列表或网格中显示的项目可能在应用的整体外观中扮演重要的角色。 修改控件模板和数据模板以定义项目的外观，并使应用外观精美。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="item-templates-gridview.md">网格视图项模板</a></p></td>
<td align="left"><p>使用这些 GridView 的示例项模板可获得常见应用类型的外观。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="nested-ui.md">嵌套 UI</a></p></td>
<td align="left"><p>嵌套 UI 是公开包含在容器内的可操作控件的用户界面，并且用户同样可以对该容器执行操作。 例如，你可能有包含一个按钮的列表视图项，并且用户可以选择该列表项，或按嵌套在其中的按钮。 按照这些最佳做法为用户提供最佳的嵌套 UI 体验。</p></td>
</tr>
</tbody>
</table>

## <a name="drop-down-lists"></a>下拉列表

下拉列表（也称为组合框）最初处于紧凑状态，可展开以显示可选择项的列表。 选定项始终可见，而不可见的项可在用户点击组合框时进入视图以将其展开。

### <a name="is-this-the-right-control"></a>这是正确的控件吗？

-   使用下拉列表可让用户从一组项中选择单个值，这些项可使用单行文本完全显示。
-   使用列表视图或网格视图（而非组合框），显示包含多行文本或多张图像的项。
-   当少于五项时，应考虑使用[单选按钮](radio-button.md)（当进行单项选择时）或[复选框](checkbox.md)（当进行多项选择时）。
-   当选择项在应用流中不太重要时使用组合框。 如果在多数情况下为大部分用户推荐了默认选项，则通过使用列表视图显示所有项可能会导致用户过多地注意这些选项。 使用组合框可以节省空间并减少分心。

### <a name="examples"></a>示例

处于紧凑状态的组合框可以显示标题。

![处于紧凑状态的下拉列表示例](images/combo_box_collapsed.png)

尽管组合框可通过展开来支持较长的字符串，但应避免使用难以阅读的过长的字符串。

![带有长文本字符串的下拉列表示例](images/combo_box_listitemstate.png)

如果组合框中的集合足够长，将显示滚动栏来容纳它。 以逻辑方式对列表中的项进行分组。

![下拉列表中的滚动栏示例](images/combo_box_scroll.png)

### <a name="recommendations"></a>建议

-   将组合框的文本内容限制为单行。
-   以最合乎逻辑的顺序对组合框中的项进行排序。 将相关选项组合到一起并将最常见的选项置于顶部。 按字母顺序对名称进行排序、按数字顺序对数字进行排序，并按时间先后顺序对日期进行排序。
-   若要创建可在用户使用箭头键（如字体所选内容下拉列表）时实时更新的组合框，请将 SelectionChangedTrigger 设置为“Always”。  

### <a name="text-search"></a>文本搜索

组合框自动支持其集合内的搜索。 当焦点位于打开或关闭的组合框上时，如果用户在物理键盘上键入字符，与用户的字符串匹配的候选项将引入视图。 当在长列表中导航时，此功能尤其有用。 例如，当与包含状态列表的下拉列表交互时，用户可以按“w”键来将“Washington”引入视图，以供快速选择。


## <a name="list-boxes"></a>列表框

列表框允许用户从集合中选择单个项或多个项。 列表框类似于下拉列表，区别在于列表框始终处于打开状态，并且不存在紧凑式状态的列表框（不可进行展开）。 当没有空间来显示所有内容时，可滚动列表中的项。

### <a name="is-this-the-right-control"></a>这是正确的控件吗？

-   当列表中的项重要到需突出显示时，以及当没有足够的屏幕空间可用来显示整个列表时，列表框会很有用。
-   列表框应将用户的注意力吸引到一项重要选择的全套备选项。 相比之下，下拉列表的设计初衷则是将用户的注意力吸引到某一选定项。
-   当出现以下情况时，应避免使用列表框：
    -   列表中的项数非常少。 始终具有两个相同选项的单选列表框可能更适合呈现为[单选按钮](radio-button.md)。 当列表中有 3 到 4 个静态项时，也可考虑使用单选按钮。
    -   列表框为单选式且始终具有两个相同选项，其中一个可表示为另一个的对立面，例如“打开”和“关闭”。 使用单个复选框或切换开关。
    -   项目数非常多。 网格视图和列表视图更适用于较长列表。 对于很长的分组数据列表，首选语义式缩放。
    -   项为相邻的数字值。 如果是这种情况，应考虑使用[滑块](slider.md)。
    -   选择项在你的应用流中不太重要，或者说对于大部分情况下的大部分用户，建议使用默认选项。 改用下拉列表。

### <a name="recommendations"></a>建议

-   在列表框中，项数的理想范围为 3 到 9 个。
-   当列表框中的项可以动态变化时，其效果颇佳。
-   如果可以，请设置列表框的大小，这样便无需平移或滚动其项列表。
-   验证列表框的目的以及当前选择的项目是否明确。
-   保留用于触摸反馈和选择的项目状态的视觉效果和动画。
-   将列表框项目的文本内容限制为单行。 如果项是视觉对象，你可以自定义其大小。 如果项包括多行文本或多个图像，请改用网格视图或列表视图。
-   使用默认字体，除非你的品牌指南指示使用其他字体。
-   不要使用列表框执行命令或者动态显示或隐藏其他控件。

## <a name="selection-mode"></a>选择模式

选择模式允许用户选择单个或多个项并对其执行操作。 可通过上下文菜单调用此模式，方法是在项上使用 CTRL+单击或 SHIFT+单击组合键，或者在库视图中的某一项上滚动选择目标。 当选择模式处于活动状态时，每个列表项旁都将显示复选框，而操作将显示在屏幕的顶部或底部。

有以下三种选择模式：

-   单选：用户一次只能选择一个项目。
-   多选：用户无需使用修改器即可选择多个项目。
-   展开：用户可以使用修改器选择多个项目，例如长按 SHIFT 键。

在项目上点击任意位置即可将其选中。 点击命令栏操作会影响所有选定项。 如果没有选择任何项目，命令栏操作应处于非活动状态，除了“全选”。

选择模式没有弱解除模型；在选择模式处于活动状态的框架之外点击不会取消该模式。 这是为了防止意外停用该模式。 单击后退按钮可关闭多选模式。

当选中某项操作时，会显示可视化确认。 请考虑为某些操作显示确认对话框，特别是诸如删除等破坏性操作。

选择模式仅限于它处于活动状态的页面，并不会影响该页面之外的任何项。

选择模式的入口点应对应于它所影响的内容。

有关命令栏建议，请参阅[命令栏指南](app-bars.md)。

## <a name="globalization-and-localization-checklist"></a>全球化和本地化清单

<table>
<tr>
<th>总结</th><td>允许两行列表标签。</td>
</tr>
<tr>
<th>水平扩展</th><td>确保字段可以适应文本扩展，并且可以滚动。</td>
</tr>
<tr>
<th>垂直间距</th><td>为垂直间距使用非拉丁字符以确保非拉丁脚本能够正确显示。</td>
</tr>
</table>


## <a name="related-articles"></a>相关文章

- [中心](hub.md)
- [大纲/细节](master-details.md)
- [导航窗格](navigationview.md)
- [语义式缩放](semantic-zoom.md)
- [拖放](https://msdn.microsoft.com/windows/uwp/app-to-app/drag-and-drop)
- [缩略图图像](../../files/thumbnails.md)

**适用于开发人员**
- [ListView 类](https://msdn.microsoft.com/library/windows/apps/br242878)
- [GridView 类](https://msdn.microsoft.com/library/windows/apps/br242705)
- [ComboBox 类](https://msdn.microsoft.com/library/windows/apps/br209348)
- [ListBox 类](https://msdn.microsoft.com/library/windows/apps/br242868)