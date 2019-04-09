---
Description: ItemsRepeater 是一个轻型控件，生成和显示的项的集合。
title: ItemsRepeater
label: ItemsRepeater
template: detail.hbs
ms.date: 02/01/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3344230bc52013825d94cfbe3668acfa0d7a2e13
ms.sourcegitcommit: c10d7843ccacb8529cb1f53948ee0077298a886d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/04/2019
ms.locfileid: "58913997"
---
# <a name="itemsrepeater"></a>ItemsRepeater

使用[ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)创建自定义集合的体验使用灵活的布局系统、 自定义视图和虚拟化。

与不同[ListView](/uwp/api/windows.ui.xaml.controls.listview)， [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)不提供全面的最终用户体验，并且它没有默认值 UI，并不提供围绕焦点、 所选内容或用户交互的任何策略。 相反，它是一个可用于创建您自己的唯一基于集合的体验和自定义控件的构造块。 虽然它没有内置策略，它使您要附加策略以构建所需的体验。 例如，可以定义要使用布局、 keyboarding 策略中，选择策略，等等。

您可以看作[ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)从概念上讲为数据驱动的面板，而不是像 ListView 完全控制。 可以指定要显示的数据项目的集合、 项模板生成 UI 元素为每个数据项，以及确定如何调整大小和定位元素的布局。 然后，ItemsRepeater 生成基于数据源的子元素，并将它们显示为指定的项模板和布局。 显示的项目不需要是同构的因为 ItemsRepeater 可以加载内容以表示在一个数据模板选择器中指定的条件的数据项目。

| **获取 Windows UI 库** |
| - |
| 此控件是作为 Windows UI 库，包含新控件和适用于 UWP 应用的 UI 功能的 NuGet 包的一部分。 有关详细信息，包括安装说明，请参阅[Windows 用户界面库概述](https://docs.microsoft.com/uwp/toolkits/winui/)。 |

> **重要的 API**：[ItemsRepeater 类](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)， [ScrollViewer 类](/uwp/api/windows.ui.xaml.controls.scrollviewer)

## <a name="is-this-the-right-control"></a>这是正确的控件吗？

使用[ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)若要创建自定义显示的数据集合。 尽管可以使用它提供一组基本的项，您通常可能会将其用作自定义控件模板中显示元素。

如果您需要在列表或使用最少的自定义的网格中显示数据的开箱控件，请考虑使用[ListView](/uwp/api/windows.ui.xaml.controls.listview)或[GridView](/uwp/api/windows.ui.xaml.controls.gridview)。

ItemsRepeater 不具有内置的项集合。 如果您需要的项集合直接提供，而不是绑定到单独的数据源，则可能需要更高策略体验并应使用[ListView](/uwp/api/windows.ui.xaml.controls.listview)或[GridView](/uwp/api/windows.ui.xaml.controls.gridview)。

[ItemsControl](/uwp/api/windows.ui.xaml.controls.itemscontrol)并 ItemsRepeater 这两个启用可自定义集合的体验，但 ItemsRepeater 而 ItemsControl 则不支持虚拟化的用户界面布局。 我们建议您使用 ItemsRepeater 而不是 ItemsControl，无论其只显示数据中的几项或生成自定义集合控件。

> [!NOTE]
> 如果您所想 ItemsControl 满足你的需求和 ItemsRepeater 不的情况下，请将反馈保留上[Windows 用户界面库 GitHub 项目](https://github.com/Microsoft/microsoft-ui-xaml/issues)并告诉我们。

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果有<strong style="font-weight: semi-bold">XAML 控件库</strong>应用程序安装，请单击此处可打开应用，请参阅<a href="xamlcontrolsgallery:/item/ItemsRepeater">ItemsRepeater</a>中操作。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="scrolling-with-itemsrepeater"></a>与 ItemsRepeater 滚动

[ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)不是派生自[控制](/uwp/api/windows.ui.xaml.controls.control)，使其不包含控件模板。 因此，它不包含像 ListView 滚动任何内置或其他集合控件执行操作。

当使用 ItemsRepeater 时，应通过将其在包装提供滚动功能[ScrollViewer](/uwp/api/windows.ui.xaml.controls.scrollviewer)控件。

## <a name="create-an-itemsrepeater"></a>创建 ItemsRepeater

若要使用[ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)，您需要为其提供要显示的 ItemsSource 属性设置的数据。 然后，告诉它如何通过设置显示的项[ItemTemplate](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate)属性。

### <a name="itemssource"></a>ItemsSource

若要填充的视图，设置[ItemsSource](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemssource)属性设置为数据项的集合。 在这里，直接向集合的实例的代码中设置 ItemsSource。

```csharp
ObservableCollection<string> Items = new ObservableCollection<string>();

ItemsRepeater itemsRepeater1 = new ItemsRepeater();
itemsRepeater1.ItemsSource = Items;
```

还可以将 ItemsSource 属性绑定到 XAML 中的集合。 有关数据绑定的详细信息，请参阅[数据绑定概述](https://msdn.microsoft.com/windows/uwp/data-binding/data-binding-quickstart)。


```xaml
<ItemsRepeater ItemsSource="{x:Bind Items}"/>
```

### <a name="data-template"></a>数据模板

项的模板定义数据可视化的方式。 默认情况下，项在视图中显示为向其绑定使用 TextBlock 的数据对象的字符串表示形式。 但是，你通常会希望更丰富地呈现你的数据。 若要指定完全项的显示方式，您可以定义[DataTemplate](/uwp/api/windows.ui.xaml.datatemplate)。 DataTemplate 中的 XAML 定义用于显示各项的控件的布局和外观。 该布局中的控件可绑定到数据对象的属性，或者具有在内联中定义的静态内容。 将分配到 DataTemplate [ItemTemplate](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate) ItemsRepeater 属性。

在此示例中，数据对象是一个简单的字符串。 使用 DataTemplate 将图像添加到左侧的文本，并设置样式的 TextBlock 青色中显示的字符串。

> [!NOTE]
> 在 DataTemplate 中使用 [x:Bind 标记扩展](https://msdn.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)时，你必须指定 DataTemplate 中的 DataType (`x:DataType`)。

```xaml
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
```

下面是项将显示与此数据模板时的显示方式。

![使用数据模板中显示的项](images/listview-itemstemplate.png)

如果您的视图显示大量的项的数据模板中使用的项的元素数可以对性能产生重大影响。 有关详细信息和如何使用数据模板来定义在列表中的各项的外观的示例，请参阅[项的容器和模板](item-containers-templates.md)。

> [!TIP]
> ItemsRepeater 不受限制的 DataTemplate 在 ListView 和其他集合控件等项容器中的内容。 相反，ItemsRepeater 显示仅在 DataTemplate 中定义。 如果你想在项目，以与列表视图项相同的外观，您可以使用容器，如 ListViewItem，数据模板中。 ItemsRepeater 将显示列表视图项的视觉对象，但不能使用其他功能，如选择或显示多选复选框。
>
> 类似地，如果你的数据集合的实际控件的集合，如按钮 (`List<Button>`)，可以将 ContentPresenter 放在你的 DataTemplate 以显示该控件。

#### <a name="datatemplateselector"></a>DataTemplateSelector

在视图中显示的项不需要的类型相同。 [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)可以使用**DataTemplateSelector**加载数据模板，用于表示基于指定的条件的数据项目。 有关详细信息和示例，请参阅[DataTemplateSelector](/uwp/api/windows.ui.xaml.controls.datatemplateselector)。

> [!NOTE]
> 使用 DataTemplate 或 DataTemplateSelector 的替代方法是实现您自己的类派生自[Microsoft.UI.Xaml.Controls.ElementFactory](/uwp/api/microsoft.ui.xaml.controls.elementfactory) ，它负责生成请求时的内容。

## <a name="configure-the-data-source"></a>配置数据源

使用[ItemsSource](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemssource)属性来指定要用于生成内容的项的集合。 您可以设置为实现的任何类型的 ItemsSource **IEnumerable**。 实现您的数据源的其他集合接口确定哪些功能可供 ItemsRepeater 与你的数据进行交互。

此列表显示了可用接口以及何时应考虑使用每个。

- [IEnumerable](/dotnet/api/system.collections.generic.ienumerable-1)(.NET) / [IIterable](/uwp/api/windows.foundation.collections.iiterable_t_)

  - 可用于小型、 静态的数据集。

    至少，数据源必须实现 IEnumerable / IIterable 接口。 如果这是受支持的所有控件将循环访问的所有内容一次创建其可用于访问通过索引值的项的副本中。

- [IReadonlyList](/dotnet/api/system.collections.generic.ireadonlylist-1)(.NET) / [IVectorView](/uwp/api/windows.foundation.collections.ivectorview_t_)

  - 可用于静态的只读数据集。

    启用按索引访问项控件，并避免冗余内部副本。

- [IList](/dotnet/api/system.collections.generic.ilist-1)(.NET) / [IVector](/uwp/api/windows.foundation.collections.ivector_t_)

  - 可用于静态数据集。

    启用按索引访问项控件，并避免冗余内部副本。

    **警告**：而无需实现更改为列表/向量[INotifyCollectionChanged](/dotnet/api/system.collections.specialized.inotifycollectionchanged)不会反映在 UI 中。

- [INotifyCollectionChanged](/dotnet/api/system.collections.specialized.inotifycollectionchanged)(.NET)

  - 建议支持更改通知。

    使控件能够观察和对数据源中的更改做出响应并在 UI 中反映这些更改。

- [IObservableVector](/uwp/api/windows.foundation.collections.iobservablevector_t_)

  - 支持更改通知

    像**INotifyCollectionChanged**接口，这使得控件可以观察并对数据源中的更改做出反应。

    **警告**：Windows.Foundation.IObservableVector\<T > 不支持的移动操作。 这会导致失去其可视状态项的 UI。  例如，当前所选和/或具有焦点移动通过添加后跟 Remove 的项会失去焦点并且不再选择。

    Platform.Collections.Vector\<T > 使用 IObservableVector\<T > 和具有此相同的限制。 如果支持需要移动操作，然后使用**INotifyCollectionChanged**接口。  .NET ObservableCollection\<T > 类使用**INotifyCollectionChanged**。

- [IKeyIndexMapping](/uwp/api/microsoft.ui.xaml.controls.ikeyindexmapping)

  - 当唯一标识符可以是与每个项相关联。  使用重置作为集合更改操作时，建议使用。

    使得控件可以非常高效地收到作为的一部分的硬重置操作后恢复现有 UI **INotifyCollectionChanged**或**IObservableVector**事件。 接收重置后该控件将使用提供的唯一 ID 来将当前数据与它已创建的元素相关联。 如果索引映射到键没有控件必须假定它需要的数据创建 UI 中从头重新开始。

在 ListView 和 GridView 中一样，IKeyIndexMapping，以外，上面列出的接口提供 ItemsRepeater 中相同的行为。


以下接口上 ItemsSource 启用 ListView 和 GridView 控件中的特殊功能，但当前没有任何影响 ItemsRepeater:

- [ISupportIncrementalLoading](/uwp/api/windows.ui.xaml.data.isupportincrementalloading)
- [IItemsRangeInfo](/uwp/api/windows.ui.xaml.data.iitemsrangeinfo)
- [ISelectionInfo](/uwp/api/windows.ui.xaml.data.iselectioninfo)

> [!TIP]
> 我们很期待你的反馈！ 让我们知道您的想法[Windows 用户界面库 GitHub 项目](https://github.com/Microsoft/microsoft-ui-xaml/issues)。 请考虑如上现有的征求建议书添加您的想法[#374](https://github.com/Microsoft/microsoft-ui-xaml/issues/374):添加对 ItemsRepeater 增量加载支持。

以增量方式加载数据，当用户滚动向上或向下一种替代方法是观察 ScrollViewer 的视区的位置和加载更多数据，随着视区的临近程度。

```xaml
<ScrollViewer ViewChanged="ScrollViewer_ViewChanged">
    <ItemsRepeater ItemsSource="{x:Bind MyItemsSource}" .../>
</ScrollViewer>
```

```csharp
private async void ScrollViewer_ViewChanged(object sender, ScrollViewerViewChangedEventArgs e)
{
    if (!e.IsIntermediate)
    {
        var scroller = (ScrollViewer)sender;
        var distanceToEnd = scroller.ExtentHeight - (scroller.VerticalOffset + scroller.ViewportHeight);

        // trigger if within 2 viewports of the end
        if (distanceToEnd <= 2.0 * scroller.ViewportHeight
                && MyItemsSource.HasMore && !itemsSource.Busy)
        {
            // show an indeterminate progress UI
            myLoadingIndicator.Visibility = Visibility.Visible;

            await MyItemsSource.LoadMoreItemsAsync(/*DataFetchSize*/);

            loadingIndicator.Visibility = Visibility.Collapsed;
        }
    }
}
```

## <a name="change-the-layout-of-items"></a>更改项目的布局

通过显示的项[ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)按排列[布局](/uwp/api/microsoft.ui.xaml.controls.layout)对象，用于管理其子元素的位置和大小。 当用于 ItemsRepeater，布局对象启用 UI 虚拟化。 提供的布局[StackLayout](/uwp/api/microsoft.ui.xaml.controls.stacklayout)并[UniformGridLayout](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout)。 默认情况下，ItemsRepeater StackLayout 使用垂直方向。

### <a name="stacklayout"></a>StackLayout

[StackLayout](/uwp/api/microsoft.ui.xaml.controls.stacklayout)元素排列成水平或垂直可以定位的一行。

可以设置[间距](/en-us/uwp/api/microsoft.ui.xaml.controls.stacklayout.spacing)属性来调整项之间的空间量。 间距已应用的布局的方向[方向](/uwp/api/microsoft.ui.xaml.controls.stacklayout.orientation)。

![堆栈布局间距](images/stack-layout.png)

此示例演示如何将 ItemsRepeater.Layout 属性设置为水平方向和间距的 8 个像素 StackLayout。

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<muxc:ItemsRepeater ItemsSource="{x:Bind Items}" ItemTemplate="{StaticResource MyTemplate}">
    <muxc:ItemsRepeater.Layout>
        <muxc:StackLayout Orientation="Horizontal" Spacing="8"/>
    </muxc:ItemsRepeater.Layout>
</muxc:ItemsRepeater>
```

### <a name="uniformgridlayout"></a>UniformGridLayout

[UniformGridLayout](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout)换行布局中按顺序定位元素。 项目是从左到右的顺序排列时[方向](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.orientation)是**水平**，并放置到上到下方向时**垂直**。 均匀调整大小的每个项。

![统一的网格布局间距](images/uniform-grid-layout.png)

水平布局的每一行中的项的数目会影响最小项宽度。 垂直布局的每个列中的项的数目会影响最小项高度。

- 您可以显式提供要使用通过设置最小大小[MinItemHeight](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.minitemheight)并[MinItemWidth](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.minitemwidth)属性。
- 如果未指定最小大小，第一项所测得的大小被视为每个项的最小大小。

您还可以设置布局以设置包含行和列之间的最小间距[MinColumnSpacing](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.mincolumnspacing)并[MinRowSpacing](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.minrowspacing)属性。

![统一的网格大小调整和间距](images/uniform-grid-sizing-spacing.png)

如果已确定行或列中的项目数基于项的最小大小和间距后，可能有未使用的空间完成后仍保持行或列中的最后一项 （如在上图中所示）。 您可以指定任何额外空间是被忽略，用来增加每个项的大小或用于创建的项之间的额外空间。 这由控制[ItemsStretch](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.itemsstretch)并[ItemsJustification](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.itemsjustification)属性。

可以设置[ItemsStretch](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.itemsstretch)属性来指定如何增加的项大小以填充未使用的空间。

此列表显示可用的值。 定义采用默认值**方向**的**水平**。

- **无**：额外空间保持为未使用的行的末尾。 这是默认设置。
- **填充**:项提供额外的宽度，用完可用空间 （如果垂直高度）。
- **统一**:项是提供额外的宽度，用完可用空间，并提供额外的高度，为了保持纵横比 （高度和宽度切换如果垂直）。

下图显示的效果**ItemsStretch**了水平布局中的值。

![统一的网格项 stretch](images/uniform-grid-item-stretch.png)

当**ItemsStretch**是**None**，可以设置[ItemsJustification](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.itemsjustification)属性以指定如何额外空间用于对齐的项。

此列表显示可用的值。 定义采用默认值**方向**的**水平**。

- **开始**：项分别与行的开头。 额外空间保持为未使用的行的末尾。 这是默认设置。
- **Center**:项的行的中心对齐。 在开始和结束的行的均匀划分额外空间。
- **结束**:项分别与行的结尾。 额外的空间保留在行的开头未使用。
- **SpaceAround**:项平均分发。 之前和之后的每个项添加相同大小的空间。
- **SpaceBetween**:项平均分发。 每个项之间添加相同大小的空间。 在开始和结束的行的添加没有空格。
- **SpaceEvenly**:项具有相同数量的空间，每个项之间和在的开始和结束的行的平均分发。

下图显示的效果**ItemsStretch**垂直布局 （应用于列而不是行） 中的值。

![统一的网格项对齐方式](images/uniform-grid-item-justification.png)

> [!TIP]
> **ItemsStretch**属性会影响_度量值_传递的布局。 **ItemsJustification**属性会影响_排列_传递的布局。

此示例演示如何设置**ItemsRepeater.Layout**属性设置为**UniformGridLayout**。

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<muxc:ItemsRepeater ItemsSource="{x:Bind Items}"
                    ItemTemplate="{StaticResource MyTemplate}">
    <muxc:ItemsRepeater.Layout>
        <muxc:UniformGridLayout MinItemWidth="200"
                                MinColumnSpacing="28"
                                ItemsJustification="SpaceAround"/>
    </muxc:ItemsRepeater.Layout>
</muxc:ItemsRepeater>
```

## <a name="lifecycle-events"></a>生命周期事件

主机中的项时[ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)，可能需要显示或停止显示项时执行某些操作，例如当开始异步下载某些内容，将元素与关联一种机制来跟踪所选内容，或停止一些后台任务。

在虚拟化控件中，您不能依赖于已加载/未加载事件因为元素可能不会删除实时可视化树中已回收时。 相反，提供了其他事件以管理元素的生命周期。 下图显示了一个元素的生命周期中 ItemsRepeater 和引发相关的事件时。

![生命周期事件关系图](images/items-repeater-lifecycle.png)

- [**ElementPrepared** ](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.elementprepared)元素由准备好使用每次时发生。 这是个新创建的元素以及已存在且正在重新使用回收队列中的元素。
- [**ElementClearing** ](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.elementclearing)元素已发送到回收队列中，例如当它不在范围内的每次意识到项会发生。
- [**ElementIndexChanged** ](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.elementindexchanged
)发生每个已实现 UIElement，它表示的项的索引已更改的。 例如，当添加或删除数据源中另一个项，后面出现在排序中的项的索引收到此事件。

此示例演示如何使用这些事件将附加自定义选择服务来跟踪使用 ItemsRepeater 来显示项的自定义控件中的项选择。

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<UserControl ...>
    ...
    <ScrollViewer>
        <muxc:ItemsRepeater ItemsSource="{x:Bind Items}"
                            ItemTemplate="{StaticResource MyTemplate}"
                            ElementPrepared="OnElementPrepared"
                            ElementIndexChanged="OnElementIndexChanged"
                            ElementClearing="OnElementClearing">
        </muxc:ItemsRepeater>
    </ScrollViewer>
    ...
</UserControl>
```

```csharp
interface ISelectable
{
    int SelectionIndex { get; set; }
    void UnregisterSelectionModel(SelectionModel selectionModel);
    void RegisterSelectionModel(SelectionModel selectionModel);
}

private void OnElementPrepared(ItemsRepeater sender, ElementPreparedEventArgs args)
{
    var selectable = args.Element as ISelectable;
    if (selectable != null)
    {
        // Wire up this item to recognize a 'select' and listen for programmatic
        // changes to the selection model to know when to update its visual state.
        selectable.SelectionIndex = args.Index;
        selectable.RegisterSelectionModel(this.SelectionModel);
    }
}

private void OnElementIndexChanged(ItemsRepeater sender, ElementIndexChangedEventArgs args)
{
    var selectable = args.Element as ISelectable;
    if (selectable != null)
    {
        // Sync the ID we use to notify the selection model when the item
        // we represent has changed location in the data source.
        selectable.SelectionIndex = args.NewIndex;
    }
}

private void OnElementClearing(ItemsRepeater sender, ElementClearingEventArgs args)
{
    var selectable = args.Element as ISelectable;
    if (selectable != null)
    {
        // Disconnect handlers to recognize a 'select' and stop
        // listening for programmatic changes to the selection model.
        selectable.UnregisterSelectionModel(this.SelectionModel);
        selectable.SelectionIndex = -1;
    }
}
```

## <a name="sorting-filtering-and-resetting-the-data"></a>排序、 筛选和重置数据

当你执行操作，例如筛选或排序数据集时，您传统上可能有相比以前的数据与新的数据集，则颁发通过精细的更改通知[INotifyCollectionChanged](/uwp/api/windows.ui.xaml.interop.inotifycollectionchanged)。 但是，它通常是更轻松地完全旧数据替换为新的数据和触发器使用集合更改通知[重置](/uwp/api/windows.ui.xaml.interop.notifycollectionchangedaction)操作相反。

通常情况下，重置会导致控件来发布现有的子元素，重新开始，构建用户界面从滚动位置 0 处开始，因为它具有完全的数据已更改在重置期间如何不清楚。

但是，如果集合分配为 ItemsSource 支持的唯一标识符通过实现[IKeyIndexMapping](/uwp/api/microsoft.ui.xaml.controls.ikeyindexmapping)接口，则可以快速识别 ItemsRepeater:

- 已存在之前和之后重置的数据的可重用 UIElements
- 已删除以前可见的项
- 添加新项，将显示

这样就避免从滚动位置 0 重新开始 ItemsRepeater。 它还允许它快速恢复的数据的未更改在重置，从而导致更好的性能 Uielement。

此示例演示如何在垂直堆栈中显示的项的列表， _MyItemsSource_是包装的项的基础列表的自定义数据源。 它公开_数据_属性，可用于重新分配要作为项目源，然后触发重置使用的新列表。

```xaml
<ScrollViewer x:Name="sv">
    <ItemsRepeater x:Name="repeater"
                ItemsSource="{x:Bind MyItemsSource}"
                ItemTemplate="{StaticResource MyTemplate}">
       <ItemsRepeater.Layout>
           <StackLayout ItemSpacing="8"/>
       </ItemsRepeater.Layout>
   </ItemsRepeater>
</ScrollViewer>
```

```csharp
public MainPage()
{
    this.InitializeComponent();

    // Similar to an ItemsControl, a developer sets the ItemsRepeater's ItemsSource.
    // Here we provide our custom source that supports unique IDs which enables
    // ItemsRepeater to be smart about handling resets from the data.
    // Unique IDs also make it easy to do things apply sorting/filtering
    // without impacting any state (i.e. selection).
    MyItemsSource myItemsSource = new MyItemsSource(data);

    repeater.ItemsSource = myItemsSource;

    // ...

    // We can sort/filter the data using whatever mechanism makes the
    // most sense (LINQ, database query, etc.) and then reassign
    // it, which in our implementation triggers a reset.
    myItemsSource.Data = someNewData;
}

// ...


public class MyItemsSource : IReadOnlyList<ItemBase>, IKeyIndexMapping, INotifyCollectionChanged
{
    private IList<ItemBase> _data;

    public MyItemsSource(IEnumerable<ItemBase> data)
    {
        if (data == null) throw new ArgumentNullException();

        this._data = data.ToList();
    }

    public IList<ItemBase> Data
    {
        get { return _data; }
        set
        {
            _data = value;

            // Instead of tossing out existing elements and re-creating them,
            // ItemsRepeater will reuse the existing elements and match them up
            // with the data again.
            this.CollectionChanged?.Invoke(
                this,
                new NotifyCollectionChangedEventArgs(NotifyCollectionChangedAction.Reset));
        }
    }

    #region IReadOnlyList<T>

    public ItemBase this[int index] => this.Data != null
        ? this.Data[index]
        : throw new IndexOutOfRangeException();

    public int Count => this.Data != null ? this.Data.Count : 0;
    public IEnumerator<ItemBase> GetEnumerator() => this.Data.GetEnumerator();
    IEnumerator IEnumerable.GetEnumerator() => this.GetEnumerator();

    #endregion

    #region INotifyCollectionChanged

    public event NotifyCollectionChangedEventHandler CollectionChanged;

    #endregion

    #region IKeyIndexMapping

    private int lastRequestedIndex = IndexNotFound;
    private const int IndexNotFound = -1;

    // When UniqueIDs are supported, the ItemsRepeater caches the unique ID for each item
    // with the matching UIElement that represents the item.  When a reset occurs the
    // ItemsRepeater pairs up the already generated UIElements with items in the data
    // source.
    // ItemsRepeater uses IndexForUniqueId after a reset to probe the data and identify
    // the new index of an item to use as the anchor.  If that item no
    // longer exists in the data source it may try using another cached unique ID until
    // either a match is found or it determines that all the previously visible items
    // no longer exist.
    public int IndexForUniqueId(string uniqueId)
    {
        // We'll try to increase our odds of finding a match sooner by starting from the
        // position that we know was last requested and search forward.
        var start = lastRequestedIndex;
        for (int i = start; i < this.Count; i++)
        {
            if (this[i].PrimaryKey.Equals(uniqueId))
                return i;
        }

        // Then try searching backward.
        start = Math.Min(this.Count - 1, lastRequestedIndex);
        for (int i = start; i >= 0; i--)
        {
            if (this[i].PrimaryKey.Equals(uniqueId))
                return i;
        }

        return IndexNotFound;
    }

    public string UniqueIdForIndex(int index)
    {
        var key = this[index].PrimaryKey;
        lastRequestedIndex = index;
        return key;
    }

    #endregion
}

```

## <a name="create-a-custom-collection-control"></a>创建自定义集合控件

可以使用[ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)创建自定义集合控件完成，但其自己的控件以呈现每个项类型。

> [!NOTE]
> 这是类似于使用**ItemsControl**，但而不是派生自**ItemsControl**放**ItemsPresenter**在控件模板中，您从派生**控件**，并插入**ItemsRepeater**控件模板中。 自定义收集控件"具有" **ItemsRepeater**而不是"是" **ItemsControl**。 这意味着必须显式选择哪些属性公开，而不是其继承属性，以不支持。

此示例演示如何将放[ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)中的名为的自定义控件模板_MediaCollectionView_并公开其属性。

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<Style TargetType="local:MediaCollectionView">
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="local:MediaCollectionView">
                <Border
                    Background="{TemplateBinding Background}"
                    BorderBrush="{TemplateBinding BorderBrush}"
                    BorderThickness="{TemplateBinding BorderThickness}">
                    <ScrollViewer x:Name="ScrollViewer">
                        <muxc:ItemsRepeater x:Name="ItemsRepeater"
                                            ItemsSource="{TemplateBinding ItemsSource}"
                                            ItemTemplate="{TemplateBinding ItemTemplate}"
                                            Layout="{TemplateBinding Layout}"
                                            TabFocusNavigation="{TemplateBinding TabFocusNavigation}"/>
                    </ScrollViewer>
                </Border>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>
```

```csharp
public sealed class MediaCollectionView : Control
{
    public object ItemsSource
    {
        get { return (object)GetValue(ItemsSourceProperty); }
        set { SetValue(ItemsSourceProperty, value); }
    }

    // Using a DependencyProperty as the backing store for ItemsSource.  This enables animation, styling, binding, etc...
    public static readonly DependencyProperty ItemsSourceProperty =
        DependencyProperty.Register(nameof(ItemsSource), typeof(object), typeof(MediaCollectionView), new PropertyMetadata(0));

    public DataTemplate ItemTemplate
    {
        get { return (DataTemplate)GetValue(ItemTemplateProperty); }
        set { SetValue(ItemTemplateProperty, value); }
    }

    // Using a DependencyProperty as the backing store for ItemTemplate.  This enables animation, styling, binding, etc...
    public static readonly DependencyProperty ItemTemplateProperty =
        DependencyProperty.Register(nameof(ItemTemplate), typeof(DataTemplate), typeof(MediaCollectionView), new PropertyMetadata(0));

    public Layout Layout
    {
        get { return (Layout)GetValue(LayoutProperty); }
        set { SetValue(LayoutProperty, value); }
    }

    // Using a DependencyProperty as the backing store for Layout.  This enables animation, styling, binding, etc...
    public static readonly DependencyProperty LayoutProperty =
        DependencyProperty.Register(nameof(Layout), typeof(Layout), typeof(MediaCollectionView), new PropertyMetadata(0));

    public MediaCollectionView()
    {
        this.DefaultStyleKey = typeof(MediaCollectionView);
    }
}
```

## <a name="display-grouped-items"></a>显示分组的项

可以嵌套[ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)中[ItemTemplate](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate)的另一个 ItemsRepeater 创建嵌套虚拟化布局。 该框架将有效地利用资源的最大程度减少不必要的实现的不可见的元素或接近当前视区。

此示例演示如何可以在垂直堆栈中显示的分组项的列表。 外部 ItemsRepeater 生成每个组。 在每个组的模板，另一个 ItemsRepeater 生成项。

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<ScrollViewer>
  <muxc:ItemsRepeater ItemsSource="{x:Bind AppNotifications}"
                      Layout="{StaticResource MyGroupLayout}">
    <muxc:ItemsRepeater.ItemTemplate>
      <DataTemplate x:DataType="ExampleApp:AppNotifications">
        <!-- Group -->
        <StackPanel>
          <!-- Header -->
          TextBlock Text="{x:Bind AppTitle}"/>
          <!-- Items -->
          <muxc:ItemsRepeater ItemsSource="{x:Bind Notifications}"
                              Layout="{StaticResource MyItemLayout}"
                              ItemTemplate="{StaticResource MyTemplate}"/>
          <!-- Footer -->
          <Button Content="{x:Bind FooterText}"/>
        </StackPanel>
      </DataTemplate>
    </muxc:ItemsRepeater.ItemTemplate>
  </muxc:ItemsRepeater>
</ScrollViewer>
```

此示例演示具有各种类别，可以使用用户首选项更改并显示为水平滚动列表，如下所示的应用的布局。

![具有项 repeater 的嵌套的布局](images/items-repeater-nested-layout.png)

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<ScrollViewer>
  <muxc:ItemsRepeater ItemsSource="{x:Bind Categories}"
                      Background="LightGreen">
    <muxc:ItemsRepeater.ItemTemplate>
      <DataTemplate x:DataType="local:Category">
        <StackPanel Margin="12,0">
          <TextBlock Text="{x:Bind Name}" Style="{ThemeResource TitleTextBlockStyle}"/>
          <ScrollViewer HorizontalScrollMode="Enabled"
                                          VerticalScrollMode="Disabled"
                                          HorizontalScrollBarVisibility="Auto" >
            <muxc:ItemsRepeater ItemsSource="{x:Bind Items}"
                                Background="Orange">
              <muxc:ItemsRepeater.ItemTemplate>
                <DataTemplate x:DataType="local:CategoryItem">
                  <Grid Margin="10"
                        Height="60" Width="120"
                        Background="LightBlue">
                    <TextBlock Text="{x:Bind Name}"
                               Style="{StaticResource SubtitleTextBlockStyle}"
                               Margin="4"/>
                  </Grid>
                </DataTemplate>
              </muxc:ItemsRepeater.ItemTemplate>
              <muxc:ItemsRepeater.Layout>
                <muxc:StackLayout Orientation="Horizontal"/>
              </muxc:ItemsRepeater.Layout>
            </muxc:ItemsRepeater>
          </ScrollViewer>
        </StackPanel>
      </DataTemplate>
    </muxc:ItemsRepeater.ItemTemplate>
  </muxc:ItemsRepeater>
</ScrollViewer>
```

## <a name="bringing-an-element-into-view"></a>将元素添加到视图

XAML 框架已处理 1） 接收键盘焦点或 2） 接收到讲述人焦点时将 FrameworkElement 引入视图。 可能有其他情况下您需要显式将元素放入视图。 例如，在响应用户操作或页面导航后还原 UI 状态。

将虚拟化的元素添加到视图涉及以下过程：
1. 请注意项 UIElement
2. 运行布局，以确保该元素具有有效的位置
3. 发起的请求将实现的元素放入视图

下面的示例演示了这些步骤作为页面导航后还原平面、 垂直列表中项的滚动位置的一部分。 对于分层数据使用嵌套的 ItemsRepeaters 方法实质上是相同的但必须完成每个级别的层次结构。

```xaml
<ScrollViewer x:Name="scrollviewer">
  <ItemsRepeater x:Name="repeater" .../>
</ScrollViewer>
```

```csharp
public class MyPage : Page
{
    // ...

    protected override void OnNavigatedTo(NavigationEventArgs e)
    {
        base.OnNavigatedTo(e);

            // retrieve saved offset + index(es) of the tracked element and then bring it into view.
            // ... 

            var element = repeater.GetOrCreateElement(index);

            // ensure the item is given a valid position
            element.UpdateLayout();

            element.StartBringIntoView(new BringIntoViewOptions()
            {
                VerticalOffset = relativeVerticalOffset
            });
    }

    protected override void OnNavigatingFrom(NavigatingCancelEventArgs e)
    {
        base.OnNavigatingFrom(e);

        // retrieve and save the relative offset and index(es) of the scrollviewer's current anchor element ...
        var anchor = this.scrollviewer.CurrentAnchor;
        var index = this.repeater.GetElementIndex(anchor);
        var anchorBounds = anchor.TransformToVisual(this.scrollviewer).TransformBounds(new Rect(0, 0, anchor.ActualWidth, anchor.ActualHeight));
        relativeVerticalOffset = this.sv.VerticalOffset – anchorBounds.Top;
    }
}

```

## <a name="enable-accessibility"></a>启用辅助功能

[ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)不提供默认可访问性体验。 上的文档[UWP 应用的可用性](/windows/uwp/design/usability)提供丰富的信息，帮助确保您的应用程序提供了非独占的用户体验。 如果您使用 ItemsRepeater 来创建自定义控件然后请务必在查看文档[自定义自动化对等](/windows/uwp/design/accessibility/custom-automation-peers)。

### <a name="keyboarding"></a>键盘操作
ItemsRepeater 提供的焦点移动的最小 keyboarding 支持基于 XAML 的[2D 方向导航的 Keyboarding](/windows/uwp/design/input/focus-navigation#2d-directional-navigation-for-keyboard)。

![方向导航](/windows/uwp/design/input/images/keyboard/directional-navigation.png)

ItemsRepeater [XYFocusKeyboardNavigation 模式](/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode)是_已启用_默认情况下。 具体取决于预期的体验，请考虑添加对常见[键盘交互](/windows/uwp/design/input/keyboard-interactions)主页、 结束、 PageUp，和 PageDown 等。

ItemsRepeater 自动确保，其项的默认 tab 键顺序 （无论是虚拟化或不） 遵循相同的项的数据中指定的顺序。 默认情况下 ItemsRepeater 具有其[TabFocusNavigation](/uwp/api/windows.ui.xaml.uielement.tabfocusnavigation)属性设置为[一次](/uwp/api/windows.ui.xaml.input.keyboardnavigationmode)而不是公共的默认值为_本地_。

> [!NOTE]
> ItemsRepeater 不会自动记住已设定焦点的最后一项。  这意味着，当用户正在使用时按 Shift + Tab 可以到最后一个执行实现项。

### <a name="announcing-item-x-of-y-in-screen-readers"></a>宣布推出"项_X_的_Y_"在屏幕读取器

管理设置适当的自动化属性，如值所需**PositionInSet**和**SizeOfSet**，并确保它们保持最新状态，当项目添加、 移动、 删除，等等。

在某些自定义布局不可能到视觉顺序的明显序列。  按最小方式，用户希望获得由屏幕读取器的 PositionInSet 和 SizeOfSet 属性的值将与匹配项中的数据 （偏移量 1 以匹配自然与正在从 0 开始计数） 的显示的顺序。

实现此目的的最佳方法是通过让项控件实现的自动化对等方[GetPositionInSetCore](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpositioninsetcore)并[GetSizeOfSetCore](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getsizeofsetcore)方法和报表数据集中的项位置表示由该控件。 在运行时通过辅助技术访问时仅计算的值，并使其保持最新变得不产生问题。 值匹配的数据顺序。

此示例演示如何来实现此目的提供自定义控件调用时_CardControl_。

```xaml
<ScrollViewer >
    <ItemsRepeater x:Name="repeater" ItemsSource="{x:Bind MyItemsSource}">
       <ItemsRepeater.ItemTemplate>
           <DataTemplate x:DataType="local:CardViewModel">
               <local:CardControl Item="{x:Bind}"/>
           </DataTemplate>
       </ItemsRepeater.ItemTemplate>
   </ItemsRepeater>
</ScrollViewer>
```

```csharp
internal sealed class CardControl : CardControlBase
{
    protected override AutomationPeer OnCreateAutomationPeer() => new CardControlAutomationPeer(this);

    private sealed class CardControlAutomationPeer : FrameworkElementAutomationPeer
    {
        private readonly CardControl owner;

        public CardControlAutomationPeer(CardControl owner) : base(owner) => this.owner = owner;

        protected override int GetPositionInSetCore()
          => ((ItemsRepeater)owner.Parent)?.GetElementIndex(this.owner) + 1 ?? base.GetPositionInSetCore();

        protected override int GetSizeOfSetCore()
          => ((ItemsRepeater)owner.Parent)?.ItemsSourceView?.Count ?? base.GetSizeOfSetCore();
    }
}
```

## <a name="related-articles"></a>相关文章

- [列表](lists.md)
- [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)
- [ScrollViewer](/uwp/api/windows.ui.xaml.controls.scrollviewer)
