---
Description: Pivot 控件启用触摸轻扫一小部分的内容部分之间。
title: Pivot
template: detail.hbs
ms.date: 06/19/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: llongley
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 56079bc51d3efa8f7ecaaee21379a6e9caf7d440
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57642922"
---
# <a name="pivot"></a>Pivot

[Pivot](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot)控件使触摸轻扫一小部分的内容部分之间。

> **重要的 API**：[透视类](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot)， [NavigationView 类](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.NavigationView)

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果有<strong style="font-weight: semi-bold">XAML 控件库</strong>应用程序安装，请单击此处<a href="xamlcontrolsgallery:/item/Pivot">打开应用并查看操作中的 Pivot 控件</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Pivot 控件，就像[NavigationView](navigationview.md)，用下划线标出所选的项。

![默认焦点为选择的标题添加下划线](images/pivot_focus_selectedHeader.png)

## <a name="is-this-the-right-control"></a>这是正确的控件吗？

若要实现常见的顶部导航栏和选项卡模式，我们建议使用[NavigationView](navigationview.md)，自动适应不同屏幕大小和允许进行更高版本的自定义。

但是，如果您的导航需要触摸轻扫，我们建议使用透视。

NavigationView 和 Pivot 控件之间的其他主要区别是默认溢出行为和导航 API:

- 透视转盘溢出项，而 NavigationView 使用菜单下拉列表中溢出，以便用户可以看到所有项。
- Pivot 处理内容的部分，而 NavigationView 允许更灵活地控制导航行为之间进行导航。

## <a name="use-navigationview-instead-of-pivot"></a>使用而不是 Pivot 的 NavigationView

如果应用程序的 UI 使用 Pivot 控件，然后您可以将转换透视为 NavigationView 下面的代码。

此 XAML 创建包含 3 个部分的内容，如示例透视 NavigationView 中[创建一个 pivot 控件](#create-a-pivot-control)。

```xaml
<NavigationView x:Name="rootNavigationView" Header="Category Title"
 ItemInvoked="NavView_ItemInvoked">
    <NavigationView.MenuItems>
        <NavigationViewItem Content="Section 1" x:Name="Section1Content" />
        <NavigationViewItem Content="Section 2" x:Name="Section2Content" />
        <NavigationViewItem Content="Section 3" x:Name="Section3Content" />
    </NavigationView.MenuItems>
</NavigationView>

<Page x:Class="AppName.Section1Page">
    <TextBlock Text="Content of section 1."/>
</Page>

<Page x:Class="AppName.Section2Page">
    <TextBlock Text="Content of section 2."/>
</Page>

<Page x:Class="AppName.Section3Page">
    <TextBlock Text="Content of section 3."/>
</Page>
```

NavigationView 可提供更好地控制导航自定义，并且需要相应的代码隐藏。 若要提供补充上述 XAML，使用以下代码隐藏：

```csharp
private void NavView_ItemInvoked(NavigationView sender, NavigationViewItemInvokedEventArgs args)
{
    FrameNavigationOptions navOptions = new FrameNavigationOptions();
    navOptions.TransitionInfoOverride = new SlideNavigationTransitionInfo() {
         SlideNavigationTransitionDirection=args.RecommendedNavigationTransitionInfo
    };

    navOptions.IsNavigationStackEnabled = False;

    switch (item.Name)
    {
        case "Section1Content":
            ContentFrame.NavigateToType(typeof(Section1Page), null, navOptions);
            break;

        case "Section2Content":
            ContentFrame.NavigateToType(typeof(Section2Page), null, navOptions);
            break;

        case "Section3Content":
            ContentFrame.NavigateToType(typeof(Section3Page), null, navOptions);
            break;
    }  
}
```

此代码会模拟透视控件的内置导航体验，减去内容部分的触摸轻扫体验。 但是，正如您所看到的则您可能还用自定义多个点，包括动画的转换、 导航参数和堆栈功能。

## <a name="create-a-pivot-control"></a>创建透视表控件

此代码将创建 3 个部分的内容与基本透视控件。

```xaml
<Pivot x:Name="rootPivot" Title="Category Title">
    <PivotItem Header="Section 1">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of section 1."/>
    </PivotItem>
    <PivotItem Header="Section 2">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of section 2."/>
    </PivotItem>
    <PivotItem Header="Section 3">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of section 3."/>
    </PivotItem>
</Pivot>
```

### <a name="pivot-items"></a>透视表项目

透视表是一个 [ItemsControl](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.aspx)，因此可以包含任何类型的项目集合。 你添加到透视表的任何非显式 [PivotItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivotitem.aspx) 的项目都隐式包装在 PivotItem 中。 由于透视表经常用于在内容页面之间导航，因此通常使用 XAML UI 元素直接填充 [Items](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.items.aspx) 集合。 或者，你可以将 [ItemsSource](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemssource.aspx) 属性设置为数据源。 ItemsSource 中绑定的项目可以属于任何类型，但如果它们不是显式 PivotItems，则必须定义 [ItemTemplate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx) 和 [HeaderTemplate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.headertemplate.aspx) 来指定这些项目的显示方式。

你可以使用 [SelectedItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.selecteditem.aspx) 属性获取或设置透视表的活动项目。 使用 [SelectedIndex](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.selectedindex.aspx) 属性获取或设置活动项目的索引。

### <a name="pivot-headers"></a>透视表标题

你可以使用 [LeftHeader](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.leftheader.aspx) 和 [RightHeader](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.rightheader.aspx) 属性将其他控件添加到透视表标题。

例如，你可以在透视表的 RightHeader 中添加 [CommandBar](https://docs.microsoft.com/en-us/windows/uwp/controls-and-patterns/app-bars)。

```xaml
<Pivot>
    <Pivot.RightHeader>
        <CommandBar>
                <AppBarButton Icon="Add"/>
                <AppBarSeparator/>
                <AppBarButton Icon="Edit"/>
                <AppBarButton Icon="Delete"/>
                <AppBarSeparator/>
                <AppBarButton Icon="Save"/>
        </CommandBar>
    </Pivot.RightHeader>
</Pivot>
```

### <a name="pivot-interaction"></a>透视表交互

该控件的特色之处在于以下触摸手势交互：

- 通过点击透视表项目标题，可导航到该标题的部分内容。
- 通过在透视表项目标题上向左或向右轻扫，可导航到相邻部分。
- 通过在部分内容上向左或向右轻扫，可导航到相邻部分。

该控件有以下两种模式：

**保持静止**

- 当所有透视表标题都适合所允许的空间时，透视表将固定不动。
- 点击某个透视表标签即可导航到相应的页面，即使透视表无法自行移动也是如此。 活动透视表将突出显示。

**轮播**

- 当所有透视表标题不适合所允许的空间时，可旋转透视表。
- 点击某个透视表标签即可导航到相应的页面，并且活动透视表标签将旋转至第一个位置。
- 从最后一个到第一个透视表部分的旋转循环中的透视表项目。

> **注意**透视表标题不应在 [10 英尺环境](../devices/designing-for-tv.md)中旋转。 如果你的应用将在 Xbox 上运行，请将 [IsHeaderItemsCarouselEnabled](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot.IsHeaderItemsCarouselEnabled) 属性设置为 **false**。

## <a name="recommendations"></a>建议

- 当使用旋转（来回旋转）模式时，避免使用 5 个以上的标题，因为超过 5 个标题的循环可能会令人困惑。

## <a name="get-the-sample-code"></a>获取示例代码

- [XAML 控件库示例](https://github.com/Microsoft/Xaml-Controls-Gallery) - 以交互式格式查看所有 XAML 控件。

## <a name="related-topics"></a>相关主题

- [透视类](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot)
- [导航设计基础知识](../basics/navigation-basics.md)