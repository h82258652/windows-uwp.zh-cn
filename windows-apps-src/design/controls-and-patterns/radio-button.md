---
Description: 单选按钮允许用户从两个或多个选项中选择一个选项。
title: 单选按钮指南
ms.assetid: 41E3F928-AA55-42A2-9281-EC3907C4F898
label: Radio buttons
template: detail.hbs
ms.date: 06/10/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 6705c314d9a70f8b6282841a7f8b1df76c6ef880
ms.sourcegitcommit: 6dd6d61c912daab2cc4defe5ba0cf717339f7765
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/18/2020
ms.locfileid: "84978399"
---
# <a name="radio-buttons"></a>单选按钮

单选按钮允许用户从包含两个或更多互斥但相关的选项的集合中选择一个选项。 每个选项由一个单选按钮表示。

默认状态下，不会选择组中的单选按钮。 但是，用户选择单选按钮选项后，无法还原组的未选中状态。

单选按钮组的单一行为将其与[复选框](checkbox.md)区分开来，后者支持多选和取消选择。

![单选按钮](images/controls/radio-button.png)

**获取 Windows UI 库**

|  |  |
| - | - |
| ![WinUI 徽标](images/winui-logo-64x64.png) | “RadioButtons”控件作为 Windows UI 库的一部分提供，该库是一个 NuGet 包，其中包含用于 Windows 应用的新控件和 UI 功能。 有关详细信息（包括安装说明），请参阅 [Windows UI 库](https://docs.microsoft.com/uwp/toolkits/winui/)。 |

> **Windows UI 库 API：** [RadioButtons 类](/uwp/api/microsoft.ui.xaml.controls.radiobuttons)、[SelectionChanged 事件](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectionchanged)、[SelectedItem 属性](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selecteditem)、[SelectedIndex 属性](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectedindex)
>
> **平台 API：** [RadioButton 类](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RadioButton)、[Checked 事件](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.Checked)、[IsChecked 属性](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.IsChecked)

## <a name="is-this-the-right-control"></a>这是正确的控件吗？

使用单选按钮可向用户提供两个或更多互斥的选项。

![一组单选按钮](images/radiobutton_basic.png)

当用户需要查看所有选项来进行选择时可以使用单选按钮。 因为单选按钮平等地强调所有选项，所以可以引起对这些选项的更多关注。 除非所有选项都值得用户平等关注，否则请考虑使用其他控件。 例如，如果在大部分情况下为大多数用户推荐默认选项，请改用[组合框](combo-box.md)。

![用于强调默认选项的下拉列表](images/combo_box_collapsed.png)

如果仅有两个互斥的选项，请将其合并到单个[复选框](checkbox.md)或[切换开关](toggles.md)。 例如，使用复选框“我同意”而不是两个单选按钮“我同意”和“我不同意”。

![复选框是展示二项选择的好方法](images/radiobutton_vs_checkbox.png)

当用户可以选择多个选项时，请使用[复选框](checkbox.md).

![复选框支持多选](images/checkbox2.png)

当选项是具有固定步长的数字（10、20、30）时，请使用[滑块](slider.md)控件。

![用于选择步进值的滑块](images/controls/slider.png)

如果存在超过 8 个选项，请使用[组合框或列表框](combo-box.md)。

![用于显示多个选项的列表框](images/combo_box_scroll.png)

> [!NOTE]
> 如果可用选项基于应用的当前上下文或可以根据其他条件动态变化，请改用单选[列表框](combo-box.md#list-boxes)。

## <a name="radiobuttons-behavior"></a>RadioButtons 行为

键盘访问和导航行为已在 [RadioButton](/uwp/api/windows.ui.xaml.controls.radiobutton?view=winrt-19041) 组中进行了优化，有助于辅助功能和键盘电源用户更加快速轻松地导航选项列表。

除了键盘快捷方式和辅助功能改进之外，RadioButton 组中各个单选按钮的默认可视布局也通过自动化方向、间距和边距设置进行了优化。 这样就无需指定这些属性，而在使用更原始的组控件（例如 [StackPanel](../layout/layout-panels.md#stackpanel) 或 [Grid](../layout/layout-panels.md#grid)）时必须指定这些属性。

### <a name="navigating-a-radiobuttons-group"></a>导航 RadioButtons 组

RadioButtons 控件支持两种状态：

- 未选择/选中任何选项的 RadioButton 控件列表
- 已选择/选中一个选项的 RadioButton 控件列表

以下两部分涵盖了这两种单选按钮焦点行为。

#### <a name="item-already-selected"></a>已选择项

选择某个单选按钮后，当用户进入列表时，所选单选按钮将获得焦点。

|没有选项卡焦点的列表 | 具有初始选项卡焦点的列表 |
|:--:|:--:|
| ![没有选项卡焦点的列表](images/radiobutton-selected-item-no-tab-focus.png) | ![具有初始选项卡焦点的列表](images/radiobutton-selected-item-tab-focus.png)|

#### <a name="no-item-selected"></a>未选择任何项

如果未选择任何单选按钮，则列表中的第一个单选按钮会获得焦点。

> [!NOTE]
> 不会选择/选中从初始选项卡导航接收选项卡焦点的项。

|没有选项卡焦点的列表 | 具有初始选项卡焦点的列表|
|:--:|:--:|
| ![没有选项卡焦点的列表](images/radiobutton-no-selected-item-no-tab-focus.png) | ![具有初始选项卡焦点的列表](images/radiobutton-no-selected-item-tab-focus.png)|

### <a name="keyboard-navigation"></a>键盘导航

如果你有单行/列的单选按钮选项，并且某个项已获得选项卡焦点，则可通过箭头键在 RadioButtons 控件中的各项之间进行“内部导航”。 有关键盘导航行为的更多详细信息，请参阅[键盘交互 - 导航](../input/keyboard-interactions.md#navigation)。

对于 RadioButtons 控件，当选项列表（以独占方式）垂直排列时，可通过向上/向下键在各项之间导航，向左/向右键不起任何作用。 但是，在（以独占方式）水平排列的列表中，向左/向右和向上/向下键都可用于以相同的方式在各项之间导航。

![单列/行 RadioButton 组中键盘导航的示例](images/radiobutton-keyboard-navigation-single-column-row.png)<br/>
单列/行 RadioButton 组中键盘导航的示例

#### <a name="navigating-within-multi-columnrow-layouts"></a>在多列/行布局中导航

在列主序（项目从上到下、从左至右填充）中，当焦点位于某列的最后一个项且按下向下键时，焦点将移至下一列的第一个项。 相同的行为以相反的顺序发生：当焦点设为某列的第一个项且按下向上键时，焦点将移至上一列的最后一个项。

![多列/行 RadioButton 组中键盘导航的示例](images/radiobutton-keyboard-navigation-multi-column-row.png)

在行主序（项目从左至右、从上到下填充）中，当焦点位于某行的最后一个项并按下向右键时，焦点将移至下一行的第一个项。 相同的行为以相反的顺序发生：当焦点设为某行的第一个项且按下向左键时，焦点将移至上一行的最后一个项。

##### <a name="wrapping"></a>总结

RadioButtons 组不换行。 这是因为在使用屏幕阅读器时，会失去边界检测和清晰的开始和结束指示，这使有视力障碍的用户难以导航列表。 RadioButtons 控件也不支持枚举，因为它用于包含合理数量的项（请参阅[这是正确的控件吗？](#is-this-the-right-control)）。

## <a name="selection-follows-focus"></a>在获得焦点后选中

当使用键盘在 RadioButtons 列表中的各项之间导航（已选择其中某个项）时，当焦点从一个项移动到下一个项时，将选择/选中新获得焦点的项，并取消选择/选中之前具有焦点的项。

|键盘导航之前 | 键盘导航之后|
|:--|:--|
| ![键盘导航之前的焦点和选择的示例](images/radiobutton-two-selected-before-keyboard-navigation.png)</br>键盘导航之前的焦点和选择的示例 | ![键盘导航之后的焦点和选择的示例](images/radiobutton-three-selected-after-keyboard-navigation.png)<br/>键盘导航之后的焦点和选择的示例，其中通过向下或向右键将焦点移到“3”RadioButton，选择“3”，并取消选择“2”。

### <a name="navigating-with-xbox-gamepad-and-remote-control"></a>通过 Xbox 游戏板和遥控器导航

如果使用 Xbox 游戏板或遥控器在 RadioButtons 控件内导航，则会禁用“在获得焦点后选中”行为，并且必须按“A”按钮才能选择具有焦点的单选按钮。

## <a name="accessibility-behavior"></a>辅助功能行为

下表详细说明了讲述人如何处理单选按钮组和播放的内容（这取决于用户设置的讲述人详细首选项）。

| 初始焦点 | 焦点移动到所选项 |
|:--|:--|
| “组名”RadioButton 集合，选择了第 x 项（共 N 项） | 已选择 RadioButton“名称”，选择了第 x 项（共 N 项） |
|“组名”RadioButton 集合，未选择任何项| 未选择 RadioButton“名称”，第 x 项（共 N 项） <br> （如果使用 Shift+箭头键进行导航，则表示没有在获得焦点后选中） |

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果已安装 <strong style="font-weight: semi-bold">XAML 控件库</strong>应用，请单击此处<a href="xamlcontrolsgallery:/item/RadioButton">打开此应用，了解 RadioButton 的实际应用</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="using-the-winui-radiobuttons-control"></a>使用 WinUI RadioButtons 控件

如果使用 [WinUI](https://github.com/microsoft/microsoft-ui-xaml)，建议使用 [RadioButtons](/uwp/api/microsoft.ui.xaml.controls.radiobuttons) 控件。

RadioButtons 控件易于设置和使用，并可确保键盘和讲述人行为正确且符合预期。

此处，我们声明了包含三个选项的基本 RadioButtons 控件。

```xaml
<RadioButtons Header="App Mode" SelectedIndex="2">
    <RadioButton>Item 1</RadioButton>
    <RadioButton>Item 2</RadioButton>
    <RadioButton>Item 3</RadioButton>
</RadioButtons>
```

![两个组中的单选按钮](images/default-radiobutton-group.png)

### <a name="defining-multiple-columns"></a>定义多列

可以通过指定 [MaxColumns 属性](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.maxcolumns)来声明多列 RadioButtons 控件。

```xaml
<muxc:RadioButtons Header="App Mode" MaxColumns="3">
    <x:String>Column 1</x:String>
    <x:String>Column 2</x:String>
    <x:String>Column 3</x:String>
    <x:String>Column 1</x:String>
    <x:String>Column 2</x:String>
    <x:String>Column 3</x:String>
</muxc:RadioButtons>
```

![多列组中的单选按钮](images/radiobutton-multi-columns.png)

### <a name="data-binding"></a>数据绑定

RadioButtons 控件支持使用其 [ItemsSource](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.itemssource) 属性进行数据绑定，如以下代码片段所示。

```xaml
<RadioButtons Header="App Mode" ItemsSource="{x:Bind radioButtonItems}" />
```

```c#
public sealed partial class MainPage : Page
{
    public class OptionDataModel
    {
        public string Label;
        public override string ToString()
        {
            return Label;
        }
    }

    List<OptionDataModel> radioButtonItems;

    public MainPage()
    {
        this.InitializeComponent();

        radioButtonItems = new List<OptionDataModel>();
        radioButtonItems.Add(new OptionDataModel() { label = "Item 1" });
        radioButtonItems.Add(new OptionDataModel() { label = "Item 2" });
        radioButtonItems.Add(new OptionDataModel() { label = "Item 3" });
    }
}
```

## <a name="create-your-own-radio-button-group"></a>创建自己的单选按钮组

> [!Important]
> 建议使用 WinUI RadioButtons 控件对 RadioButton 元素进行分组（除非使用较旧版本的 WinUI）。

单选按钮成组工作。 可通过以下两种方法组合单选按钮控件：

- 将它们放在同一个父容器内
- 在每个单选按钮上将 [GroupName](/uwp/api/Windows.UI.Xaml.Controls.RadioButton.GroupName) 属性设置为相同的值

在此示例中，单选按钮的第一组在相同的堆栈面板中进行隐式分组。 第二组分为两个堆栈面板，以便它们按照 GroupName 进行显式分组。

```xaml
<StackPanel>
    <StackPanel>
        <TextBlock Text="Background" Style="{ThemeResource BaseTextBlockStyle}"/>
        <StackPanel Orientation="Horizontal">
            <RadioButton Content="Green" Tag="Green" Checked="BGRadioButton_Checked"/>
            <RadioButton Content="Yellow" Tag="Yellow" Checked="BGRadioButton_Checked"/>
            <RadioButton Content="Blue" Tag="Blue" Checked="BGRadioButton_Checked"/>
            <RadioButton Content="White" Tag="White" Checked="BGRadioButton_Checked" IsChecked="True"/>
        </StackPanel>
    </StackPanel>
    <StackPanel>
        <TextBlock Text="BorderBrush" Style="{ThemeResource BaseTextBlockStyle}"/>
        <StackPanel Orientation="Horizontal">
            <StackPanel>
                <RadioButton Content="Green" GroupName="BorderBrush" Tag="Green" Checked="BorderRadioButton_Checked"/>
                <RadioButton Content="Yellow" GroupName="BorderBrush" Tag="Yellow" Checked="BorderRadioButton_Checked" IsChecked="True"/>
            </StackPanel>
            <StackPanel>
                <RadioButton Content="Blue" GroupName="BorderBrush" Tag="Blue" Checked="BorderRadioButton_Checked"/>
                <RadioButton Content="White" GroupName="BorderBrush" Tag="White"  Checked="BorderRadioButton_Checked"/>
            </StackPanel>
        </StackPanel>
    </StackPanel>
    <Border x:Name="BorderExample1" BorderThickness="10" BorderBrush="#FFFFD700" Background="#FFFFFFFF" Height="50" Margin="0,10,0,10"/>
</StackPanel>
```

```csharp
private void BGRadioButton_Checked(object sender, RoutedEventArgs e)
{
    RadioButton rb = sender as RadioButton;

    if (rb != null && BorderExample1 != null)
    {
        string colorName = rb.Tag.ToString();
        switch (colorName)
        {
            case "Yellow":
                BorderExample1.Background = new SolidColorBrush(Colors.Yellow);
                break;
            case "Green":
                BorderExample1.Background = new SolidColorBrush(Colors.Green);
                break;
            case "Blue":
                BorderExample1.Background = new SolidColorBrush(Colors.Blue);
                break;
            case "White":
                BorderExample1.Background = new SolidColorBrush(Colors.White);
                break;
        }
    }
}

private void BorderRadioButton_Checked(object sender, RoutedEventArgs e)
{
    RadioButton rb = sender as RadioButton;

    if (rb != null && BorderExample1 != null)
    {
        string colorName = rb.Tag.ToString();
        switch (colorName)
        {
            case "Yellow":
                BorderExample1.BorderBrush = new SolidColorBrush(Colors.Gold);
                break;
            case "Green":
                BorderExample1.BorderBrush = new SolidColorBrush(Colors.DarkGreen);
                break;
            case "Blue":
                BorderExample1.BorderBrush = new SolidColorBrush(Colors.DarkBlue);
                break;
            case "White":
                BorderExample1.BorderBrush = new SolidColorBrush(Colors.White);
                break;
        }
    }
}
```

下图显示了此单选按钮组的呈现方式：

![两个组中的单选按钮](images/radio-button-groups.png)

## <a name="radio-button-states"></a>单选按钮状态

单选按钮有两个状态：“已选中”或“取消选中”。 当选择单选按钮时，其 [IsChecked](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.IsChecked) 属性为 true。 当取消选中单选按钮时，其“IsChecked”属性为“false” 。 要取消选中某个单选按钮，可以单击同一组中的另外一个单选按钮，但无法通过再次单击该按钮将其取消选中。 但是，你可以通过将单选按钮的 IsChecked 属性设置为“False”以编程方式取消选中它。

## <a name="recommendations"></a>建议

- 请确保一组单选按钮的目的和当前状态十分明确。
- 将单选按钮的文本内容限制在一行以内。
- 如果文本内容是动态的，请考虑如何调整按钮大小，以及它周围的视觉对象将发生什么情况。
- 请使用默认字体，除非你的品牌指南告诉你使用其他字体。
- 不要并排放置两个单选按钮组。 当两个两个单选按钮组并排放置时，很难确定哪些按钮属于哪个组。

### <a name="visuals-to-consider"></a>要考虑的视觉对象

下图显示了如何更好地在 RadioButton 组中设置单选按钮的布局。

![一组单选按钮](images/radiobutton-layout.png)

![单选按钮间距指南](images/radiobutton-redline.png)

> [!NOTE]
> 如果使用 WinUI RadioButtons 控件，则间距、边距和方向都已经过优化。

## <a name="get-the-sample-code"></a>获取示例代码

- [XAML 控件库示例](https://github.com/Microsoft/Xaml-Controls-Gallery) - 以交互式格式查看所有 XAML 控件。

## <a name="related-topics"></a>相关主题

### <a name="for-designers"></a>对于设计人员

- [按钮](buttons.md)
- [切换开关](toggles.md)
- [复选框](checkbox.md)
- [列表和组合框](lists.md)
- [滑块](slider.md)

### <a name="for-developers-xaml"></a>面向开发人员 (XAML)

- [RadioButton 类](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.radiobutton)
