---
Description: Pivot 控件可在一小组内容部分之间进行触控滑动。
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
ms.openlocfilehash: bb3d8fc251a357f4251552ba80e4c922878fe805
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970812"
---
# <a name="pivot"></a>Pivot

[Pivot](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot) 控件可在一小组内容部分之间进行触控滑动。

![默认焦点为选择的标题添加下划线](images/pivot_focus_selectedHeader.png)

**获取 Windows UI 库**

|  |  |
| - | - |
| ![WinUI 徽标](images/winui-logo-64x64.png) | Windows UI 库 2.2 或更高版本包含此控件的使用圆角的新模板。 有关详细信息，请参阅[圆角半径](/windows/uwp/design/style/rounded-corner)。 WinUI 是一种 NuGet 包，其中包含用于 Windows 应用的新控件和 UI 功能。 有关详细信息（包括安装说明），请参阅 [Windows UI 库](https://docs.microsoft.com/uwp/toolkits/winui/)。 |

> **平台 API**：[Pivot 类](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot)、[NavigationView 类](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.NavigationView)

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果已安装 XAML 控件库应用，请单击此处<a href="xamlcontrolsgallery:/item/Pivot">打开此应用，了解 Pivot 控件的实际应用</a><strong style="font-weight: semi-bold"></strong>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

与 [NavigationView](navigationview.md) 一样，Pivot 控件强调所选项。

![默认焦点为选择的标题添加下划线](images/pivot_focus_selectedHeader.png)

## <a name="is-this-the-right-control"></a>这是正确的控件吗？

要实现常见的顶部导航和标签模式，建议使用 [NavigationView](navigationview.md)，它会自动适应不同的屏幕尺寸，并允许更高版本的自定义。

但如果导航需要触控滑动，则最好使用 Pivot。

NavigationView 和 Pivot 控件之间的其他主要区别是默认溢出行为和导航 API：

- Pivot 轮转溢出项目，而 NavigationView 使用菜单下拉溢出，以便用户可以查看所有项目。
- Pivot 处理内容部分之间的导航，而 NavigationView 支持对导航行为的更多控制。

## <a name="use-navigationview-instead-of-pivot"></a>使用 NavigationView 而不是 Pivot

如果应用的 UI 使用 Pivot 控件，则可使用以下代码将 Pivot 转换为 NavigationView。

此 XAML 创建包含 3 个部分内容的 NavigationView，如[创建 Pivot 控件](#create-a-pivot-control)中的示例 Pivot。

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

NavigationView 提供了对导航定制的更多控制，并且需要相应的代码隐藏。 若要为上述 XAML 提供补充，请使用以下代码隐藏：

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

此代码模拟 Pivot 控件的内置导航体验，消除了内容部分之间的触控滑动体验。 但是，正如你所看到的，还可自定义多个点，包括动态转换、导航参数和堆叠功能。

## <a name="create-a-pivot-control"></a>创建透视表控件

此代码使用 3 个部分的内容创建基本 Pivot 控件。

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

透视表是一个 [ItemsControl](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsControl)，因此可以包含任何类型的项目集合。 你添加到透视表的任何非显式 [PivotItem](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.PivotItem) 的项目都隐式包装在 PivotItem 中。 由于透视表经常用于在内容页面之间导航，因此通常使用 XAML UI 元素直接填充 [Items](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.items) 集合。 或者，你可以将 [ItemsSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) 属性设置为数据源。 ItemsSource 中绑定的项目可以属于任何类型，但如果它们不是显式 PivotItems，则必须定义 [ItemTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate) 和 [HeaderTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pivot.headertemplate) 来指定这些项目的显示方式。

你可以使用 [SelectedItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pivot.selecteditem) 属性获取或设置透视表的活动项目。 使用 [SelectedIndex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pivot.selectedindex) 属性获取或设置活动项目的索引。

### <a name="pivot-headers"></a>透视表标题

你可以使用 [LeftHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pivot.leftheader) 和 [RightHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pivot.rightheader) 属性将其他控件添加到透视表标题。

例如，可在透视表的 RightHeader 中添加 [CommandBar](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/app-bars)。

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

**固定不动**

- 当所有透视表标题都适合所允许的空间时，透视表将固定不动。
- 点击某个透视表标签即可导航到相应的页面，即使透视表无法自行移动也是如此。 活动透视表将突出显示。

**旋转**

- 当所有透视表标题不适合所允许的空间时，可旋转透视表。
- 点击某个透视表标签即可导航到相应的页面，并且活动透视表标签将旋转至第一个位置。
- 从最后一个到第一个透视表部分的旋转循环中的透视表项目。

> **注意** 透视表标题不应在 [10 英尺环境](../devices/designing-for-tv.md)中旋转。 如果你的应用将在 Xbox 上运行，请将 [IsHeaderItemsCarouselEnabled](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot.IsHeaderItemsCarouselEnabled) 属性设置为“false”  。

## <a name="recommendations"></a>建议

- 当使用旋转（来回旋转）模式时，避免使用 5 个以上的标题，因为超过 5 个标题的循环可能会令人困惑。

## <a name="get-the-sample-code"></a>获取示例代码

- [XAML 控件库示例](https://github.com/Microsoft/Xaml-Controls-Gallery) - 以交互式格式查看所有 XAML 控件。

## <a name="related-topics"></a>相关主题

- [Pivot 类](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot)
- [导航设计基础知识](../basics/navigation-basics.md)
