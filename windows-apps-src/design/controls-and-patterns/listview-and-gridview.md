---
author: muhsinking
Description: Use ListView and GridView controls to display and manipulate sets of data, such as a gallery of images or a set of email messages.
title: 列表视图和网格视图
label: List view and grid view
template: detail.hbs
ms.author: jimwalk
ms.date: 05/20/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: f8532ba0-5510-4686-9fcf-87fd7c643e7b
pm-contact: predavid
design-contact: kimsea
dev-contact: ranjeshj
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 1ee00a9af23be945ad27ab4b39eec127ec397894
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/01/2018
ms.locfileid: "5934118"
---
# <a name="list-view-and-grid-view"></a>列表视图和网格视图

大多数应用程序都会操作和显示数据集，例如图像库或一组电子邮件。 XAML UI 框架提供了轻松显示和操控应用数据的 ListView 和 GridView 控件。  

> **重要 API**：[ListView 类](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx)，[GridView 类](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridview.aspx)，[ItemsSource 属性](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemssource.aspx)，[Items 属性](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.items.aspx)

ListView 和 GridView 都从 ListViewBase 类派生，因此它们的功能相同，但数据显示方法不同。 在本文中，当谈论 ListView 时，信息都适用于 ListView 和 GridView 控件，除非另行指定。 我们可能会引用 ListView 或 ListViewItem 等类，但“List”前缀可使用相应网格等效项（GridView 或 GridViewItem）的“Grid”代替。 

## <a name="is-this-the-right-control"></a>这是正确的控件吗？

ListView 采用垂直堆叠的方式在单个列中显示数据。 该控件常用于显示按顺序排列的项目列表，如电子邮件列表或搜索结果列表。 

![具有分组数据的列表视图](images/simple-list-view-phone.png)

GridView 显示可在行和列中垂直滚动的项目集合。 数据水平堆叠，直到填满列，然后继续执行下一行。 对于占驻较多控件的每个项目（如照片库），当你需要为其显示丰富的视觉信息时，该控件很常用。 

![内容库示例](images/controls_list_contentlibrary.png)

有关更详细的比较和使用哪个控件的指南，请参阅[列表](lists.md)。

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果已安装 <strong style="font-weight: semi-bold">XAML 控件库</strong>应用，请单击此处打开该应用，了解 <a href="xamlcontrolsgallery:/item/ListView">ListView</a> 或 <a href="xamlcontrolsgallery:/item/GridView">GridView</a> 的实际应用。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-list-view"></a>创建列表视图

列表视图是一个 [ItemsControl](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.aspx)，因此可以包含任何类型的项目集合。 在能够在屏幕上显示任何内容前，它必须在自己的 [Items](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.items.aspx) 集合中有项目。 若要填充视图，可以将项目直接添加到 [Items](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.items.aspx) 集合，或者将 [ItemsSource](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemssource.aspx) 属性设置为数据源。 

**重要提示**&nbsp;&nbsp;可以使用 Items 或 ItemsSource 填充列表，但无法同时使用这两者。 如果你设置 ItemsSource 属性并使用 XAML 添加项目，将忽略添加的项目。 如果你设置 ItemsSource 属性并使用代码向 Items 集合中添加项目，将引发异常。

> **注意**&nbsp;&nbsp;为方便起见，本文中的许多示例直接填充了 **Items** 集合。 但是，列表中的项目来自于动态源的情况更常见，例如书籍列表来自于在线数据库。 出于此目的，你使用 **ItemsSource** 属性。 

### <a name="add-items-to-the-items-collection"></a>将项添加到项集合

可以通过使用 XAML 或代码向 [Items](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.items.aspx) 集合添加项。 在以下情况下通常采用这种方式添加项：具有不更改且使用 XAML 轻松定义的少量项，或者在运行时采用代码生成项。 

以下是一个列表视图，内含在 XAML 中以内联方式定义的项目。 使用 XAML 定义项时，这些项会自动添加到项集合。

**XAML**
```xaml
<ListView x:Name="listView1"> 
   <x:String>Item 1</x:String> 
   <x:String>Item 2</x:String> 
   <x:String>Item 3</x:String> 
   <x:String>Item 4</x:String> 
   <x:String>Item 5</x:String> 
</ListView>  
```

以下是在代码中创建的列表视图。 生成的列表与之前在 XAML 中创建的列表相同。

**C#**
```csharp
// Create a new ListView and add content. 
ListView listView1 = new ListView(); 
listView1.Items.Add("Item 1"); 
listView1.Items.Add("Item 2"); 
listView1.Items.Add("Item 3"); 
listView1.Items.Add("Item 4"); 
listView1.Items.Add("Item 5");
 
// Add the ListView to a parent container in the visual tree. 
stackPanel1.Children.Add(listView1); 
```

ListView 如下所示。

![简单的列表视图](images/listview-simple.png)

### <a name="set-the-items-source"></a>设置项目源

通常使用列表视图显示源（例如数据库或 Internet）中的数据。 若要填充数据源中的列表视图，请将其 [ItemsSource](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemssource.aspx) 属性设置为数据项集合。

此时，直接在代码中将列表视图的 ItemsSource 设置为集合实例。

**C#**
```csharp 
// Instead of hard coded items, the data could be pulled 
// asynchronously from a database or the internet.
ObservableCollection<string> listItems = new ObservableCollection<string>();
listItems.Add("Item 1");
listItems.Add("Item 2");
listItems.Add("Item 3");
listItems.Add("Item 4");
listItems.Add("Item 5");

// Create a new list view, add content, 
ListView itemListView = new ListView();
itemListView.ItemsSource = listItems;

// Add the list view to a parent container in the visual tree.
stackPanel1.Children.Add(itemListView);
```

还可以将 ItemsSource 属性绑定到 XAML 中的集合。 有关数据绑定的详细信息，请参阅[数据绑定概述](https://msdn.microsoft.com/windows/uwp/data-binding/data-binding-quickstart)。

在此处，ItemsSource 绑定到名为 `Items` 并且公开 Page 的专用数据集合的公共属性。

**XAML**
```xaml
<ListView x:Name="itemListView" ItemsSource="{x:Bind Items}"/>
```

**C#**
```csharp
private ObservableCollection<string> _items = new ObservableCollection<string>();

public ObservableCollection<string> Items
{
    get { return this._items; }
}

protected override void OnNavigatedTo(NavigationEventArgs e)
{
    base.OnNavigatedTo(e);

    // Instead of hard coded items, the data could be pulled 
    // asynchronously from a database or the internet.
    Items.Add("Item 1");
    Items.Add("Item 2");
    Items.Add("Item 3");
    Items.Add("Item 4");
    Items.Add("Item 5");
}
```

如果你需要在列表视图中显示分组数据，必须绑定到 [CollectionViewSource](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.data.collectionviewsource.aspx)。 CollectionViewSource 在 XAML 中充当集合类的代理角色，并启用分组支持。 有关详细信息，请参阅 [CollectionViewSource](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.data.collectionviewsource.aspx)。

## <a name="data-template"></a>数据模板

项的模板定义数据可视化的方式。 在默认情况下，数据项以绑定到的数据对象的字符串表现形式显示在列表视图中。 通过将 [DisplayMemberPath](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.displaymemberpath.aspx) 设置到特定的属性，你可以显示数据项的该属性的字符串表现形式。

但是，你通常会希望更丰富地呈现你的数据。 若要具体地指定列表视图中项的显示方式，可以创建 [DataTemplate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.datatemplate.aspx)。 DataTemplate 中的 XAML 定义用于显示各项的控件的布局和外观。 该布局中的控件可绑定到数据对象的属性，或者具有在内联中定义的静态内容。 将 DataTemplate 分配给列表控件的 [ItemTemplate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx) 属性。

在此示例中，数据项是简单的字符串。 使用 DataTemplate 将图像添加到字符串左侧，并用青色显示该字符串。

> **注意**&nbsp;&nbsp;当在 DataTemplate 中使用 [x:Bind 标记扩展](https://msdn.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)时，必须在 DataTemplate 上指定 DataType (`x:DataType`)。

**XAML**
```XAML
<ListView x:Name="listView1">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="x:String">
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="47"/>
                    <ColumnDefinition/>
                </Grid.ColumnDefinitions>
                <Image Source="Assets/placeholder.png" Width="32" Height="32" 
                       HorizontalAlignment="Left"/>
                <TextBlock Text="{x:Bind}" Foreground="Teal" 
                           FontSize="15" Grid.Column="1"/>
            </Grid> 
        </DataTemplate>
    </ListView.ItemTemplate>
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
    <x:String>Item 3</x:String>
    <x:String>Item 4</x:String>
    <x:String>Item 5</x:String>
</ListView>
```

以下是通过此数据模板显示时数据项的外观。

![使用数据模板的列表视图项目](images/listview-itemstemplate.png)

使用数据模板是定义列表视图外观的主要方法。 如果列表显示大量项目，它们也能对性能产生重大影响。 在本文中，我们为大多数示例使用简单的字符串数据，并且未指定数据模板。 有关如何使用数据模板和项目容器定义列表或网格中的项目的外观的更多信息和示例，请参阅[项目容器和模板](item-containers-templates.md)。 

## <a name="change-the-layout-of-items"></a>更改项目的布局

当你将项目添加到列表视图或网格视图时，控件会使每个项目在项目容器中自动换行，然后设置所有项目容器的布局。 这些项目容器的布局方式取决于控件的 [ItemsPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemspanel.aspx)。  
- 默认情况下，**ListView** 使用 [ItemsStackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemsstackpanel.aspx)，这可以生成垂直列表，如下所示。

![简单的列表视图](images/listview-simple.png)

- **GridView** 使用 [ItemsWrapGrid](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemswrapgrid.aspx)，这会水平添加项目，并且垂直换行和滚动，如下所示。

![简单的网格视图](images/gridview-simple.png)

你可以通过在项目面板上调整属性来修改项目布局，或者可以将默认面板替换为其他面板。

> 注意&nbsp;&nbsp; 如果更改 ItemsPanel，注意不要禁用虚拟化。 **ItemsStackPanel** 和 **ItemsWrapGrid** 均支持虚拟化，所以可以安全使用它们。 如果你使用任何其他面板，可能会禁用虚拟化，并且降低列表视图的性能。 有关详细信息，请参阅[性能](https://msdn.microsoft.com/windows/uwp/debug-test-perf/performance-and-xaml-ui)下的列表视图文章。 

此示例显示如何通过更改 **ItemsStackPanel** 的 [Orientation](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemsstackpanel.orientation.aspx) 属性来使 **ListView** 在水平列表中设置项目容器的布局。
因为默认情况下列表视图垂直滚动，所以你还需要在列表视图的内部 [ScrollViewer](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.aspx) 上调整某些属性以使其可以水平滚动。
- [ScrollViewer.HorizontalScrollMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.horizontalscrollmode.aspx) 设置为 **Enabled** 或 **Auto**
- [ScrollViewer.HorizontalScrollBarVisibility](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.horizontalscrollbarvisibility.aspx) 设置为 **Auto** 
- [ScrollViewer.VerticalScrollMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.verticalscrollmode.aspx) 设置为 **Disabled** 
- [ScrollViewer.VerticalScrollBarVisibility](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.verticalscrollbarvisibility.aspx) 设置为 **Hidden** 

> **注意**&nbsp;&nbsp;显示这些示例时，列表视图宽度不受约束，因此不会显示水平滚动条。 如果你运行此代码，可以在 ListView 上设置 `Width="180"` 来使滚动条显示。

**XAML**
```xaml
<ListView Height="60" 
          ScrollViewer.HorizontalScrollMode="Enabled" 
          ScrollViewer.HorizontalScrollBarVisibility="Auto"
          ScrollViewer.VerticalScrollMode="Disabled"
          ScrollViewer.VerticalScrollBarVisibility="Hidden">
    <ListView.ItemsPanel>
        <ItemsPanelTemplate>
            <ItemsStackPanel Orientation="Horizontal"/>
        </ItemsPanelTemplate>
    </ListView.ItemsPanel>
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
    <x:String>Item 3</x:String>
    <x:String>Item 4</x:String>
    <x:String>Item 5</x:String>
</ListView>
```

生成的列表如下所示。

![水平列表视图](images/listview-horizontal.png)

 在下一个示例中，通过使用 **ItemsWrapGrid** 而非 **ItemsStackPanel**，**ListView** 在垂直换行列表中设置项目的布局。 
 
> **注意**&nbsp;&nbsp;列表视图的高度必须受限，以强制控件使容器换行。

**XAML**
```xaml
<ListView Height="100"
          ScrollViewer.HorizontalScrollMode="Enabled" 
          ScrollViewer.HorizontalScrollBarVisibility="Auto"
          ScrollViewer.VerticalScrollMode="Disabled"
          ScrollViewer.VerticalScrollBarVisibility="Hidden">
    <ListView.ItemsPanel>
        <ItemsPanelTemplate>
            <ItemsWrapGrid/>
        </ItemsPanelTemplate>
    </ListView.ItemsPanel>
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
    <x:String>Item 3</x:String>
    <x:String>Item 4</x:String>
    <x:String>Item 5</x:String>
</ListView>
```

生成的列表如下所示。

![具有网格布局的列表视图](images/listview-itemswrapgrid.png)

如果你在列表视图中显示分组数据，ItemsPanel 将确定项目组的布局方式，而非单独项目的布局方式。例如，如果之前显示的水平 ItemsStackPanel 用于显示分组数据，则各组按水平方式排列，但每组中的项目仍垂直堆叠，如下所示。

![已分组的水平列表视图](images/listview-horizontal-groups.png)

## <a name="item-selection-and-interaction"></a>项目选择和交互

你可以选择多种方法来使用户与列表视图交互。 默认情况下，用户可选择一个项目。 你可以更改 [SelectionMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.selectionmode.aspx) 属性以启用多选或禁用选择。 你可以设置 [IsItemClickEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.isitemclickenabled.aspx) 属性，以便用户单击某个项目即可调用操作（例如按钮），而不是选择该项目。

> **注意**&nbsp;&nbsp;ListView 和 GridView 均将 [ListViewSelectionMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewselectionmode.aspx) 枚举用于其 SelectionMode 属性。 默认情况下，IsItemClickEnabled 为 **False**，因此你需要仅将其设置为启用单击模式。

此表显示用户可与列表视图交互的方式以及响应交互的方式。

若要启用此交互： | 使用这些设置： | 处理此事件： | 使用此属性以获取选定的项目：
----------------------------|---------------------|--------------------|--------------------------------------------
无交互 | [SelectionMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.selectionmode.aspx) = **None**、[IsItemClickEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.isitemclickenabled.aspx) = **False** | 不适用 | 不适用 
单选 | SelectionMode = **Single**、IsItemClickEnabled = **False** | [SelectionChanged](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.selector.selectionchanged.aspx) | [SelectedItem](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.selector.selecteditem.aspx)、[SelectedIndex](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.selector.selectedindex.aspx)  
多选 | SelectionMode = **Multiple**、IsItemClickEnabled = **False** | [SelectionChanged](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.selector.selectionchanged.aspx) | [SelectedItems](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.selecteditems.aspx)  
扩展选择 | SelectionMode = **Extended**、IsItemClickEnabled = **False** | [SelectionChanged](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.selector.selectionchanged.aspx) | [SelectedItems](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.selecteditems.aspx)  
单击 | SelectionMode = **None**、IsItemClickEnabled = **True** | [ItemClick](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.itemclick.aspx) | 不适用 

> **注意**&nbsp;&nbsp;从 Windows 10 开始，你可以启用 IsItemClickEnabled 以引发 ItemClick 事件，同时 SelectionMode 也设置为 Single、Multiple 或 Extended。 如果你执行此操作，将先后引发 ItemClick 事件和 SelectionChanged 事件。 在某些情况下（例如在 ItemClick 事件处理程序中导航到其他页面），不会引发 SelectionChanged 事件，并且不会选择该项目。

可以采用 XAML 或代码设置这些属性，如下所示。

**XAML**
```xaml
<ListView x:Name="myListView" SelectionMode="Multiple"/>

<GridView x:Name="myGridView" SelectionMode="None" IsItemClickEnabled="True"/> 
```

**C#**
```csharp
myListView.SelectionMode = ListViewSelectionMode.Multiple; 

myGridView.SelectionMode = ListViewSelectionMode.None;
myGridView.IsItemClickEnabled = true;
```

### <a name="read-only"></a>只读

你可以将 SelectionMode 属性设置为 **ListViewSelectionMode.None** 以禁用项目选择。 这会将控件置于只读模式下、使之用于显示数据，但不是为了与之交互。 控件本身不会被禁用，仅禁用项目选择。

### <a name="single-selection"></a>单选

下表介绍在 SelectionMode 设置为 **Single** 时，键盘、鼠标和触摸的交互情况。

修改键 | 交互
-------------|------------
无 | <li>用户可以使用空格键、鼠标单击或触摸点击来选择单个项。</li>
Ctrl | <li>用户可以使用空格键、鼠标单击或触摸点击来取消选择单个项。</li><li>通过使用箭头键，用户可以独立于选择来移动焦点。</li>

当 SelectionMode 设置为 **Single** 时，可以从 [SelectedItem](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.selector.selecteditem.aspx) 属性获取选定的数据项。 你可以使用 [SelectedIndex](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.selector.selectedindex.aspx) 属性获取选定项目集合中的索引。 如果没有选择任何项目，则 SelectedItem 为 **null**，并且 SelectedIndex 为 -1。 
 
如果你尝试设置像 **SelectedItem** 那样不在 **Items** 集合中的项目，则该操作将被忽略，并且 SelectedItem 为 **null**。 但是，如果你尝试在列表中将 **SelectedIndex** 设置为超出 **Items** 范围的索引，将会发生 **System.ArgumentException** 异常。 

### <a name="multiple-selection"></a>多选

下表介绍在 SelectionMode 设置为 **Multiple** 时，键盘、鼠标和触摸的交互情况。

修改键 | 交互
-------------|------------
无 | <li>用户可以使用空格键、鼠标单击或触摸点击来选择多个项目，以在聚焦项目上切换选择。</li><li>通过使用箭头键，用户可以独立于选择来移动焦点。</li>
Shift | <li>用户可以通过先后单击或点击选择中的第一个和最后一个项目来选择多个连续项目。</li><li>通过使用箭头键，用户可以创建从在按下 Shift 时选择的项目开始的连续选择。</li>

### <a name="extended-selection"></a>扩展选择

下表介绍在 SelectionMode 设置为 **Extended** 时，键盘、鼠标和触摸的交互情况。

修改键 | 交互
-------------|------------
无 | <li>该行为与 **Single** 选择相同。</li>
Ctrl | <li>用户可以使用空格键、鼠标单击或触摸点击来选择多个项目，以在聚焦项目上切换选择。</li><li>通过使用箭头键，用户可以独立于选择来移动焦点。</li>
Shift | <li>用户可以通过先后单击或点击选择中的第一个和最后一个项目来选择多个连续项目。</li><li>通过使用箭头键，用户可以创建从在按下 Shift 时选择的项目开始的连续选择。</li>

当 SelectionMode 设置为 **Multiple** 或 **Extended** 时，可以从 [SelectedItems](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.selecteditems.aspx) 属性获取选定的数据项。 

**SelectedIndex**、**SelectedItem** 和 **SelectedItems** 属性已同步。 例如，如果你将 SelectedIndex 设置为 -1、将 SelectedItem 设置为 **null** 并且将 SelectedItems 设置为空；如果你将 SelectedItem 设置为 **null**、将 SelectedIndex 设置为 -1 并且将 SelectedItems 设置为空。

在多选模式下，**SelectedItem** 包含第一个选择的项目，而 **Selectedindex** 包含第一个选择的项目的索引。 

### <a name="respond-to-selection-changes"></a>响应选择更改

若要响应列表视图中的选择更改，请处理 [SelectionChanged](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.selector.selectionchanged.aspx) 事件。 在事件处理程序代码中，可以从 [SelectionChangedEventArgs.AddedItems](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.selectionchangedeventargs.addeditems.aspx) 属性获取选择项列表。 你可以获取从 [SelectionChangedEventArgs.RemovedItems](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.selectionchangedeventargs.removeditems.aspx) 属性取消选择的任何项目。 除非用户通过按住 Shift 键选择项目范围，否则 AddedItems 和 RemovedItems 集合将最多包含 1 个项目。

此示例显示了如何处理 **SelectionChanged** 事件和访问各种项目集合。

**XAML**
```xaml
<StackPanel HorizontalAlignment="Right">
    <ListView x:Name="listView1" SelectionMode="Multiple" 
              SelectionChanged="ListView1_SelectionChanged">
        <x:String>Item 1</x:String>
        <x:String>Item 2</x:String>
        <x:String>Item 3</x:String>
        <x:String>Item 4</x:String>
        <x:String>Item 5</x:String>
    </ListView>
    <TextBlock x:Name="selectedItem"/>
    <TextBlock x:Name="selectedIndex"/>
    <TextBlock x:Name="selectedItemCount"/>
    <TextBlock x:Name="addedItems"/>
    <TextBlock x:Name="removedItems"/>
</StackPanel> 
```

**C#**
```csharp
private void ListView1_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (listView1.SelectedItem != null)
    {
        selectedItem.Text = 
            "Selected item: " + listView1.SelectedItem.ToString();
    }
    else
    {
        selectedItem.Text = 
            "Selected item: null";
    }
    selectedIndex.Text = 
        "Selected index: " + listView1.SelectedIndex.ToString();
    selectedItemCount.Text = 
        "Items selected: " + listView1.SelectedItems.Count.ToString();
    addedItems.Text = 
        "Added: " + e.AddedItems.Count.ToString();
    removedItems.Text = 
        "Removed: " + e.RemovedItems.Count.ToString();
}
```

### <a name="click-mode"></a>单击模式

你可以更改列表视图，从而使用户单击项目（如按钮），而不是选择项目。 例如，当用户点击列表或网格中的一个项目时，如果你的应用导航至一个新页面，这将会很有用。 若要启用此行为：
- 将 **SelectionMode** 设置为 **None**。
- 将 **IsItemClickEnabled** 设置为 **true**。
- 在用户单击某个项目时，处理 **ItemClick** 事件以执行某些操作。

下面是具有可单击项的列表视图。 ItemClick 事件处理程序中的代码会导航到新的页面。

**XAML**
```xaml
<ListView SelectionMode="None"
          IsItemClickEnabled="True" 
          ItemClick="ListView1_ItemClick">
    <x:String>Page 1</x:String>
    <x:String>Page 2</x:String>
    <x:String>Page 3</x:String>
    <x:String>Page 4</x:String>
    <x:String>Page 5</x:String>
</ListView>
```

**C#**
```csharp
private void ListView1_ItemClick(object sender, ItemClickEventArgs e)
{
    switch (e.ClickedItem.ToString())
    {
        case "Page 1":
            this.Frame.Navigate(typeof(Page1));
            break;

        case "Page 2":
            this.Frame.Navigate(typeof(Page2));
            break;

        case "Page 3":
            this.Frame.Navigate(typeof(Page3));
            break;

        case "Page 4":
            this.Frame.Navigate(typeof(Page4));
            break;

        case "Page 5":
            this.Frame.Navigate(typeof(Page5));
            break;

        default:
            break;
    }
}
```

### <a name="select-a-range-of-items-programmatically"></a>以编程方式选择项目的范围

有时你需要以编程方式来操控列表视图的项目选择。 例如，你可能拥有**全选**按钮来让用户选择列表中的所有项目。 在这种情况下，从 SelectedItems 集合逐个添加或删除项目通常效率不高。 每个项目更改都会导致发生 SelectionChanged 事件，并且当你直接处理项目而非索引值时，该项目会取消虚拟化。

[SelectAll](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.selectall.aspx)、[SelectRange](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.selectrange.aspx) 和 [DeselectRange](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.deselectrange.aspx) 方法提供比使用 SelectedItems 属性更高效的修改选择的方法。 这些方法使用项目索引范围进行选择或取消选择。 虚拟化的项目将保持虚拟化状态，因为仅使用了索引。 指定范围中的所有项目均已选定（或已取消选定），无论初始选择状态是什么。 SelectionChanged 事件在每次调用这些方法时仅发生一次。

> **重要提示**&nbsp;&nbsp;仅当 SelectionMode 属性设置为 Multiple 或 Extended 时才应调用这些方法。 如果在 SelectionMode 是 Single 或 None 时调用 SelectRange，将引发异常。

当使用索引范围选择项目时，请使用 [SelectedRanges](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.selectedranges.aspx) 属性获取列表中的所有选定范围。

如果 ItemsSource 实现了 [IItemsRangeInfo](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.iitemsrangeinfo.aspx)，并且你使用这些方法修改选择，则 **AddedItems** 和 **RemovedItems** 属性将不会在 SelectionChangedEventArgs 中进行设置。 设置这些属性需要对项目对象执行取消虚拟化操作。 改为使用 **SelectedRanges** 属性获取项目。

通过调用 SelectAll 方法，可以选择集合中的所有项目。 但是没有相应的方法来取消选择所有项目。 你可以通过以下方法取消选择所有项目：调用 DeselectRange，并传递 [FirstIndex](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.itemindexrange.aspx) 值为 0 并且 [Length](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.itemindexrange.firstindex.aspx) 值等于集合中项目数的 [ItemIndexRange](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.itemindexrange.length.aspx)。 

**XAML**
```xaml
<StackPanel Width="160">
    <Button Content="Select all" Click="SelectAllButton_Click"/>
    <Button Content="Deselect all" Click="DeselectAllButton_Click"/>
    <ListView x:Name="listView1" SelectionMode="Multiple">
        <x:String>Item 1</x:String>
        <x:String>Item 2</x:String>
        <x:String>Item 3</x:String>
        <x:String>Item 4</x:String>
        <x:String>Item 5</x:String>
    </ListView>
</StackPanel>
```

**C#**
```csharp
private void SelectAllButton_Click(object sender, RoutedEventArgs e)
{
    if (listView1.SelectionMode == ListViewSelectionMode.Multiple ||
        listView1.SelectionMode == ListViewSelectionMode.Extended)
    {
        listView1.SelectAll();
    }
}

private void DeselectAllButton_Click(object sender, RoutedEventArgs e)
{
    if (listView1.SelectionMode == ListViewSelectionMode.Multiple ||
        listView1.SelectionMode == ListViewSelectionMode.Extended)
    {
        listView1.DeselectRange(new ItemIndexRange(0, (uint)listView1.Items.Count));
    }
}
```

有关如何更改所选项目外观的信息，请参阅[项目容器和模板](item-containers-templates.md)。

### <a name="drag-and-drop"></a>拖放

ListView 和 GridView 控件支持在其自身内部以及它们自身与其他 ListView 和 GridView 控件之间拖放项目。 有关实现拖放模式的详细信息，请参阅[拖放](https://msdn.microsoft.com/windows/uwp/design/input/drag-and-drop)。 

## <a name="get-the-sample-code"></a>获取示例代码

- [XAML ListView 和 GridView 示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlListView) - 演示 ListView 和 GridView 控件。
- [XAML 拖放示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlDragAndDrop) - 演示使用 ListView 控件进行拖放。
- [XAML 控件库示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 以交互式格式查看所有 XAML 控件。

## <a name="related-articles"></a>相关文章

- [列表](lists.md)
- [项目容器和模板](item-containers-templates.md)
- [拖放](https://msdn.microsoft.com/windows/uwp/app-to-app/drag-and-drop)
