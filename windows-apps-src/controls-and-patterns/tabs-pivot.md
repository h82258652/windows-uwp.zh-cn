---
Description: 利用表和透视表，用户可以在经常访问的内容之间导航。
title: 表和透视表
ms.assetid: 556BC70D-CF5D-4295-A655-D58163CC1824
label: 表和透视表
template: detail.hbs
---
# 表和透视表

表和透视表用于导航经常访问的不同内容类别。 表/透视表模式由两个或多个具有相应类别标题的内容窗格构成。 标题将保留在屏幕上且具有一个清晰显示的选择状态，以便用户可以始终知道自己所在的类别。
![表示例](images/HIGSecOne_Tabs.png)

表和透视表实际上是相同的模式，并且两者均使用[**透视表**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.aspx)控件生成。 透视表控件的基本功能将在本文的后面部分进行介绍。

<span class="sidebar_heading" style="font-weight: bold;">重要的 API</span>

-   [**Pivot 类**](https://msdn.microsoft.com/library/windows/apps/dn608241)

## 表/透视表模式

在使用表/透视表模式生成应用时，根据模式的可配置功能集，有一些需要考虑的关键设计变量。

- **标题放置位置。**   标题可放置在屏幕的顶部或底部。
    
    **注意** 将标题放置在屏幕底部需要重新创建透视表控件的模板。
- **标题标签。**  标题可以是带有文本的图标、纯文本或纯图标。
- **标题对齐方式。**  标题可以左对齐，也可以居中对齐。
- **顶级或次级导航。**  表/透视表既可以用于任一级别的导航，也可以在顶级/次级模式下堆栈。 当存在两个级别的表/透视表时，顶级和次级标题具有足够视觉区分度，以便用户能够清楚地区分它们。
- **触摸手势支持。**  对于支持触摸手势的设备，你可以使用以下两组交互之一在不同的内容类别之间进行导航：
    1. 点击表/透视表标题以导航到该类别，或者通过在内容区域上轻扫导航到相邻类别。
    2. 点击表/透视表标题以导航到该类型（不支持轻扫操作）。

### 模式配置

表/透视表模式的最佳排列方式取决于交互方案以及应用将显示在其上的设备。 此表概括了一些顶级方案和模式配置。

交互方案|建议的配置
--------------------|-------------------------
在手机或平板手机上，在 2 到 5个顶级列表或网格视图内容类别之间进行横向移动|表/透视表：居中放在屏幕顶部
|标题标签：图标 + 文本
|在内容区域上轻扫：已启用
在无法通过轻扫内容区域进行导航的手机或平板手机上，在各种内容类别之间进行移动|表/透视表：居中放在屏幕底部
|标题标签：图标 + 文本
|在内容区域上轻扫：已禁用
支持鼠标和键盘的顶级导航|表/透视表：放置在屏幕顶部，左对齐
 *或*|标题标签：仅文本
 触摸设备上的页面级导航|在内容区域上轻扫：已禁用

## 示例

此快餐车应用设计展示了将表/透视表标题放在顶部或底部时可能会呈现的外观。 在移动设备上，将它们放在底部有助于提高可访问性。

![移动设备上的表示例](images/uap_foodtruck_phone_320_tabsboth.png)

针对笔记本电脑/台式机而设计的快餐车应用的特征是使用仅文标题。 将带文本的图标用于标题可帮助确定触摸目标，但对于鼠标和键盘而言，使用仅文本标题的效果会更好。

![台式机上的表示例](images/uap_foodtruck_desktop_home_700.png)

## 创建透视表控件

表/透视表导航模式使用[**透视表**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.aspx)控件生成。 该控件随附本部分中所述的基本功能。

此 XAML 使用 3 个部分的内容创建基本透视表控件。

```xaml
<Pivot x:Name="rootPivot" Title="PIVOT TITLE">
    <PivotItem Header="Pivot Item 1">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of pivot item 1."/>
    </PivotItem>
    <PivotItem Header="Pivot Item 2">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of pivot item 2."/>
    </PivotItem>
    <PivotItem Header="Pivot Item 3">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of pivot item 3."/>
    </PivotItem>
</Pivot>
```

**透视表项目**

透视表是一个 [**ItemsControl**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.aspx)，因此可以包含任何类型的项目集合。 你添加到透视表的任何非显式 [**PivotItem**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivotitem.aspx) 的项目都隐式包装在 PivotItem 中。 由于透视表经常用于在内容页面之间导航，因此通常使用 XAML UI 元素直接填充 [**Items**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.items.aspx) 集合。 或者，你可以将 [**ItemsSource**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemssource.aspx) 属性设置为数据源。 ItemsSource 中绑定的项目可以属于任何类型，但如果它们不是显式 PivotItems，则必须定义 [**ItemTemplate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx) 和 [**HeaderTemplate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.headertemplate.aspx) 来指定这些项目的显示方式。

你可以使用 [**SelectedItem**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.selecteditem.aspx) 属性获取或设置透视表的活动项目。 使用 [**SelectedIndex**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.selectedindex.aspx) 属性获取或设置活动项目的索引。 

**透视表标题**

你可以使用 [**LeftHeader**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.leftheader.aspx) 和 [**RightHeader**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.rightheader.aspx) 属性将其他控件添加到透视表标题。 

### 透视表交互

该控件的特色之处在于以下触摸手势交互：

-   通过点击标题，可导航到该标题的部分内容。
-   通过在标题上向左或向右轻扫，可导航到相邻的标题/部分。
-   通过在部分内容上向左或向右轻扫，可导航到相邻的标题/部分。

该控件支持以下两种模式：

**固定不动**

-   当所有透视表标题都适合所允许的空间时，透视表将固定不动。
-   点击某个透视表标签即可导航到相应的页面，尽管透视表无法自行移动也是如此。 活动透视表将突出显示。

**旋转**

-   当所有透视表标题不适合所允许的空间时，可旋转透视表。
-   点击某个透视表标签即可导航到相应的页面，并且活动透视表标签将旋转至第一个位置。

该控件具有内置的断点功能，该功能基于标题的数目和标签的字符串长度。

## 建议

-   根据屏幕大小来选择表/透视表标题的对齐方式。 对于宽度低于 720 像素的屏幕，通常使用中心对齐效果更佳；而对于宽度高于 720 像素的屏幕，在大多数情况下都推荐使用左对齐。
-   在缩放窗口时，一旦表/透视表标题的数目超出可用的实际使用空间所能容纳的数目，应立即将标题推送到溢出区中。
-   表/透视表可在横向和纵向方向中任一屏幕方向上使用，唯一前提条件是，这两个方向上标题（可见状态和隐藏状态）的总数目需保持相同。
-   当使用旋转（来回旋转）模式时，避免使用 5 个以上的标题，因为超过 5 个标题可能会导致用户难以识别出所使用的屏幕方向。
-   在移动设备上，如果已在 UI 的其他部分中使用轻扫操作，则将表/透视表放在底部既可以有助于提高可访问性，还可以避免 UI 过于繁琐。
-   部署屏幕键盘后，标题可移出屏幕以保留空间。

\[本文包含特定于通用 Windows 平台 (UWP) 应用和 Windows 10 的信息。 有关 Windows 8.1 指南，请下载 [Windows 8.1 指南 PDF](https://go.microsoft.com/fwlink/p/?linkid=258743)。\]

## 相关主题

[导航设计基础知识](https://msdn.microsoft.com/library/windows/apps/dn958438)


<!--HONumber=Mar16_HO1-->


