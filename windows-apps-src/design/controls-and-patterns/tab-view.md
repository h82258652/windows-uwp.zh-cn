---
Description: 可以通过 TabView 在动态选项卡中灵活地组织多个文档
title: 选项卡视图
template: detail.hbs
ms.date: 09/12/2019
ms.topic: article
keywords: windows 10, uwp
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: ce9e3775f4b0f78d17f0ffdf3d6381f2e8a233d9
ms.sourcegitcommit: af4050f69168c15b0afaaa8eea66a5ee38b88fed
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/21/2020
ms.locfileid: "80081526"
---
# <a name="tabview"></a>TabView

可以通过 TabView 控件显示一组选项卡及其相应的内容。 用户可以使用 TabView 来显示多个页面（或文档）的内容，以及重新排列、打开或关闭新选项卡。

![TabView 示例](images/tabview/tab-introduction.png)

**获取 Windows UI 库**

|  |  |
| - | - |
| ![WinUI 徽标](images/winui-logo-64x64.png) | **TabView** 控件作为 Windows UI 库的一部分提供，该库是一个 Nuget 包，包含新控件和 UWP 应用的 UI 功能。 有关详细信息（包括安装说明），请参阅 [Windows UI 库](https://docs.microsoft.com/uwp/toolkits/winui/)。 |

> **Windows UI 库 API**：[TabView 类](/uwp/api/microsoft.ui.xaml.controls.tabview)、[TabViewItem 类](/uwp/api/microsoft.ui.xaml.controls.tabviewitem)

## <a name="is-this-the-right-control"></a>这是正确的控件吗？

通常情况下，选项卡式 UI 有两种不同的样式，这两种样式的功能和外观均不相同：**静态选项卡**是通常在设置窗口中发现的选项卡类型。 它们包含一定数目的页面，这些页面的顺序固定且通常包含预定义的内容。
**文档选项卡**是在浏览器（例如 Microsoft Edge）中发现的选项卡类型。 用户可以创建、删除和重新排列选项卡；在窗口之间移动选项卡；更改选项卡的内容。

[TabView](/uwp/api/microsoft.ui.xaml.controls.tabview) 提供适用于 UWP 应用的文档选项卡。 在以下情况下使用 TabView：

- 用户需要能够动态地打开、关闭或重新排列选项卡。
- 用户需要能够将文档或网页直接打开到选项卡中。
- 用户需要能够在窗口之间拖放选项卡。

如果 TabView 不适合你的应用，考虑使用 [Pivot](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/pivot) 或 [NavigationView](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/navigationview) 之类的控件。

## <a name="anatomy"></a>结构

下图显示 [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview) 控件的部件。 TabStrip 有一个头和尾，但与文档不同，TabStrip 的头和尾分别位于此选项卡条的最左端和最右端。

![TabView 控件的结构](images/tabview/tab-view-anatomy.png)

下图显示 [TabViewItem](/uwp/api/microsoft.ui.xaml.controls.tabviewitem) 控件的部件。 请注意，虽然内容显示在 TabView 控件中，但该内容实际上是 TabViewItem 的一部分。

![TabViewItem 控件的结构](images/tabview/tab-control-anatomy.png)

### <a name="create-a-tab-view"></a>创建选项卡视图

以下示例创建一个简单的 [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview) 和多个支持打开和关闭选项卡的事件处理程序。

```xaml
<TabView AddTabButtonClick="Tabs_AddTabButtonClick"
         TabCloseRequested="Tabs_TabCloseRequested" />
```

```csharp
// Add a new Tab to the TabView
private void Tabs_AddTabButtonClick(TabView sender, TabViewAddTabButtonClickEventArgs e)
{
    var newTab = new TabViewItem();
    newTab.IconSource = new SymbolIconSource() { Symbol = Symbol.Document };
    newTab.Header = "New Document";

    // The Content of a TabViewItem is often a frame which hosts a page.
    Frame frame = new Frame();
    newTab.Content = frame;
    frame.Navigate(typeof(BaconIpsumPage));

    sender.TabItems.Add(newTab);
}

// Remove the requested tab from the TabView
private void Tabs_TabCloseRequested(TabView sender, TabViewTabCloseRequestedEventArgs args)
{
    sender.TabItems.Remove(args.Tab);
}
```

## <a name="behavior"></a>行为

可以通过多种方法来利用或扩展 [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview) 的功能。

### <a name="bind-tabitemssource-to-a-tabviewitemcollection"></a>将 TabItemsSource 绑定到 TabViewItemCollection

```csharp
<TabView TabItemsSource="{x:Bind TabViewItemCollection}" />
```

### <a name="display-tabview-tabs-in-a-windows-titlebar"></a>在窗口的标题栏中显示 TabView 选项卡

可以将选项卡和窗口的标题栏合并到同一区域中，而不是让选项卡在窗口的标题栏下自占一行。 这样可以节省内容的垂直空间，为应用带来现代的观感。

由于用户可以通过窗口的标题栏来拖动窗口，调整窗口的位置，因此不能让标题栏填满选项卡，这一点很重要。 因此，在标题栏中显示选项卡时，必须指定标题栏的一部分作为可拖动区域保留。 如果不指定可拖动区域，则整个标题栏会变得可拖动，这会妨碍选项卡接收输入事件。 如果 TabView 将显示在窗口的标题栏中，则应始终在 [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview) 中包括一个 [TabStripFooter](/uwp/api/microsoft.ui.xaml.controls.tabview.tabstripfooter) 并将其标记为可拖动区域。

有关详细信息，请参阅[标题栏自定义](https://docs.microsoft.com/windows/uwp/design/shell/title-bar)

![标题栏中的选项卡](images/tabview/tab-extend-to-title.png)

```xaml
<Page>
    <TabView HorizontalAlignment="Stretch" VerticalAlignment="Stretch">
        <TabViewItem Icon="Home" Header="Home" IsClosable="False" />
        <TabViewItem Icon="Document" Header="Document 1" />
        <TabViewItem Icon="Document" Header="Document 2" />
        <TabViewItem Icon="Document" Header="Document 3" />

        <TabView.TabStripHeader>
            <Grid x:Name="ShellTitlebarInset" Background="Transparent" />
        </TabView.TabStripHeader>
        <TabView.TabStripFooter>
            <Grid x:Name="CustomDragRegion" Background="Transparent" />
        </TabView.TabStripFooter>
    </TabView>
</Page>
```

```csharp
public MainPage()
{
    this.InitializeComponent();

    var coreTitleBar = CoreApplication.GetCurrentView().TitleBar;
    coreTitleBar.ExtendViewIntoTitleBar = true;
    coreTitleBar.LayoutMetricsChanged += CoreTitleBar_LayoutMetricsChanged;

    Window.Current.SetTitleBar(CustomDragRegion);
}

private void CoreTitleBar_LayoutMetricsChanged(CoreApplicationViewTitleBar sender, object args)
{
    if (FlowDirection == FlowDirection.LeftToRight)
    {
        CustomDragRegion.MinWidth = sender.SystemOverlayRightInset;
        ShellTitlebarInset.MinWidth = sender.SystemOverlayLeftInset;
    }
    else
    {
        CustomDragRegion.MinWidth = sender.SystemOverlayLeftInset;
        ShellTitlebarInset.MinWidth = sender.SystemOverlayRightInset;
    }

    CustomDragRegion.Height = ShellTitlebarInset.Height = sender.Height;
}
```

>[!NOTE]
> 若要确保标题栏中的选项卡不被 shell 内容遮挡，必须考虑到左侧和右侧的重叠问题。 在 LTR 布局中，右侧的嵌入部分包含标题按钮和拖动区域。 在 RTL 布局中，情形相反。 SystemOverlayLeftInset 和 SystemOverlayRightInset 值对应于物理上的左侧和右侧，因此在 RTL 布局中，需将这两个项的顺序反转过来。

### <a name="control-overflow-behavior"></a>控制溢出行为

当选项卡栏充满选项卡时，可以通过设置 [TabView.TabWidthMode](/uwp/api/microsoft.ui.xaml.controls.tabview.tabwidthmode) 来控制选项卡的显示方式。

| TabWidthMode 值 | 行为                                                                                                                                                    |
|--------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 等于              | 添加新选项卡后，所有选项卡都会水平收缩，直到其宽度达到一个很小的最小宽度。                                                       |
| SizeToContent      | 选项卡将始终为其“自然大小”，即，显示其图标和标题所需的最小大小。 添加或关闭选项卡时，它们不会扩展或收缩。 |

不管选择什么值，最终都会出现选项卡过多而无法在选项卡条中显示的情况。 在这种情况下，系统会显示滚动缓冲键，让用户可以左右滚动 TabStrip。

### <a name="guidance-for-tab-selection"></a>选项卡选择指南

大多数用户只需使用 Web 浏览器即可获得文档选项卡的使用体验。 他们在你的应用中使用文档选项卡时，会根据自己的体验预期你的选项卡的具体行为。

不管用户以何种方式与一组文档选项卡交互，始终应有一个活动选项卡。如果用户关闭所选选项卡或将所选选项卡移到另一窗口中，则另一选项卡应成为活动选项卡。为此，[TabView](/uwp/api/microsoft.ui.xaml.controls.tabview) 会尝试自动选择下一选项卡。如果你觉得应用应该允许 TabView 的选项卡处于未选中状态，则应让 TabView 的内容区域直接显示为空白。

## <a name="keyboard-navigation"></a>键盘导航

默认情况下，[TabView](/uwp/api/microsoft.ui.xaml.controls.tabview) 支持许多常见的键盘导航方案。 此部分介绍内置功能，并提供可能对某些应用有用的其他功能的建议。

### <a name="tab-and-cursor-key-behavior"></a>Tab 键和光标键的行为

当焦点移到 _TabStrip_ 区域时，所选 [TabViewItem](/uwp/api/microsoft.ui.xaml.controls.tabviewitem) 获得焦点。 然后，用户可以使用向左和向右箭头键将焦点（不是选择的内容）移到 TabStrip 中的其他选项卡。 箭头焦点限制在选项卡条和“添加选项卡(+)”按钮（如果存在）中。 若要将焦点移出 TabStrip 区域，用户可以按 Tab 键，将焦点移到下一个可聚焦元素。

通过 Tab 键移动焦点

![通过 Tab 键移动焦点](images/tabview/tab-keyboard-behavior-1.png)

箭头键不循环访问焦点

![箭头键不循环访问焦点](images/tabview/tab-keyboard-behavior-3.png)

### <a name="selecting-a-tab"></a>选择选项卡

当某个 TabViewItem 获得焦点以后，按空格键或 Enter 键即可选择该 TabViewItem。

使用箭头键移动焦点，然后按空格键选择选项卡。

![按空格键选择选项卡](images/tabview/tab-keyboard-behavior-2.png)

### <a name="shortcuts-for-selecting-adjacent-tabs"></a>选择相邻选项卡的快捷方式

按 Ctrl+Tab 会选择下一 [TabViewItem](/uwp/api/microsoft.ui.xaml.controls.tabviewitem)。 按 Ctrl+Shift+Tab 会选择上一 TabViewItem。 进行此类用途的操作时，选项卡列表处于“循环”状态。因此，如果在最后一个选项卡处于选中状态的情况下选择下一个选项卡，会导致第一个选项卡变为选中状态。

### <a name="closing-a-tab"></a>关闭选项卡

按 Ctrl + F4 会引发 [TabCloseRequested](/uwp/api/microsoft.ui.xaml.controls.tabview.tabcloserequested) 事件。 请根据需要处理该事件并关闭选项卡。

### <a name="keyboard-guidance-for-app-developers"></a>应用开发人员键盘指南

某些应用程序可能需要更高级的键盘控制。 考虑根据应用的具体情况实现以下快捷方式。

> [!WARNING]
> 如果将 [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview) 添加到现有应用，你可能会发现，你已经创建了键盘快捷方式，这些快捷方式映射到建议的 TabView 键盘快捷方式的组合键。 在这种情况下，必须考虑是保留现有的快捷方式，还是为用户提供直观的选项卡体验。

- Ctrl + T 会打开新的选项卡。通常情况下，该选项卡填充了预定义的文档，或者在创建时是空的，但可以通过简单的方式选择其内容。 如果用户必须为新选项卡选择内容，考虑为内容选择控件提供输入焦点。
- Ctrl + W 会关闭所选选项卡。记住，TabView 会自动选择下一选项卡。
- Ctrl + Shift + T 会打开最近关闭的选项卡（更准确地说，是会打开内容与最近关闭的选项卡相同的新选项卡）。 可以从最近关闭的选项卡开始，以后溯的方式针对每个后续时间调用此快捷方式。 请注意，这需要保留一个列表，其中包含最近关闭的选项卡。
- Ctrl + 1 会选择选项卡列表中的第一个选项卡。 类似地，Ctrl + 2 会选择第二个选项卡，Ctrl + 3 会选择第三个选项卡，依此类推，Ctrl + 8 会选择第八个选项卡。
- Ctrl + 9 会选择选项卡列表中的最后一个选项卡，不管列表中有多少个选项卡。
- 如果除了关闭命令，选项卡还提供其他命令（例如用于复制或固定选项卡的命令），请使用上下文菜单显示所有能够在选项卡上执行的可用操作。

### <a name="implement-browser-style-keyboarding-behavior"></a>实现浏览器样式的键盘操作行为

以下示例在 [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview) 上实现了上述许多建议。 具体说来，此示例实现了 Ctrl + T、Ctrl + W、Ctrl + 1-8 和 Ctrl + 9。

```xaml
<controls:TabView x:Name="TabRoot">
    <controls:TabView.KeyboardAccelerators>
        <KeyboardAccelerator Key="T" Modifiers="Control" Invoked="NewTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="W" Modifiers="Control" Invoked="CloseSelectedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number1" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number2" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number3" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number4" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number5" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number6" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number7" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number8" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number9" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
    </controls:TabView.KeyboardAccelerators>
    <!-- ... some tabs ... -->
</controls:TabView>
```

```csharp
private void NewTabKeyboardAccelerator_Invoked(KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
    // See previous sample
    CreateNewTab();
}

private void CloseSelectedTabKeyboardAccelerator_Invoked(KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
    // Only close the selected tab if it is closeable
    if (((TabViewItem)TabRoot.SelectedItem).IsCloseable)
    {
        TabRoot.TabItems.Remove(TabRoot.SelectedItem);
    }
}

private void NavigateToNumberedTabKeyboardAccelerator_Invoked(KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
    int tabToSelect = 0;

    switch (sender.Key)
    {
        case Windows.System.VirtualKey.Number1:
            tabToSelect = 0;
            break;
        case Windows.System.VirtualKey.Number2:
            tabToSelect = 1;
            break;
        case Windows.System.VirtualKey.Number3:
            tabToSelect = 2;
            break;
        case Windows.System.VirtualKey.Number4:
            tabToSelect = 3;
            break;
        case Windows.System.VirtualKey.Number5:
            tabToSelect = 4;
            break;
        case Windows.System.VirtualKey.Number6:
            tabToSelect = 5;
            break;
        case Windows.System.VirtualKey.Number7:
            tabToSelect = 6;
            break;
        case Windows.System.VirtualKey.Number8:
            tabToSelect = 7;
            break;
        case Windows.System.VirtualKey.Number9:
            // Select the last tab
            tabToSelect = TabRoot.TabItems.Count - 1;
            break;
    }

    // Only select the tab if it is in the list
    if (tabToSelect < TabRoot.TabItems.Count)
    {
        TabRoot.SelectedIndex = tabToSelect;
    }
}
```

## <a name="related-articles"></a>相关文章

- [MasterDetails](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/master-details)
- [NavigationView](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/navigationview)
- [透视表](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/pivot)
