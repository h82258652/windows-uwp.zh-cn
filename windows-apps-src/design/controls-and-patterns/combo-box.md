---
author: Jwmsft
Description: A text entry box that provides suggestions as the user types.
title: 组合框 （下拉列表）
label: Combo box
template: detail.hbs
ms.author: jimwalk
ms.date: 10/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: ''
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 518ce49ddb631e3e914a6c7662b4e74de247c29c
ms.sourcegitcommit: 310a4555fedd4246188a98b31f6c094abb33ec60
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/19/2018
ms.locfileid: "5126714"
---
# <a name="combo-box"></a>组合框

使用组合框 （也称为下拉列表） 显示用户可以从中进行选择的项的列表。 组合框启动处于紧凑状态，并展开以显示可选项目的列表。

当关闭组合框时，它显示当前选择或者如果没有选定项为空。 当用户扩展的组合框时，它将显示可选项目的列表。

> **重要 Api**: [ComboBox 类](/uwp/api/Windows.UI.Xaml.Controls.ComboBox)、 [IsEditable 属性](/uwp/api/windows.ui.xaml.controls.combobox.iseditable)、 [Text 属性](/uwp/api/Windows.UI.Xaml.Controls.ComboBox)、 [TextSubmitted 事件](/uwp/api/Windows.UI.Xaml.Controls.ComboBox)

处于紧凑状态的标头组合框。

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
    <p>如果你安装了该<strong style="font-weight: semi-bold">XAML 控件库</strong>应用，单击此处<a href="xamlcontrolsgallery:/item/ComboBox">打开此应用，请参阅的实际组合框</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">获取源代码 (GitHub)</a></li>
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

通过将对象添加到[项](/uwp/api/windows.ui.xaml.controls.itemscontrol.items)集合直接或通过将[ItemsSource](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource)属性绑定到数据源，你可以填充组合框。 添加到组合框项被包装在[ComboBoxItem](/uwp/api/windows.ui.xaml.controls.comboboxitem)容器。

下面是在 XAML 中添加项目的简单组合框。

```xaml
<ComboBox Header="Colors" PlaceholderText="Pick a color" Width="200">
    <x:String>Blue<x:String>
    <x:String>Green<x:String>
    <x:String>Red<x:String>
    <x:String>Yellow<x:String>
</ComboBox>
```

以下示例演示了将组合框绑定到 FontFamily 对象的集合。

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

ListView 和 GridView，如组合框派生自[选择器](/uwp/api/windows.ui.xaml.controls.primitives.selector)，以便你可以获取用户的选择标准方式相同。

你可以获取或设置组合框的选中项通过使用[SelectedItem](/uwp/api/windows.ui.xaml.controls.primitives.selector.selecteditem)属性，和获取或设置选定的项目的索引使用[SelectedIndex](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedindex)属性。

若要获取选定的数据项上的特定属性的值，你可以使用[SelectedValue](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedvalue)属性。 在此情况下，设置[SelectedValuePath](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedvaluepath)在所选项目，以获取将该值从上指定的属性。

> [!TIP]
> 如果你设置 SelectedItem 或 SelectedIndex 以指示默认选择，如果属性设置为之前填充组合框项集合发生异常。 除非你在 XAML 中定义你的项，最好是处理组合框已加载的事件，并将 SelectedItem 或 SelectedIndex 设置 Loaded 的事件处理程序中。

你可以将绑定到在 XAML 中，这些属性或处理[SelectionChanged](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged)事件来响应选择更改。

在事件处理程序代码中，你可以从[SelectionChangedEventArgs.AddedItems](/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.addeditems)属性获取选定的项目。 你可以从[SelectionChangedEventArgs.RemovedItems](/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.removeditems)属性获取先前选定的项 （如果有）。 否则 AddedItems 和 RemovedItems 集合每个包含只有 1 个项目，因为组合框不支持多个选择。

此示例显示如何处理 SelectionChanged 事件，以及如何将绑定到所选项目。

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

默认情况下，当用户点击量、 点击时，或按某个项目在列表中确认其所选内容，并组合框关闭，则 SelectionChanged 事件发生。 当用户通过键盘箭头键导航开放式组合框列表，选择保持不变。

若要使时的组合框"动态更新"用户正在通过箭头键 （如字体所选内容下拉列表） 进行导航打开列表，请将[SelectionChangedTrigger](/uwp/api/windows.ui.xaml.controls.combobox.selectionchangedtrigger)设置为[始终](/uwp/api/windows.ui.xaml.controls.comboboxselectionchangedtrigger)。 这将导致发生焦点更改为另一个打开的列表项目时 SelectionChanged 事件。

#### <a name="selected-item-behavior-change"></a>选定的项行为更改

在 RS5 (Windows SDK 版本 10.0.NNNNN.0 (Windows 10，版本 YYMM) 选定项的行为将更新以支持可编辑的组合框。

之前 RS5，SelectedItem 属性的值 (并因此，SelectedValue 和 SelectedIndex) 需要进行组合框的项目集合中。 使用前面示例中，设置`colorComboBox.SelectedItem = "Pink"`导致：

- SelectedItem = null
- SelectedValue = null
- SelectedIndex =-1

在 RS5 及更高版本，SelectedItem 属性的值 (并因此，SelectedValue 和 SelectedIndex) 不需要是组合框的项目集合中。 使用前面示例中，设置`colorComboBox.SelectedItem = "Pink"`导致：

- SelectedItem = 粉红色
- SelectedValue = 粉红色
- SelectedIndex =-1

### <a name="text-search"></a>文本搜索

组合框自动支持其集合内的搜索。 当焦点位于打开或关闭的组合框上时，如果用户在物理键盘上键入字符，与用户的字符串匹配的候选项将引入视图。 当在长列表中导航时，此功能尤其有用。 例如，当与包含状态列表的下拉列表交互，用户可以按"w"键可以将"Washington"引入视图以供快速选择。 文本搜索不区分大小写。

你可以将[IsTextSearchEnabled](/uwp/api/windows.ui.xaml.controls.combobox.istextsearchenabled)属性设置为**false**来禁用此功能。

## <a name="make-a-combo-box-editable"></a>使组合框可编辑

> [!IMPORTANT]
> 此功能需要的[最新的 Windows 10 Insider Preview 版本和 SDK](https://insider.windows.com/for-developers/)。

默认情况下，一个组合框让用户从预定义的选项列表中进行选择。 但是，有列表仅包含一个子集的有效的值，并且用户应该能够输入未列出的其他值的情况。 若要支持这一点，你可以进行组合框可编辑。

若要使组合框可编辑，请将[IsEditable](/uwp/api/windows.ui.xaml.controls.combobox.iseditable)属性设置为**true**。 然后，处理[TextSubmitted](/uwp/api/Windows.UI.Xaml.Controls.ComboBox)事件来处理用户输入的值。

默认情况下，当用户提交自定义文本更新 SelectedItem 值。 你可以通过将**Handled**设置为**true**在 TextSubmitted 事件参数中替代此行为。 会将事件标记为已处理，当组合框将不执行任何进一步的操作事件之后，并将保持正在编辑的状态。 SelectedItem 将不会更新。

此示例显示了一个简单的可编辑组合框。 该列表包含简单字符串和用户输入的任何值用作输入。

"最近使用的名称"的选择器使用户可以输入自定义字符串。 RecentlyUsedNames 列表中包含用户可以选择从，某些值，但用户还可以添加一个新的自定义的值。 CurrentName 属性表示当前输入的名称。

```xaml
<ComboBox IsEditable="true"
          ItemsSource="{x:Bind RecentlyUsedNames}"
          SelectedItem="{x:Bind CurrentName, Mode=TwoWay}"/>
```

### <a name="text-submitted"></a>提交的文本

你可以处理[TextSubmitted](/uwp/api/Windows.UI.Xaml.Controls.ComboBox)事件，以便与用户输入的值。 在事件处理程序中，你将通常验证用户输入的值是有效，然后在应用中使用的值。 根据情况，你可能会将值添加到组合框的选项列表中以供将来使用。

当满足这些条件，则 TextSubmitted 事件发生：

- IsEditable 属性是**true**
- 用户输入的不匹配组合框列表中的现有输入的文本
- 用户按 Enter，或将焦点移从组合框。

如果用户输入文本，然后在列表中导航向上或向下不会发生 TextSubmitted 事件。

### <a name="sample---validate-input-and-use-locally"></a>示例-验证输入和本地使用

在此 examle，字体大小选择器包含一组值对应于字体大小渐变，但用户可以输入不在列表中的字体大小。

当用户将添加一个值，不在列表、 字体大小更新，但的值不添加到列表中的字体大小。

如果新输入的值不是有效的你使用 SelectedValue 将 Text 属性恢复到最后一个已知良好的值。

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

### <a name="sample---validate-input-and-add-to-list"></a>示例-验证输入，并将添加到列表

在这里，"最喜爱的颜色选择器"包含最常见最喜爱的颜色 （红色、 蓝色、 绿色、 Orange），但用户可以输入最喜爱的颜色为不在列表中。 当用户添加 （如粉红色） 是有效的颜色时，新输入的颜色添加到列表，并设置为活动"最喜爱的颜色"。

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

- [XAML 控件库示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 以交互式格式查看所有 XAML 控件。
- [AutoSuggestBox 示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlAutoSuggestBox)

## <a name="related-articles"></a>相关文章

- [文本控件](text-controls.md)
- [拼写检查](text-controls.md)
- [搜索](search.md)
- [TextBox 类](https://msdn.microsoft.com/library/windows/apps/br209683)
- [Windows.UI.Xaml.Controls PasswordBox 类](https://msdn.microsoft.com/library/windows/apps/br227519)
- [String.Length 属性](https://msdn.microsoft.com/library/system.string.length.aspx)
