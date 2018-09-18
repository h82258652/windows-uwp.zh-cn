---
author: QuinnRadich
Description: Radio buttons let users select one option from two or more choices.
title: 单选按钮指南
ms.assetid: 41E3F928-AA55-42A2-9281-EC3907C4F898
label: Radio buttons
template: detail.hbs
ms.author: quradic
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: kisai
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 48d830b388fee8a0007447a66aa58e3794cfaae0
ms.sourcegitcommit: f5321b525034e2b3af202709e9b942ad5557e193
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/18/2018
ms.locfileid: "4018771"
---
# <a name="radio-buttons"></a>单选按钮

> **重要 API**：[RadioButton 类](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RadioButton)、[Checked 事件](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.Checked)、[IsChecked 属性](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.IsChecked)

单选按钮允许用户从一组中选择一个选项。 每个选项都表示为一个单选按钮，用户只能选择单选按钮组中的一个单选按钮。

（如果你对名称感到好奇，单选按钮是根据收音机上的频道预设按钮命名的。）

![单选按钮](images/controls/radio-button.png)

## <a name="is-this-the-right-control"></a>这是正确的控件吗？

使用单选按钮可向用户显示两个相互排斥的选项。

![一组单选按钮](images/radiobutton_basic.png)

当用户需要查看所有选项来进行选择时可以使用单选按钮。 因为单选按钮平等地强调所有选项，所以可以引起对这些选项的更多关注。 除非选项值得用户额外关注，否则请考虑使用其他控件。 例如，如果在大部分情况下推荐大多数用户使用默认选项，请改用[下拉列表](lists.md)。

![下拉列表](images/combo_box_collapsed.png)

如果仅有两个相互排斥的选项，请将其合并到单个[复选框](checkbox.md)或[切换开关](toggles.md)。 例如，使用复选框“我同意”而不是两个单选按钮“我同意”和“我不同意”。

![提供二元选项的两种方法](images/radiobutton_vs_checkbox.png)

当用户可以选择多个选项时，请使用[复选框](checkbox.md).

![使用复选框选择多个选项](images/checkbox2.png)

当选项是具有固定步长的数字（10、20、30）时，请使用[滑块](slider.md)控件。

![滑块控件](images/controls/slider.png)

如果存在 8 个以上选项，请使用[下拉列表](lists.md)或[列表框](lists.md)。

![组合框](images/combo_box_scroll.png)

如果可用选项基于应用的当前上下文或可以动态变化，请改用单选[列表框](lists.md)。

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果已安装 <strong style="font-weight: semi-bold">XAML 控件库</strong>应用，请单击此处<a href="xamlcontrolsgallery:/item/RadioButton">打开此应用，了解 RadioButton 的实际应用</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Microsoft Edge 浏览器设置中的单选按钮。

![Microsoft Edge 浏览器设置中的单选按钮](images/control-examples/radio-buttons-edge.png)

## <a name="create-a-radio-button"></a>创建单选按钮

单选按钮成组工作。 可通过以下两种方法组合单选按钮控件：
- 将它们放在同一个父容器内。
- 将每个单选按钮的 [GroupName](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RadioButton.GroupName) 属性设置为相同的值。

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

单选按钮组如下所示。

![两个组中的单选按钮](images/radio-button-groups.png)

单选按钮有两个状态：*已选择*或*已清除*。 当选择单选按钮时，其 [IsChecked](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.IsChecked) 属性为 **true**。 当清除单选按钮时，其 **IsChecked** 属性为 **false**。 某个单选按钮可以通过单击同一组中的另外一个单选按钮进行清除，但再次单击该按钮时将无法将其清除。 但是，你可以通过将单选按钮的 IsChecked 属性设置为 **false** 以编程方式清除它。 实际上，通过获取 **IsChecked** 属性的 **Value** 可以将 **IsChecked** 属性与一个布尔值进行比较。

## <a name="recommendations"></a>建议

-   请确保一组单选按钮的目的和当前状态十分明确。
-   将单选按钮的文本内容限制为单行。
-   如果文本内容是动态的，应考虑按钮大小将如何调整以及其周围的视觉对象将有哪些影响。
-   请使用默认字体，除非你的品牌指南告诉你使用另一种字体。
-   不要并排放置两个单选按钮组。 当两个两个单选按钮组并排放置时，很难确定哪些按钮属于哪个组。

## <a name="additional-usage-guidance"></a>其他使用指南

下图展示定位单选按钮和为单选按钮选取位置的正确方法。

![一组单选按钮](images/radiobutton-layout.png)

![单选按钮间距指南](images/radiobutton-redlines.png)

## <a name="related-topics"></a>相关主题

**对于设计人员**
- [按钮](buttons.md)
- [切换开关](toggles.md)
- [复选框](checkbox.md)
- [列表和组合框](lists.md)
- [滑块](slider.md)

**对于开发人员 (XAML)**
- [RadioButton 类](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.radiobutton)
