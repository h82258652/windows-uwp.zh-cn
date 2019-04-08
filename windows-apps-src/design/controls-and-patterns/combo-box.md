---
Description: 在用户键入时提供建议的文本输入框。
title: 组合框 （下拉列表）
label: Combo box
template: detail.hbs
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: ''
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 21a6c698fa0e07587e2c25ae827dc6654a8ced9d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618622"
---
# <a name="combo-box"></a>组合框

使用组合框 （也称为下拉列表） 提供的用户可以从选择的项的列表。 组合框处于 compact 状态启动，并将展开以显示选择项的列表。

当关闭组合框时，它显示当前所选内容，或者如果没有选定的项为空。 用户在扩展组合框，将显示可选择项的列表。

> **重要的 API**：[ComboBox 类](/uwp/api/Windows.UI.Xaml.Controls.ComboBox)， [IsEditable 属性](/uwp/api/windows.ui.xaml.controls.combobox.iseditable)， [Text 属性](/uwp/api/Windows.UI.Xaml.Controls.ComboBox)， [TextSubmitted 事件](/uwp/api/Windows.UI.Xaml.Controls.ComboBox)

组合框使用标头压缩状态。

![处于紧凑状态的下拉列表示例](images/combo_box_collapsed.png)

## <a name="is-this-the-right-control"></a>这是正确的控件吗？

- 使用下拉列表可让用户从一组项中选择单个值，这些项可使用单行文本完全显示。
- 使用列表视图或网格视图（而非组合框），显示包含多行文本或多张图像的项。
- 当少于五项时，应考虑使用[单选按钮](radio-button.md)（当进行单项选择时）或[复选框](checkbox.md)（当进行多项选择时）。
- 当选择项在应用流中不太重要时使用组合框。 如果在多数情况下为大部分用户推荐了默认选项，则通过使用列表视图显示所有项可能会导致用户过多地注意这些选项。 使用组合框可以节省空间并减少分心。

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果有<strong style="font-weight: semi-bold">XAML 控件库</strong>应用程序安装，请单击此处<a href="xamlcontrolsgallery:/item/ComboBox">打开应用，请参阅操作组合框</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

处于紧凑状态的组合框可以显示标题。

![处于紧凑状态的下拉列表示例](images/combo_box_collapsed.png)

尽管组合框可通过展开来支持较长的字符串，但应避免使用难以阅读的过长的字符串。

![带有长文本字符串的下拉列表示例](images/combo_box_listitemstate.png)

如果组合框中的集合足够长，将显示滚动栏来容纳它。 以逻辑方式对列表中的项进行分组。

![下拉列表中的滚动栏示例](images/combo_box_scroll.png)

## <a name="create-a-combo-box"></a>创建一个组合框

通过直接向添加对象来填充组合框[项](/uwp/api/windows.ui.xaml.controls.itemscontrol.items)集合或通过将绑定[ItemsSource](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource)到数据源的属性。 添加到组合框项包装在[ComboBoxItem](/uwp/api/windows.ui.xaml.controls.comboboxitem)容器。

下面是带有 XAML 以添加的项的简单组合框。

```xaml
<ComboBox Header="Colors" PlaceholderText="Pick a color" Width="200">
    <x:String>Blue</x:String>
    <x:String>Green</x:String>
    <x:String>Red</x:String>
    <x:String>Yellow</x:String>
</ComboBox>
```

下面的示例演示如何将组合框绑定到的 FontFamily 对象的集合。

```xaml
<ComboBox x:Name="FontsCombo" Header="Fonts" Height="44" Width="296"
          ItemsSource="{x:Bind fonts}" DisplayMemberPath="Source"/>
```

```csharp
ObservableCollection<FontFamily> fonts = new ObservableCollection<FontFamily>();

public MainPage()
{
    this.InitializeComponent();
    fonts.Add(new FontFamily("Arial"));
    fonts.Add(new FontFamily("Courier New"));
    fonts.Add(new FontFamily("Times New Roman"));
}
```

### <a name="item-selection"></a>项目选择

例如 ListView、 GridView、 组合框派生自[选择器](/uwp/api/windows.ui.xaml.controls.primitives.selector)，因此可以在相同的标准方式获取用户的选择。

你可以获取或设置组合框使用选定的项[SelectedItem](/uwp/api/windows.ui.xaml.controls.primitives.selector.selecteditem)属性，并获取或设置通过使用所选的项的索引[SelectedIndex](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedindex)属性。

若要在所选的数据项上获取特定属性的值，可以使用[SelectedValue](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedvalue)属性。 在这种情况下，设置[SelectedValuePath](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedvaluepath)以指定要获取的值的选定项上的属性。

> [!TIP]
> 如果设置了 SelectedItem 或 SelectedIndex，以指示默认选择，如果该属性设置填充组合框项集合之前发生异常。 在 XAML 中定义你的项，除非它是处理组合框的 Loaded 的事件，并设置 SelectedItem 或 SelectedIndex Loaded 的事件处理程序中。

可以将绑定到 XAML 中的这些属性或处理[SelectionChanged](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged)事件对所选内容更改进行响应。

在事件处理程序代码时，可以获取从所选的项[SelectionChangedEventArgs.AddedItems](/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.addeditems)属性。 你可以获取之前选择的项 （如果有） 从[SelectionChangedEventArgs.RemovedItems](/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.removeditems)属性。 AddedItems 和 RemovedItems 集合每个包含只有 1 项，因为组合框不支持多个选择。

此示例演示如何处理 SelectionChanged 事件，以及如何绑定到所选的项。

```xaml
<StackPanel>
    <ComboBox x:Name="colorComboBox" Width="200"
              Header="Colors" PlaceholderText="Pick a color"
              SelectionChanged="ColorComboBox_SelectionChanged">
        <x:String>Blue</x:String>
        <x:String>Green</x:String>
        <x:String>Red</x:String>
        <x:String>Yellow</x:String>
    </ComboBox>

    <Rectangle x:Name="colorRectangle" Height="30" Width="100"
               Margin="0,8,0,0" HorizontalAlignment="Left"/>

    <TextBlock Text="{x:Bind colorComboBox.SelectedIndex, Mode=OneWay}"/>
    <TextBlock Text="{x:Bind colorComboBox.SelectedItem, Mode=OneWay}"/>
</StackPanel>
```

```csharp
private void ColorComboBox_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    // Add "using Windows.UI;" for Color and Colors.
    string colorName = e.AddedItems[0].ToString();
    Color color;
    switch (colorName)
    {
        case "Yellow":
            color = Colors.Yellow;
            break;
        case "Green":
            color = Colors.Green;
            break;
        case "Blue":
            color = Colors.Blue;
            break;
        case "Red":
            color = Colors.Red;
            break;
    }
    colorRectangle.Fill = new SolidColorBrush(color);
}
```

#### <a name="selectionchanged-and-keyboard-navigation"></a>SelectionChanged 和键盘导航

默认情况下，当用户单击、 点击，或按项上的 Enter 键提交其选择列表中，组合框关闭，SelectionChanged 事件时发生。 当用户使用键盘箭头键导航打开组合框列表时，不会更改所选内容。

若要使一个组合框的"实时更新"，而用户在使用箭头键 （如字体选择下拉列表） 导航打开列表，请设置[SelectionChangedTrigger](/uwp/api/windows.ui.xaml.controls.combobox.selectionchangedtrigger)到[始终](/uwp/api/windows.ui.xaml.controls.comboboxselectionchangedtrigger)。 这将导致 SelectionChanged 事件发生时焦点更改到另一个项，打开列表中。

#### <a name="selected-item-behavior-change"></a>选定的项的行为更改

在 Windows 10，版本 1809年 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 或更高版本，更新选定项的行为以支持可编辑组合框。

在 SDK 17763，SelectedItem 属性的值之前 (因此 SelectedValue 和 SelectedIndex) 需要出现在组合框的项集合。 使用上述示例中，设置`colorComboBox.SelectedItem = "Pink"`导致：

- SelectedItem = null
- SelectedValue = null
- SelectedIndex =-1

在 SDK 17763 及更高版本，SelectedItem 属性的值 (因此 SelectedValue 和 SelectedIndex) 不需要出现在组合框的项集合。 使用上述示例中，设置`colorComboBox.SelectedItem = "Pink"`导致：

- SelectedItem = Pink
- SelectedValue = Pink
- SelectedIndex =-1

### <a name="text-search"></a>文本搜索

组合框自动支持其集合内的搜索。 当焦点位于打开或关闭的组合框上时，如果用户在物理键盘上键入字符，与用户的字符串匹配的候选项将引入视图。 当在长列表中导航时，此功能尤其有用。 例如时与下拉列表包含状态的列表进行交互，, 用户可以按"w"键来快速选择将"Washington"放入视图。 文本搜索不区分大小写。

可以设置[IsTextSearchEnabled](/uwp/api/windows.ui.xaml.controls.combobox.istextsearchenabled)属性设置为**false**若要禁用此功能。

## <a name="make-a-combo-box-editable"></a>使组合框可编辑

> [!IMPORTANT]
> 此功能需要 Windows 10，版本 1809年 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 或更高版本。

默认情况下，组合框允许用户选择从预定义列表的选项。 但是，一些情况下，列表中包含有效的值的一个子集，并且用户应该能够输入未列出的其他值。 若要支持此功能，可以使组合框可编辑。

若要使组合框可编辑，将设置[IsEditable](/uwp/api/windows.ui.xaml.controls.combobox.iseditable)属性设置为**true**。 然后，处理[TextSubmitted](/uwp/api/Windows.UI.Xaml.Controls.ComboBox)事件以使用用户输入的值。

默认情况下，当用户提交自定义文本时更新 SelectedItem 值。 可以通过设置替代此行为**Handled**到**true** TextSubmitted 事件参数中。 在事件标记为已处理时，组合框需要任何进一步的操作事件发生后，将处于编辑状态。 将不会更新 SelectedItem。

此示例演示一个简单的可编辑组合框。 列表中包含的简单字符串，并且用户输入的任何值用作输入。

"最近使用的名称"选择器使用户可以输入自定义字符串。 RecentlyUsedNames 列表包含一些值，用户可供选择的但用户还可以添加一个新的自定义值。 CurrentName 属性表示当前输入的名称。

```xaml
<ComboBox IsEditable="true"
          ItemsSource="{x:Bind RecentlyUsedNames}"
          SelectedItem="{x:Bind CurrentName, Mode=TwoWay}"/>
```

### <a name="text-submitted"></a>提交文本

您可以处理[TextSubmitted](/uwp/api/Windows.UI.Xaml.Controls.ComboBox)事件以使用用户输入的值。 事件处理程序中，您将通常验证用户输入的值有效，然后在应用中使用的值。 根据具体情况，您可能会将值添加到以供将来使用的选项的组合框的列表。

TextSubmitted 事件发生时满足这些条件：

- IsEditable 属性是 **，则返回 true**
- 用户输入与组合框列表中的现有条目不匹配的文本
- 用户按下 enter 键，或将焦点移从组合框。

如果用户输入文本，然后导航列表中向上或向下，TextSubmitted 事件不会发生。

### <a name="sample---validate-input-and-use-locally"></a>示例-验证输入，并使用本地

在此示例中，字体大小选择器包含一组值对应于字体大小负载增加，但用户可以输入不在列表中的字体大小。

当用户将添加一个值，不在列表中，字体大小更新操作，但值不添加到字体大小的列表。

如果新输入的值不是有效的使用到 SelectedValue，若要还原的 Text 属性的最后一个已知的正确值。

```xaml
<ComboBox x:Name="fontSizeComboBox"
          IsEditable="true"
          ItemsSource="{x:Bind ListOfFontSizes}"
          TextSubmitted="FontSizeComboBox_TextSubmitted"/>
```

```csharp
private void FontSizeComboBox_TextSubmitted(ComboBox sender, ComboBoxTextSubmittedEventArgs e)
{
    if (byte.TryParse(e.Text, out double newValue))
    {
        // Update the app’s font size.
        _fontSize = newValue;
    }
    else
    {
        // If the item is invalid, reject it and revert the text.
        // Mark the event as handled so the framework doesn’t update the selected item.
        sender.Text = sender.SelectedValue.ToString();
        e.Handled = true;
    }
}
```

### <a name="sample---validate-input-and-add-to-list"></a>示例-验证输入并将添加到列表

在这里，"喜爱的颜色选择器"中包括最常用的收藏颜色 （红色、 蓝色、 绿色、 橙色），但用户可以输入不在列表中的最喜欢的颜色。 当用户添加有效的颜色 （如粉红色） 时，新输入的颜色添加到列表，并设置为活动"favorite color"。

```xaml
<ComboBox x:Name="favoriteColorComboBox"
          IsEditable="true"
          ItemsSource="{x:Bind ListOfColors}"
          TextSubmitted="FavoriteColorComboBox_TextSubmitted"/>
```

```csharp
private void FavoriteColorComboBox_TextSubmitted(ComboBox sender, ComboBoxTextSubmittedEventArgs e)
{
    if (IsValid(e.Text))
    {
        FavoriteColor newColor = new FavoriteColor()
        {
            ColorName = e.Text,
            Color = ColorFromStringConverter(e.Text)
        }
        ListOfColors.Add(newColor);
    }
    else
    {
        // If the item is invalid, reject it but do not revert the text.
        // Mark the event as handled so the framework doesn’t update the selected item.
        e.Handled = true;
    }
}

bool IsValid(string Text)
{
    // Validate that the string is: not empty; a color.
}
```

## <a name="dos-and-donts"></a>应做事项和禁止事项

- 将组合框的文本内容限制为单行。
- 以最合乎逻辑的顺序对组合框中的项进行排序。 将相关选项组合到一起并将最常见的选项置于顶部。 按字母顺序对名称进行排序、按数字顺序对数字进行排序，并按时间先后顺序对日期进行排序。

## <a name="get-the-sample-code"></a>获取示例代码

- [XAML 控件库示例](https://github.com/Microsoft/Xaml-Controls-Gallery) - 以交互式格式查看所有 XAML 控件。
- [AutoSuggestBox 示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlAutoSuggestBox)

## <a name="related-articles"></a>相关文章

- [文本控件](text-controls.md)
- [拼写检查](text-controls.md)
- [搜索](search.md)
- [TextBox 类](https://msdn.microsoft.com/library/windows/apps/br209683)
- [Windows.UI.Xaml.Controls PasswordBox 类](https://msdn.microsoft.com/library/windows/apps/br227519)
- [String.Length 属性](https://msdn.microsoft.com/library/system.string.length.aspx)
