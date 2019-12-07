---
Description: 使用 ListView 和 GridView 控件显示和操控数据组，例如图像库或一组电子邮件。
title: 列表视图和网格视图
label: List view and grid view
template: detail.hbs
ms.date: 11/04/2019
ms.topic: article
keywords: windows 10, uwp
ms.assetid: f8532ba0-5510-4686-9fcf-87fd7c643e7b
pm-contact: predavid
design-contact: kimsea
dev-contact: ranjeshj
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: e95a9a1f6a0d34e377f48c5b19497eb638fb186e
ms.sourcegitcommit: 27cb7c4539bb6417d32883824ccea160bb948c15
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2019
ms.locfileid: "74830821"
---
# <a name="list-view-and-grid-view"></a>列表视图和网格视图

大多数应用都会操纵和显示数据集，例如图像库或一组电子邮件。 XAML UI 框架提供了轻松显示和操控应用数据的 ListView 和 GridView 控件。  

> **重要的 API**：[ListView 类](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview)、[GridView 类](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.gridview)、[ItemsSource 属性](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource)、[Items 属性](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.items)

> [!NOTE]
> ListView 和 GridView 都从 [ListViewBase](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase) 类派生，因此它们的功能相同，但数据显示方式不同。 在本文中，涉及到列表视图时，信息适用于 ListView 和 GridView 控件，除非另行指定。 我们可能会引用 ListView 或 ListViewItem 等类，但 *List* 前缀可使用相应网格等效项（GridView 或 GridViewItem）的 *Grid* 代替。 

ListView 和 GridView 提供使用集合的许多好处。 它们都易于实施且提供基本 UI、交互和滚动，同时仍可以轻松自定义。 ListView 和 GridView 可以绑定到现有的动态数据源，也可以绑定到 XAML 本身/代码隐藏中提供的硬编码数据。 

这两种控件可以灵活应用于许多用例，但总体来说最适合所有项必须具有相同基本结构和外观以及相同交互行为的集合 - 即单击时它们都应执行相同操作（打开链接、导航等）。


## <a name="differences-between-listview-and-gridview"></a>ListView 和 GridView 之间的差异

### <a name="listview"></a>ListView
ListView 采用垂直堆叠的方式在单个列中显示数据。 ListView 更适用于将文本作为焦点的项以及应从上到下读取的集合（即按字母顺序排列）。 ListView 的几个常见用例包括消息列表和搜索结果。

![具有分组数据的列表视图](images/listview-grouped-example-resized-final.png)

### <a name="gridview"></a>GridView
GridView 显示可在行和列中垂直滚动的项目集合。 数据水平堆叠，直到填满列，然后继续执行下一行。 GridView 更适用于将图像作为焦点的项，以及可从一侧到另一侧读取或未按特定顺序进行排序的集合。 GridView 的一个常见用例是照片或产品库。

![内容库示例](images/gridview-simple-example-final.png)

## <a name="which-collection-control-should-you-use-a-comparison-with-itemsrepeater"></a>应使用哪个集合控件？ 与 ItemsRepeater 的比较

ListView 和 GridView 是显示任何集合的现成控件，具有自己的内置 UI 和用户体验。 [ItemsRepeater](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/items-repeater) 控件也用于显示集合，但作为构建基块创建，用于创建符合你的确切 UI 需要的自定义控件。 下面列出了应影响你最终使用的控件的最重要的差异：

-   ListView 和 GridView 是功能丰富的控件，只需很少的自定义，但会提供大量的功能。 ItemsRepeater 构建基块可用于创建你自己的布局控件，它没有相同的内置功能 - 它需要你实施任何必需的功能或交互。
-   如果你需要创建一个无法通过 ListView 或 GridView 创建的高度自定义 UI，或者你的数据源的每个项需要非常不同的行为，则应使用 ItemsRepeater。


通过阅读[指南](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/items-repeater)和[ API 文档](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.itemsrepeater?view=winui-2.2)页面，详细了解 ItemsRepeater。

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果已安装 <strong style="font-weight: semi-bold">XAML 控件库</strong>应用，请单击此处打开该应用，了解 <a href="xamlcontrolsgallery:/item/ListView">ListView</a> 或 <a href="xamlcontrolsgallery:/item/GridView">GridView</a> 的实际应用。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-listview-or-gridview"></a>创建 ListView 或 GridView

ListView 和 GridView 的类型都是 [ItemsControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol)，因此它们可以包含任何类型的项的集合。 ListView 或 GridView 必须在自己的 [Items](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.items) 集合中有项，才能够在屏幕上显示内容。 若要填充视图，可以将项目直接添加到 [Items](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.items) 集合，或者将 [ItemsSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) 属性设置为数据源。 

> [!IMPORTANT]
> 可以使用 Items 或 ItemsSource 填充列表，但无法同时使用这两者。 如果你设置 ItemsSource 属性并使用 XAML 添加项目，将忽略添加的项目。 如果 ItemsSource 属性已设置且使用代码向项集合中添加项，则会引发异常。

为方便起见，本文中的许多示例直接填充了 `Items` 集合。 但是，列表中的项目来自于动态源的情况更常见，例如书籍列表来自于在线数据库。 出于此目的，使用 `ItemsSource` 属性。 

### <a name="add-items-to-a-listview-or-gridview"></a>将项添加到 ListView 或 GridView

可以将项添加到 ListView 或 GridView 的 [Items](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.items) 集合中，使用 XAML 或代码生成相同的结果。 在以下情况下通常通过 XAML 添加项：具有不更改且可以轻松定义的少量项，或者在运行时使用代码生成项。 

<u>方法 1：将项添加到项集合</u>
#### <a name="option-1-add-items-through-xaml"></a>选项 1：通过 XAML 添加项
```xml
<!-- No corresponding C# code is needed for this example. -->

<ListView x:Name="Fruits"> 
   <x:String>Apricot</x:String> 
   <x:String>Banana</x:String> 
   <x:String>Cherry</x:String> 
   <x:String>Orange</x:String> 
   <x:String>Strawberry</x:String> 
</ListView>  
```


#### <a name="option-2-add-items-through-c"></a>选项 2：通过 C# 添加项

##### <a name="c-code"></a>C# 代码：
```csharp
// Create a new ListView and add content. 
ListView Fruits = new ListView(); 
Fruits.Items.Add("Apricot"); 
Fruits.Items.Add("Banana"); 
Fruits.Items.Add("Cherry"); 
Fruits.Items.Add("Orange"); 
Fruits.Items.Add("Strawberry");
 
// Add the ListView to a parent container in the visual tree (that you created in the corresponding XAML file).
FruitsPanel.Children.Add(Fruits); 
```

##### <a name="corresponding-xaml-code"></a>相应的 XAML 代码：
```xml
<StackPanel Name="FruitsPanel"></StackPanel>
```
上述两个选项将生成相同的 ListView，如下所示：

![简单的列表视图](images/listview-basic-code-example2.png)
<br/>
<u>方法 2：通过设置 ItemsSource 添加项</u>

通常使用 ListView 或 GridView 显示源（例如数据库或 Internet）中的数据。 要填充数据源中的 ListView/GridView，请将其 [ItemsSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) 属性设置为数据项集合。 如果 ListView 或 GridView 要保存自定义类对象（如以下示例中所示），则此方法更好。

#### <a name="option-1-set-itemssource-in-c"></a>选项 1：使用 C# 设置 ItemsSource
此时，直接在代码中将列表视图的 ItemsSource 设置为集合实例。 

##### <a name="c-code"></a>C# 代码：
```csharp 
// Class defintion should be provided within the namespace being used, outside of any other classes.

this.InitializeComponent();

// Instead of adding hard coded items to an ObservableCollection as shown below, 
//the data could be pulled asynchronously from a database or the internet.
ObservableCollection<Contact> Contacts = new ObservableCollection<Contact>();

// Contact objects are created by providing a first name, last name, and company for the Contact constructor.
// They are then added to the ObservableCollection Contacts.
Contacts.Add(new Contact("John", "Doe", "ABC Printers"));
Contacts.Add(new Contact("Jane", "Doe", "XYZ Refridgerators"));
Contacts.Add(new Contact("Santa", "Claus", "North Pole Toy Factory Inc."));

// Create a new ListView (or GridView) for the UI, add content by setting ItemsSource
ListView ContactsLV = new ListView();
ContactsLV.ItemsSource = Contacts;

// Add the ListView to a parent container in the visual tree (that you created in the corresponding XAML file)
ContactPanel.Children.Add(ContactsLV);
```

##### <a name="xaml-code"></a>XAML 代码：
```xml
<StackPanel x:Name="ContactPanel"></StackPanel>
```

#### <a name="option-2-set-itemssource-in-xaml"></a>选项 2：使用 XAML 设置 ItemsSource
还可以将 ItemsSource 属性绑定到 XAML 中的集合。 在此处，ItemsSource 绑定到名为 `Contacts` 并且公开 Page 的专用数据集合 `_contacts` 的公共属性。

**XAML**
```xml
<ListView x:Name="ContactsLV" ItemsSource="{x:Bind Contacts}"/>
```

**C#**
```csharp
// Class defintion should be provided within the namespace being used, outside of any other classes.
// These two declarations belong outside of the main page class.
private ObservableCollection<Contact> _contacts = new ObservableCollection<Contact>();

public ObservableCollection<Contact> Contacts
{
    get { return this._contacts; }
}

// This method should be defined within your main page class.
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    base.OnNavigatedTo(e);

    // Instead of hard coded items, the data could be pulled 
    // asynchronously from a database or the internet.
    Contacts.Add(new Contact("John", "Doe", "ABC Printers"));
    Contacts.Add(new Contact("Jane", "Doe", "XYZ Refridgerators"));
    Contacts.Add(new Contact("Santa", "Claus", "North Pole Toy Factory Inc."));
}
```

上述两个选项将生成相同的 ListView，如下所示。 ListView 只显示每个项的字符串表示形式，因为我们未提供数据模板。

![具有 ItemsSource 集的简单列表视图](images/listview-basic-code-example-final.png)

> [!IMPORTANT]
> 如果不定义数据模板，自定义类对象将仅以 ListView 形式显示字符串值（如果有已定义的 [ToString()](https://docs.microsoft.com/uwp/api/windows.foundation.istringable.tostring) 方法）。

 下一部分将详细介绍如何在 ListView 或 GridView 中恰当地直观表示简单的和自定义的类项。

有关数据绑定的详细信息，请参阅[数据绑定概述](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-quickstart)。

> [!NOTE]
> 如果需要在 ListView 中显示分组数据，必须绑定到 [CollectionViewSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource)。 CollectionViewSource 在 XAML 中充当集合类的代理角色，并启用分组支持。 有关详细信息，请参阅 [CollectionViewSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource)。

## <a name="customizing-the-look-of-items-with-a-datatemplate"></a>使用 DataTemplate 自定义项的外观

ListView 或 GridView 中的数据模板定义可视化项/数据的方式。 在默认情况下，数据项以绑定到的数据对象的字符串表现形式显示在 ListView 中。 通过将 [DisplayMemberPath](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.displaymemberpath) 设置到特定的属性，你可以显示数据项的该属性的字符串表现形式。

但是，你通常会希望更丰富地呈现你的数据。 若要具体地指定 ListView/GridView 中项的显示方式，可以创建 [DataTemplate](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DataTemplate)。 DataTemplate 中的 XAML 定义用于显示各项的控件的布局和外观。 该布局中的控件可绑定到数据对象的属性，或者具有在内联中定义的静态内容。 

> [!NOTE]
> 在 DataTemplate 中使用 [x:Bind 标记扩展](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)时，你必须指定 DataTemplate 中的 DataType (`x:DataType`)。

#### <a name="simple-listview-data-template"></a>简单的 ListView 数据模板
在此示例中，数据项是简单的字符串。 数据模板在 ListView 定义中内联定义，将图像添加到字符串左侧，并用青色显示字符串。 这是使用上面所示的方法 1 和选项 1 创建的相同 ListView。

**XAML**
```XML
<!--No corresponding C# code is needed for this example.-->
<ListView x:Name="FruitsList">
                <ListView.ItemTemplate>
                    <DataTemplate x:DataType="x:String">
                        <Grid>
                            <Grid.ColumnDefinitions>
                                <ColumnDefinition Width="47"/>
                                <ColumnDefinition/>
                            </Grid.ColumnDefinitions>
                            <Image Source="Assets/placeholder.png" Width="32" Height="32"
                                HorizontalAlignment="Left" VerticalAlignment="Center"/>
                            <TextBlock Text="{x:Bind}" Foreground="Teal" FontSize="14" 
                                Grid.Column="1" VerticalAlignment="Center"/>
                        </Grid>
                    </DataTemplate>
                </ListView.ItemTemplate>
                <x:String>Apricot</x:String>
                <x:String>Banana</x:String>
                <x:String>Cherry</x:String>
                <x:String>Orange</x:String>
                <x:String>Strawberry</x:String>
            </ListView>

```

以下是在 ListView 中通过此数据模板显示时数据项的外观：

![使用数据模板的 ListView 项目](images/listview-w-datatemplate1-final.png)

#### <a name="listview-data-template-for-custom-class-objects"></a>自定义类对象的 ListView 数据模板
在此示例中，数据项是联系人对象。 数据模板在 ListView 定义中内联定义，将联系人图像添加到联系人名称和公司左侧。 此 ListView 是使用上面提到的方法 2 和选项 2 创建的。
```xml
<ListView x:Name="ContactsLV" ItemsSource="{x:Bind Contacts}">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="local:Contact">
            <Grid>
                <Grid.RowDefinitions>
                    <RowDefinition Height="*"/>
                    <RowDefinition Height="*"/>
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto"/>
                    <ColumnDefinition Width="*"/>
                </Grid.ColumnDefinitions>
                <Image Grid.Column="0" Grid.RowSpan="2" Source="Assets/grey-placeholder.png" Width="32"
                    Height="32" HorizontalAlignment="Center" VerticalAlignment="Center"></Image>
                <TextBlock Grid.Column="1" Text="{x:Bind Name}" Margin="12,6,0,0" 
                    Style="{ThemeResource BaseTextBlockStyle}"/>
                <TextBlock  Grid.Column="1" Grid.Row="1" Text="{x:Bind Company}" Margin="12,0,0,6" 
                    Style="{ThemeResource BodyTextBlockStyle}"/>
            </Grid>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

以下是在 ListView 中通过此数据模板显示时数据项的外观：

![使用数据模板的 ListView 自定义类项](images/listview-customclass-datatemplate-final.png)

使用数据模板是定义 ListView 外观的主要方法。 如果列表保留大量项目，它们也能对性能产生重大影响。  

数据模板可在 ListView/GridView 定义中内联定义（如上所示），也可以在“资源”部分单独定义。 如果数据模板在 ListView/GridView 外定义，则必须被赋予 [x:Key](https://docs.microsoft.com/windows/uwp/xaml-platform/x-key-attribute) 特性并赋给使用该键的 ListView 或 GridView 的 [ItemTemplate ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate) 属性。

有关如何使用数据模板和项目容器定义列表或网格中的项目的外观的更多信息和示例，请参阅[项目容器和模板](item-containers-templates.md)。 

## <a name="change-the-layout-of-items"></a>更改项目的布局

当你将项目添加到 ListView 或 GridView 时，控件会使每个项目在项目容器中自动换行，然后设置所有项目容器的布局。 这些项目容器的布局方式取决于控件的 [ItemsPanel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemspanel)。  
- 默认情况下，“ListView”使用 [ItemsStackPanel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemsstackpanel)，这可以生成垂直列表，如下所示  。

![简单的列表视图](images/listview-simple.png)

- “GridView”使用 [ItemsWrapGrid](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemswrapgrid)，这会水平添加项目，并且垂直换行和滚动，如下所示  。

![简单的网格视图](images/gridview-simple.png)

你可以通过在项目面板上调整属性来修改项目布局，或者可以将默认面板替换为其他面板。

> [!NOTE]
> 如果更改 ItemsPanel，注意不要禁用虚拟化。 **ItemsStackPanel** 和 **ItemsWrapGrid** 均支持虚拟化，所以可以安全使用它们。 如果你使用任何其他面板，可能会禁用虚拟化，并且降低列表视图的性能。 有关详细信息，请参阅[性能](https://docs.microsoft.com/windows/uwp/debug-test-perf/performance-and-xaml-ui)下的列表视图文章。 

此示例显示如何通过更改“ItemsStackPanel”的 [Orientation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemsstackpanel.orientation) 属性来使“ListView”在水平列表中设置项目容器的布局   。
因为默认情况下列表视图垂直滚动，所以你还需要在列表视图的内部 [ScrollViewer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer) 上调整某些属性以使其可以水平滚动。
- [ScrollViewer.HorizontalScrollMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.horizontalscrollmode) 设置为 **Enabled** 或 **Auto**
- [ScrollViewer.HorizontalScrollBarVisibility](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.horizontalscrollbarvisibility) 设置为 **Auto** 
- [ScrollViewer.VerticalScrollMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.verticalscrollmode) 设置为 **Disabled** 
- [ScrollViewer.VerticalScrollBarVisibility](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.verticalscrollbarvisibility) 设置为 **Hidden** 

> [!IMPORTANT]
> 显示这些示例时，列表视图宽度不受约束，因此不会显示水平滚动条。 如果你运行此代码，可以在 ListView 上设置 `Width="180"` 来使滚动条显示。

**XAML**
```xml
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
    <x:String>Apricot</x:String>
    <x:String>Banana</x:String>
    <x:String>Cherry</x:String>
    <x:String>Orange</x:String>
    <x:String>Strawberry</x:String>
</ListView>
```

生成的列表如下所示。

![水平列表视图](images/listview-horizontal2-final.png)

 在下一个示例中，通过使用 **ItemsWrapGrid** 而非 **ItemsStackPanel**，**ListView** 在垂直换行列表中设置项目的布局。 
 
> [!IMPORTANT]
> 列表视图的高度必须受限，以强制控件使容器换行。

**XAML**
```xml
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
    <x:String>Apricot</x:String>
    <x:String>Banana</x:String>
    <x:String>Cherry</x:String>
    <x:String>Orange</x:String>
    <x:String>Strawberry</x:String>
</ListView>
```

生成的列表如下所示。

![具有网格布局的列表视图](images/listview-itemswrapgrid2-final.png)

如果你在列表视图中显示分组数据，ItemsPanel 将确定项目组的布局方式，而非单独项目的布局方式。例如，如果之前显示的水平 ItemsStackPanel 用于显示分组数据，则各组按水平方式排列，但每组中的项目仍垂直堆叠，如下所示。

![已分组的水平列表视图](images/listview-horizontal-groups.png)

## <a name="item-selection-and-interaction"></a>项目选择和交互

你可以选择多种方法来使用户与列表视图交互。 默认情况下，用户可选择一个项目。 你可以更改 [SelectionMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selectionmode) 属性以启用多选或禁用选择。 你可以设置 [IsItemClickEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.isitemclickenabled) 属性，以便用户单击某个项目即可调用操作（例如按钮），而不是选择该项目。

> **注意**&nbsp;&nbsp;ListView 和 GridView 均将 [ListViewSelectionMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewselectionmode) 枚举用于其 SelectionMode 属性。 默认情况下，IsItemClickEnabled 为 **False**，因此你需要仅将其设置为启用单击模式。

此表显示用户可与列表视图交互的方式以及响应交互的方式。

若要启用此交互： | 使用这些设置： | 处理此事件： | 使用此属性以获取选定的项目：
----------------------------|---------------------|--------------------|--------------------------------------------
无交互 | [SelectionMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selectionmode) = **None**、[IsItemClickEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.isitemclickenabled) = **False** | N/A | N/A 
单选 | SelectionMode = **Single**、IsItemClickEnabled = **False** | [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) | [SelectedItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selecteditem)、[SelectedIndex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedindex)  
多选 | SelectionMode = **Multiple**、IsItemClickEnabled = **False** | [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) | [SelectedItems](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selecteditems)  
扩展选择 | SelectionMode = **Extended**、IsItemClickEnabled = **False** | [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) | [SelectedItems](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selecteditems)  
单击 | SelectionMode = **None**、IsItemClickEnabled = **True** | [ItemClick](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.itemclick) | N/A 

> **注意**&nbsp;&nbsp;从 Windows 10 开始，可启用 IsItemClickEnabled 来引发 ItemClick 事件，同时 SelectionMode 也可设置为 Single、Multiple 或 Extended。 如果你执行此操作，将先后引发 ItemClick 事件和 SelectionChanged 事件。 在某些情况下（例如在 ItemClick 事件处理程序中导航到其他页面），不会引发 SelectionChanged 事件，并且不会选择该项目。

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

当 SelectionMode 设置为“Single”时，可以从 [SelectedItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selecteditem) 属性获取选定的数据项  。 你可以使用 [SelectedIndex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedindex) 属性获取选定项目集合中的索引。 如果没有选择任何项目，则 SelectedItem 为 **null**，并且 SelectedIndex 为 -1。 
 
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

当 SelectionMode 设置为“Multiple”或“Extended”时，可以从 [SelectedItems](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selecteditems) 属性获取选定的数据项   。 

**SelectedIndex**、**SelectedItem** 和 **SelectedItems** 属性已同步。 例如，如果你将 SelectedIndex 设置为 -1、将 SelectedItem 设置为 **null** 并且将 SelectedItems 设置为空；如果你将 SelectedItem 设置为 **null**、将 SelectedIndex 设置为 -1 并且将 SelectedItems 设置为空。

在多选模式下，**SelectedItem** 包含第一个选择的项目，而 **Selectedindex** 包含第一个选择的项目的索引。 

### <a name="respond-to-selection-changes"></a>响应选择更改

若要响应列表视图中的选择更改，请处理 [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) 事件。 在事件处理程序代码中，可以从 [SelectionChangedEventArgs.AddedItems](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.addeditems) 属性获取选择项列表。 你可以获取从 [SelectionChangedEventArgs.RemovedItems](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.removeditems) 属性取消选择的任何项目。 除非用户通过按住 Shift 键选择项目范围，否则 AddedItems 和 RemovedItems 集合将最多包含 1 个项目。

此示例显示了如何处理 **SelectionChanged** 事件和访问各种项目集合。

**XAML**
```xml
<StackPanel HorizontalAlignment="Right">
    <ListView x:Name="listView1" SelectionMode="Multiple" 
              SelectionChanged="ListView1_SelectionChanged">
        <x:String>Apricot</x:String>
        <x:String>Banana</x:String>
        <x:String>Cherry</x:String>
        <x:String>Orange</x:String>
        <x:String>Strawberry</x:String>
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
```xml
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

有时你需要以编程方式来操控列表视图的项目选择。 例如，你可能拥有“全选”  按钮来让用户选择列表中的所有项目。 在这种情况下，从 SelectedItems 集合逐个添加或删除项目通常效率不高。 每个项目更改都会导致发生 SelectionChanged 事件，并且当你直接处理项目而非索引值时，该项目会取消虚拟化。

[SelectAll](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selectall)、[SelectRange](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selectrange) 和 [DeselectRange](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.deselectrange) 方法提供比使用 SelectedItems 属性更高效的修改选择的方法。 这些方法使用项目索引范围进行选择或取消选择。 虚拟化的项目将保持虚拟化状态，因为仅使用了索引。 指定范围中的所有项目均已选定（或已取消选定），无论初始选择状态是什么。 SelectionChanged 事件在每次调用这些方法时仅发生一次。

> [!IMPORTANT]
> 仅当 SelectionMode 属性设置为 Multiple 或 Extended 时才应调用这些方法。 如果在 SelectionMode 是 Single 或 None 时调用 SelectRange，将引发异常。

当使用索引范围选择项目时，请使用 [SelectedRanges](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selectedranges) 属性获取列表中的所有选定范围。

如果 ItemsSource 实现了 [IItemsRangeInfo](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.iitemsrangeinfo)，并且你使用这些方法修改选择，则“AddedItems”和“RemovedItems”属性将不会在 SelectionChangedEventArgs 中进行设置   。 设置这些属性需要对项目对象执行取消虚拟化操作。 改为使用 **SelectedRanges** 属性获取项目。

通过调用 SelectAll 方法，可以选择集合中的所有项目。 但是没有相应的方法来取消选择所有项目。 你可以通过以下方法取消选择所有项目：调用 DeselectRange，并传递 [FirstIndex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.itemindexrange.firstindex) 值为 0 并且 [Length](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.itemindexrange.length) 值等于集合中项目数的 [ItemIndexRange](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.itemindexrange)。 下面的示例将显示此内容以及用于选择所有项的选项。

**XAML**
```xml
<StackPanel Width="160">
    <Button Content="Select all" Click="SelectAllButton_Click"/>
    <Button Content="Deselect all" Click="DeselectAllButton_Click"/>
    <ListView x:Name="listView1" SelectionMode="Multiple">
        <x:String>Apricot</x:String>
        <x:String>Banana</x:String>
        <x:String>Cherry</x:String>
        <x:String>Orange</x:String>
        <x:String>Strawberry</x:String>
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

ListView 和 GridView 控件支持在其自身内部以及它们自身与其他 ListView 和 GridView 控件之间拖放项目。 有关实现拖放模式的详细信息，请参阅[拖放](../input/drag-and-drop.md)。

## <a name="get-the-sample-code"></a>获取示例代码

- [XAML ListView 和 GridView 示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlListView) - 演示 ListView 和 GridView 控件。
- [XAML 拖放示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlDragAndDrop) - 演示使用 ListView 控件进行拖放。
- [XAML 控件库示例](https://github.com/Microsoft/Xaml-Controls-Gallery) - 以交互式格式查看所有 XAML 控件。

## <a name="related-articles"></a>相关文章

- [列表](lists.md)
- [项目容器和模板](item-containers-templates.md)
- [拖放](../input/drag-and-drop.md)
