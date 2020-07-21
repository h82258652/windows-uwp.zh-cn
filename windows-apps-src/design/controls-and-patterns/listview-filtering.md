---
Description: 通过用户输入筛选集合中的项。
title: 筛选集合
label: Filtering collections
template: detail.hbs
ms.date: 12/3/2019
ms.topic: article
keywords: windows 10, uwp
pm-contact: anawish
ms.openlocfilehash: 24669b81c244339509e30a43a0da8a2b27e67eeb
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "75302651"
---
# <a name="filtering-collections-and-lists-through-user-input"></a>通过用户输入筛选集合和列表
如果集合显示许多项或紧密绑定到用户交互，则筛选是一项很有用的可以实施的功能。 使用本文所述方法进行的筛选可以实施到大多数集合控件，其中包括 [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView)、[GridView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.gridview) 和 [ItemsRepeater](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.itemsrepeater?view=winui-2.2)。 许多类型的用户输入（例如复选框、单选按钮、滑块）可以用来筛选集合，但本文将重点介绍如何使用基于文本的用户输入，根据用户的搜索用它来实时更新 ListView。 

> [!NOTE]
> 本文将重点介绍如何使用 ListView 进行筛选。 请注意，此筛选方法也可应用到其他集合控件，例如 GridView、ItemsRepeater 或 TreeView。

## <a name="setting-up-the-ui-and-xaml-for-filtering"></a>设置 UI 和 XAML 以便进行筛选
若要实施筛选，应该让应用的 ListView 与 TextBox 或其他允许用户输入的控件一起显示。 用户键入 TextBox 中的文本将用作筛选器，也就是说，只会显示那些包含文本输入/搜索查询的结果。 当用户将内容键入 TextBox 中时，ListView 会不断使用筛选的结果进行更新 - 具体说来，只要 TextBox 中的文本发生变化，即使只变了一个字母，ListView 也会在其项中使用该词进行筛选。

下面的代码显示了一个 UI，其中包含简单的 ListView 及其 DataTemplate，此外还有伴随的 TextBox。 在此示例中，ListView 显示 Person 对象的集合。 Person 是在代码隐藏（不显示在下面的代码示例中）中定义的一个类，每个 Person 对象具有以下属性：FirstName、LastName 和 Company。

使用 TextBox，用户可以键入搜索/筛选词，以便按姓来筛选 Person 对象的列表。 请注意，TextBox 绑定到特定的名称 (`FilterByLName`) 并有其自己的 TextChanged 事件 (`FilteredLV_LNameChanged`)。 我们可以通过绑定名称访问代码隐藏中 TextBox 的内容/文本。只要用户在 TextBox 中键入内容，就会触发 TextChanged 事件，这样我们就可以在接收用户输入以后执行筛选操作。 

若要进行筛选，ListView 必须有一个可以在代码隐藏中进行操作的数据源，例如 `ObservableCollection<>`。 在此示例中，我们在代码隐藏中为 ListView 的 ItemsSource 属性分配了 `ObservableCollection<Person>`。 

```xaml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="1*"></ColumnDefinition>
        <ColumnDefinition Width="1*"></ColumnDefinition>
    </Grid.ColumnDefinitions>
    <Grid.RowDefinitions>
            <RowDefinition Height="400"></RowDefinition>
            <RowDefinition Height="400"></RowDefinition>
    </Grid.RowDefinitions>

    <ListView x:Name="FilteredListView"
                Grid.Column="0"
                Margin="0,0,20,0">

        <ListView.ItemTemplate>
            <DataTemplate x:DataType="local:Person">
                <StackPanel>
                    <TextBlock Style="{ThemeResource BaseTextBlockStyle}" Margin="0,5,0,5">
                        <Run Text="{x:Bind FirstName}"></Run>
                        <Run Text="{x:Bind LastName}"></Run>
                    </TextBlock>
                    <TextBlock Style="{ThemeResource BodyTextBlockStyle}" Margin="0,5,0,5" Text="{x:Bind Company}"/>
                </StackPanel>
            </DataTemplate>
        </ListView.ItemTemplate>

    </ListView>

    <TextBox x:Name="FilterByLName" Grid.Column="1" Header="Last Name" Width="200"
             HorizontalAlignment="Left" VerticalAlignment="Top" Margin="0,0,0,20"
             TextChanged="FilteredLV_LNameChanged"/>
</Grid>
```
## <a name="filtering-the-data"></a>筛选数据
[Linq](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/introduction-to-linq-queries) 查询允许对集合中的某些项进行分组、排序和选择操作。 为了筛选某个列表，我们将构造一个 Linq 查询，该查询仅选择与用户输入的搜索查询/筛选词（在 `FilterByLName` TextBox 中输入）匹配的词。 查询结果可以分配给 [IEnumerable<T>](https://docs.microsoft.com/dotnet/api/system.collections.generic.ienumerable-1) 集合对象。 有了该集合以后，我们就可以使用它与原始列表进行比较，删除不匹配的项，将确实匹配的项添加回来（在使用了退格键的情况下）。

> [!NOTE]
> 若要让 ListView 能够在添加和删减项时以最直观的方式进行动画显示，必须在 ListView 的 ItemsSource 集合本身中删除和添加项，而不是创建一个包含已筛选对象的新集合并将其分配给 ListView 的 ItemsSource 属性。

若要开始，我们需要在单独的集合（例如 `List<T>` 或 `ObservableCollection<T>`）中初始化原始数据源。 在此示例中，我们有一个名为 `People` 的 `List<Person>`，用于保存在 ListView 中显示的所有 Person 对象（此列表的填充/初始化不显示在下面的代码片段中）。 我们还需要一个列表来保存已筛选的数据。每次应用筛选器时，该列表就会更改。 这将是一个名为 `PeopleFiltered` 的 `ObservableCollection<Person>`，在初始化时会有与 `People` 相同的内容。
 
可以通过以下步骤执行筛选操作，如以下代码所示：
 - 将 ListView 的 ItemsSource 属性设置为 `PeopledFiltered`。 
 - 为 `FilterByLName` TextBox 定义 TextChanged 事件 `FilteredLV_LNameChanged()`。 在此函数中筛选数据。
 - 若要筛选数据，请通过 `FilterByLName.Text` 访问用户输入的搜索查询/筛选词。 使用 Linq 查询在 `People` 中选择其姓包含 `FilterByLName.Text` 一词的项，然后将这些匹配项添加到名为 `TempFiltered` 的集合中。
 - 将当前的 `PeopleFiltered` 集合与 `TempFiltered` 中新筛选的项进行比较，根据需要在 `PeopleFiltered` 中删除和添加项。
 - 在 `PeopleFiltered` 中删除和添加项时，ListView 会进行相应的更新和动画显示。

 ```csharp
using System.Linq;

public MainPage()
{
    // Define People collection to hold all Person objects. 
    // Populate collection - i.e. add Person objects (not shown)
    IList<Person> People = new List<Person>();

    // Create PeopleFiltered collection and copy data from original People collection
    ObservableCollection<Person> PeopleFiltered = new ObservableCollection<Person>(People);

    // Set the ListView's ItemsSource property to the PeopleFiltered collection
    FilteredListView.ItemsSource = PeopleFiltered;

    // ... 
}

private void FilteredLV_LNameChanged(object sender, TextChangedEventArgs e)
{
    /* Perform a Linq query to find all Person objects (from the original People collection)
    that fit the criteria of the filter, save them in a new List called TempFiltered. */
    List<Person> TempFiltered;
    
    /* Make sure all text is case-insensitive when comparing, and make sure 
    the filtered items are in a List object */
    TempFiltered = people.Where(contact => contact.LastName.Contains(FilterByLName.Text, StringComparison.InvariantCultureIgnoreCase)).ToList();
    
    /* Go through TempFiltered and compare it with the current PeopleFiltered collection,
    adding and subtracting items as necessary: */

    // First, remove any Person objects in PeopleFiltered that are not in TempFiltered
    for (int i = PeopleFiltered.Count - 1; i >= 0; i--)
    {
        var item = PeopleFiltered[i];
        if (!TempFiltered.Contains(item))
        {
            PeopleFiltered.Remove(item);
        }
    }

    /* Next, add back any Person objects that are included in TempFiltered and may 
    not currently be in PeopleFiltered (in case of a backspace) */

    foreach (var item in TempFiltered)
    {
        if (!PeopleFiltered.Contains(item))
        {
            PeopleFiltered.Add(item);
        }
    }
}
 ```

现在，当用户在 `FilterByLName` TextBox 中键入其筛选词时，ListView 会立即进行更新，只显示其姓包含筛选词的人员。

## <a name="next-steps"></a>后续步骤

### <a name="get-the-sample-code"></a>获取示例代码
- 如果已安装 XAML 控件库</strong>应用，请单击[此处](xamlcontrolsgallery:/item/ListView)打开该应用，此时会看到一个更有说服力且更深入的列表示例，介绍了如何在 ListView 页上进行筛选。
- 获取 [XAML 控件库应用 (Microsoft Store)](https://www.microsoft.com/store/productId/9MSVH128X2ZT)。

### <a name="related-articles"></a>相关文章
- [列表](lists.md)
- [列表视图和网格视图](listview-and-gridview.md)
- [集合命令处理](collection-commanding.md)