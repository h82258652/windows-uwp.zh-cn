---
Description: 在用户键入时提供建议的文本输入框。
title: 组合框和列表框
label: Combo box and list box
template: detail.hbs
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: ''
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 31b3bcc2388a98941fc5e8aa44d18beee53de5c7
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "80081100"
---
# <a name="combo-box-and-list-box"></a>组合框和列表框

使用组合框（也称下拉列表）可提供一个项列表供用户选择。 组合框最初处于紧凑状态，可展开以显示可选择项的列表。 ListBox 类似于组合框，但不可折叠，也没有紧凑状态。 可以在本文末尾详细了解列表框。

组合框在关闭后，会显示当前的选择或为空（如果没有选中项）。 当用户展开组合框时，它会显示可选择项的列表。

![处于紧凑状态的下拉列表示例](images/combo_box_collapsed.png)

> 处于紧凑状态的组合框，显示一个标题。 

**获取 Windows UI 库**

|  |  |
| - | - |
| ![WinUI 徽标](images/winui-logo-64x64.png) | Windows UI 库 2.2 或更高版本包含此控件的使用圆角的新模板。 有关详细信息，请参阅[圆角半径](/windows/uwp/design/style/rounded-corner)。 WinUI 是一种 NuGet 包，其中包含用于 UWP 应用的新控件和 UI 功能。 有关详细信息（包括安装说明），请参阅 [Windows UI 库](https://docs.microsoft.com/uwp/toolkits/winui/)。 |

> **平台 API：** [ComboBox 类](/uwp/api/Windows.UI.Xaml.Controls.ComboBox)、[IsEditable 属性](/uwp/api/windows.ui.xaml.controls.combobox.iseditable)、[Text 属性](/uwp/api/Windows.UI.Xaml.Controls.ComboBox)、[TextSubmitted 事件](/uwp/api/Windows.UI.Xaml.Controls.ComboBox)

## <a name="is-this-the-right-control"></a>这是正确的控件吗？

- 使用下拉列表可让用户从一组项中选择单个值，这些项可使用单行文本完全显示。
- 使用列表视图或网格视图（而非组合框），显示包含多行文本或多张图像的项。
- 当少于五项时，应考虑使用[单选按钮](radio-button.md)（当进行单项选择时）或[复选框](checkbox.md)（当进行多项选择时）。
- 当选择项在应用流中不太重要时使用组合框。 如果在多数情况下为大部分用户推荐了默认选项，则通过使用列表视图显示所有项可能会导致用户过多地注意这些选项。 使用组合框可以节省空间并减少分心。

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果已安装 <strong style="font-weight: semi-bold">XAML 控件库</strong>应用，请单击此处<a href="xamlcontrolsgallery:/item/ComboBox">打开应用，了解 ComboBox 的实际操作</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">获取 XAML 控件库应用 (Microsoft Store)</a></li>
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

## <a name="create-a-combo-box"></a>创建组合框

填充组合框时，可以将对象直接添加到[项](/uwp/api/windows.ui.xaml.controls.itemscontrol.items)集，也可以将 [ItemsSource](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) 属性绑定到数据源。 添加到 ComboBox 的项会包装到 [ComboBoxItem](/uwp/api/windows.ui.xaml.controls.comboboxitem) 容器中。

下面是一个简单的组合框，已在 XAML 中添加项。

```xaml
<ComboBox Header="Colors" PlaceholderText="Pick a color" Width="200">
    <x:String>Blue</x:String>
    <x:String>Green</x:String>
    <x:String>Red</x:String>
    <x:String>Yellow</x:String>
</ComboBox>
```

以下示例演示了如何将组合框绑定到 FontFamily 对象的集合。

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

### <a name="item-selection"></a>项选择

与 ListView 和 GridView 一样，ComboBox 派生自 [Selector](/uwp/api/windows.ui.xaml.controls.primitives.selector)，因此你可以采用同一标准方式获取用户的选择。

可以通过 [SelectedItem](/uwp/api/windows.ui.xaml.controls.primitives.selector.selecteditem) 属性获取或设置组合框的所选项，以及通过 [SelectedIndex](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedindex) 属性获取或设置所选项的索引。

若要获取所选数据项的特定属性的值，可以使用 [SelectedValue](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedvalue) 属性。 在这种情况下，可以设置 [SelectedValuePath](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedvaluepath)，以便指定要从所选项的哪个属性获取值。

> [!TIP]
> 如果通过设置 SelectedItem 或 SelectedIndex 来指示默认选择，并且是在填充组合框项集合之前设置该属性，则会出现异常。 除非是在 XAML 中定义项，否则最好是处理组合框的 Loaded 事件，并在 Loaded 事件处理程序中设置 SelectedItem 或 SelectedIndex。

可以在 XAML 中绑定到这些属性，或者通过处理 [SelectionChanged](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) 事件来响应选择变化。

在事件处理程序代码中，可以从 [SelectionChangedEventArgs.AddedItems](/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.addeditems) 属性获取所选项。 可以从 [SelectionChangedEventArgs.RemovedItems](/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.removeditems) 属性获取以前选择的项（如果有）。 AddedItems 和 RemovedItems 集合都只包含 1 个项，因为组合框不支持多选。

此示例显示了如何处理 SelectionChanged 事件，以及如何绑定到所选项。

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

默认情况下，当用户单击、点击列表中的项或对其按 Enter 以提交所做的选择时，就会发生 SelectionChanged 事件，然后组合框会关闭。 当用户使用键盘箭头键导航打开的组合框列表时，不会改变所做的选择。

若要创建可在用户使用箭头键导航打开的列表时实时更新的组合框（如字体选择下拉列表），请将 [SelectionChangedTrigger](/uwp/api/windows.ui.xaml.controls.combobox.selectionchangedtrigger) 设置为 [Always](/uwp/api/windows.ui.xaml.controls.comboboxselectionchangedtrigger)。 这样，当焦点转到开放列表中的另一项时，就会发生 SelectionChanged 事件。

#### <a name="selected-item-behavior-change"></a>所选项行为变化

在 Windows 10 版本 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 或更高版本中，选定项的行为已更新，支持可编辑的组合框。

在 SDK 17763 之前，SelectedItem 属性（以及相应的 SelectedValue 和 SelectedIndex）的值必须位于组合框的项集中。 使用上一示例时，设置 `colorComboBox.SelectedItem = "Pink"` 会导致：

- SelectedItem = null
- SelectedValue = null
- SelectedIndex = -1

在 SDK 17763 及更高版本中，SelectedItem 属性（以及相应的 SelectedValue 和 SelectedIndex）的值不需位于组合框的项集中。 使用上一示例时，设置 `colorComboBox.SelectedItem = "Pink"` 会导致：

- SelectedItem = Pink
- SelectedValue = Pink
- SelectedIndex = -1

### <a name="text-search"></a>文本搜索

组合框自动支持其集合内的搜索。 当焦点位于打开或关闭的组合框上时，如果用户在物理键盘上键入字符，与用户的字符串匹配的候选项将引入视图。 当在长列表中导航时，此功能尤其有用。 例如，当与包含状态列表的下拉列表交互时，用户可以按“w”键来将“Washington”引入视图，以供快速选择。 文本搜索不区分大小写。

可以将 [IsTextSearchEnabled](/uwp/api/windows.ui.xaml.controls.combobox.istextsearchenabled) 属性设置为 **false** 以禁用此功能。

## <a name="make-a-combo-box-editable"></a>使组合框变得可编辑

> [!IMPORTANT]
> 此功能需要 Windows 10 版本 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 或更高版本。

默认情况下，组合框允许用户从预定义的选项列表中进行选择。 但有时候，列表仅包含一部分有效值，因此应该允许用户输入未列出的其他值。 为了支持该功能，可以让组合框变得可编辑。

若要让组合框变得可编辑，请将 [IsEditable](/uwp/api/windows.ui.xaml.controls.combobox.iseditable) 属性设置为 **true**。 然后处理 [TextSubmitted](/uwp/api/Windows.UI.Xaml.Controls.ComboBox) 事件，以便处理用户输入的值。

默认情况下，当用户提交自定义文本时，SelectedItem 值就会更新。 可以在 TextSubmitted 事件参数中将 **Handled** 设置为 **true**，以便覆盖此行为。 将事件标记为“已处理”后，组合框就不会在事件之后执行进一步的操作，会停留在“正在编辑”状态。 SelectedItem 将不会更新。

此示例显示了一个简单的可编辑组合框。 此列表包含简单的字符串，用户输入的任何值都会用作已输入值。

“最近使用的名称”选择器允许用户输入自定义字符串。 “RecentlyUsedNames”列表包含可供用户选择的一些值，但用户也可添加新的自定义值。 “CurrentName”属性表示当前输入的名称。

```xaml
<ComboBox IsEditable="true"
          ItemsSource="{x:Bind RecentlyUsedNames}"
          SelectedItem="{x:Bind CurrentName, Mode=TwoWay}"/>
```

### <a name="text-submitted"></a>提交的文本

可以处理 [TextSubmitted](/uwp/api/Windows.UI.Xaml.Controls.ComboBox) 事件，以便处理用户输入的值。 在事件处理程序中，通常需验证用户输入的值是否有效，然后即可在应用中使用该值。 也可根据情况将值添加到组合框的选项列表中供将来使用。

当以下条件满足时，会发生 TextSubmitted 事件：

- IsEditable 属性为 **true**
- 用户输入的文本与组合框列表中的现有条目不匹配
- 用户按了 Enter，或已将焦点从组合框中移开。

如果用户在输入文本后又通过列表进行上下导航，则不会发生 TextSubmitted 事件。

### <a name="sample---validate-input-and-use-locally"></a>示例 - 验证输入并在本地使用

在此示例中，字体大小选择器包含与字体大小渐变相对应的一组值，但用户可输入不在列表中的字体大小。

当用户添加不在列表中的值时，字体大小会更新，但值不会添加到字体大小列表中。

如果新输入的值无效，则请使用 SelectedValue 将 Text 属性还原为上一个已知有效的值。

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
        // Update the app's font size.
        _fontSize = newValue;
    }
    else
    {
        // If the item is invalid, reject it and revert the text.
        // Mark the event as handled so the framework doesn't update the selected item.
        sender.Text = sender.SelectedValue.ToString();
        e.Handled = true;
    }
}
```

### <a name="sample---validate-input-and-add-to-list"></a>示例 - 验证输入并将其添加到列表

在这里，“最喜爱颜色选择器”包含通常情况下人们最喜爱的颜色（红、蓝、绿、橙），但用户可以输入不在列表中的最喜爱的颜色。 当用户添加有效颜色（例如粉红色）时，新输入的颜色会添加到列表中，并被设置为有效的“最喜爱颜色”。

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
        // Mark the event as handled so the framework doesn't update the selected item.
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

## <a name="list-boxes"></a>列表框

列表框允许用户从集合中选择单个项或多个项。 列表框类似于下拉列表，区别在于列表框始终处于打开状态，并且不存在紧凑式状态的列表框（不可进行展开）。 当没有空间来显示所有内容时，可滚动列表中的项。

### <a name="is-this-the-right-control"></a>这是正确的控件吗？

- 当列表中的项重要到需突出显示时，以及当没有足够的屏幕空间可用来显示整个列表时，列表框会很有用。
- 列表框应将用户的注意力吸引到一项重要选择的全套备选项。 相比之下，下拉列表的设计初衷则是将用户的注意力吸引到某一选定项。
- 当出现以下情况时，应避免使用列表框：
    - 列表中的项数非常少。 始终具有两个相同选项的单选列表框可能更适合呈现为[单选按钮](radio-button.md)。 当列表中有 3 到 4 个静态项时，也可考虑使用单选按钮。
    - 列表框为单选式且始终具有两个相同选项，其中一个可表示为另一个的对立面，例如“打开”和“关闭”。 使用单个复选框或切换开关。
    - 项目数非常多。 网格视图和列表视图更适用于较长列表。 对于很长的分组数据列表，首选语义式缩放。
    - 项为相邻的数字值。 如果是这种情况，应考虑使用[滑块](slider.md)。
    - 选择项在你的应用流中不太重要，或者说对于大部分情况下的大部分用户，建议使用默认选项。 改用下拉列表。

### <a name="recommendations"></a>建议

- 在列表框中，项数的理想范围为 3 到 9 个。
- 当列表框中的项可以动态变化时，其效果颇佳。
- 如果可以，请设置列表框的大小，这样便无需平移或滚动其项列表。
- 验证列表框的目的以及当前选择的项目是否明确。
- 保留用于触摸反馈和选择的项目状态的视觉效果和动画。
- 将列表框项目的文本内容限制为单行。 如果项是视觉对象，你可以自定义其大小。 如果项包括多行文本或多个图像，请改用网格视图或列表视图。
- 使用默认字体，除非你的品牌指南指示使用其他字体。
- 不要使用列表框执行命令或者动态显示或隐藏其他控件。

## <a name="get-the-sample-code"></a>获取示例代码

- [XAML 控件库示例](https://github.com/Microsoft/Xaml-Controls-Gallery) - 以交互式格式查看所有 XAML 控件。
- [AutoSuggestBox 示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlAutoSuggestBox)

## <a name="related-articles"></a>相关文章

- [文本控件](text-controls.md)
- [拼写检查](text-controls.md)
- [搜索](search.md)
- [TextBox class](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox)（TextBox 类）
- [Windows.UI.Xaml.Controls PasswordBox 类](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.PasswordBox)
- [String.Length 属性](https://docs.microsoft.com/dotnet/api/system.string.length)
