---
author: Jwmsft
Description: "单选按钮允许用户从两个或多个选项中选择一个选项。"
title: "单选按钮指南"
ms.assetid: 41E3F928-AA55-42A2-9281-EC3907C4F898
label: Radio buttons
template: detail.hbs
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 33a8b62a378e4a9abe20be04a49c94d886144cc5
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="radio-buttons"></a>单选按钮

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

单选按钮允许用户从两个或多个选项中选择一个选项。 每个选项都表示为一个单选按钮；用户只能选择单选按钮组中的一个单选按钮。

（如果你对名称感到好奇，单选按钮根据收音机上的频道预设按钮命名。）

![单选按钮](images/controls/radio-button.png)

<div class="important-apis" >
<b>重要的 API</b><br/>
<ul>
<li>[**RadioButton 类**](https://msdn.microsoft.com/library/windows/apps/br227544)</li>
<li>[**Checked 事件**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.togglebutton.checked.aspx)</li>
<li>[**IsChecked 属性**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.togglebutton.ischecked.aspx)</li>
</ul>
</div>


## <a name="is-this-the-right-control"></a>这是正确的控件吗？

使用单选按钮可向用户显示两个相互排斥的选项，如下所示。

![一组单选按钮](images/radiobutton_basic.png)

单选按钮为应用中十分重要的选项增加明确度和权重。 如果提供的选项重要到可以占用更多屏幕空间，并且较高的选择明确度要求使用非常明确的选项，请使用单选按钮。

单选按钮平等地强调所有选项，这可以引起对这些选项的更多关注。 请考虑使用其他控件，除非这些选项应该引起用户的额外关注。 例如， 如果在大部分情况下为大多数用户推荐默认选项，请改用[下拉菜单](lists.md)。

如果仅有两个相互排斥的选项，请将其合并到单个[复选框](checkbox.md)或[切换开关](toggles.md)。 例如，使用复选框“我同意”而不是两个单选按钮“我同意”和“我不同意”。

![提供二元选项的两种方法](images/radiobutton_vs_checkbox.png)

当用户可选择多个选项时，请使用[复选框](checkbox.md)或[列表框](lists.md)控件。

![使用复选框选择多个选项](images/checkbox2.png)

当选项为具有固定步长的数字（如 10、20、30）时，不要使用单选按钮。 请改用[滑块](slider.md)控件。

如果存在 8 个以上选项，改用[下拉列表](lists.md)、单选[列表框](lists.md)或[列表框](lists.md)。

如果可用选项基于应用的当前上下文或可以其他方式动态变化，请改用单选[列表框](lists.md)。

## <a name="example"></a>示例
Microsoft Edge 浏览器设置中的单选按钮。

![Microsoft Edge 浏览器设置中的单选按钮](images/control-examples/radio-buttons-edge.png)

## <a name="create-a-radio-button"></a>创建单选按钮

单选按钮成组工作。 可通过以下两种方法组合单选按钮控件：
- 将它们放在同一个父容器内。
- 在每个单选按钮上将 [**GroupName**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.radiobutton.groupname.aspx) 属性设置为相同的值。

> **注意**&nbsp;&nbsp;通过键盘访问时，一组单选按钮的行为类似于单个控件。 使用 Tab 键只能访问选定选项，但用户可以使用箭头键循环浏览该组。

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

单选按钮有两个状态：*已选择*或*已清除*。 当选择单选按钮时，其 [**IsChecked**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.togglebutton.ischecked.aspx) 属性为 **true**。 当清除单选按钮时，其 **IsChecked** 属性为 **false**。 某个单选按钮可以通过单击同一组中的另外一个单选按钮进行清除，但再次单击该按钮时将无法将其清除。 但是，你可以通过将单选按钮的 IsChecked 属性设置为 **false** 以编程方式清除它。

## <a name="recommendations"></a>建议

-   请确保一组单选按钮的目的和当前状态十分明确。
-   在用户点击单选按钮时，始终提供视觉反馈。
-   在用户与单选按钮交互时，提供视觉反馈。 单选按钮状态的例子包括正常、已按、以选中和已禁用。 用户可点击单选按钮以激活相关选项。 点击激活的选项不会取消激活，但点击另一个选项会将激活操作转移到后者。
-   保留触摸反馈和选中状态的视觉效果和动画；在取消选中状态，单选按钮控件应显示为未使用或非活动状态（但不是已禁用）。
-   将单选按钮的文本内容限制为单行。 你可以自定义单选按钮的视觉对象以在主要的文本行下方以较小的字体显示选项的说明。
-   如果文本内容是动态的，请考虑如何调整按钮大小，以及它周围的视觉对象将发生什么情况。
-   请使用默认字体，除非你的品牌指南告诉你使用另一种字体。
-   将单选按钮包含在标签元素内，以便点击该标签时可选中单选按钮。
-   将标签文本放在单选按钮控件之后，而不是之前或之上。
-   考虑自定义你的单选按钮。 默认情况下，单选按钮包括两个同心圆（里层为实心并在选中单选按钮时显示，外层为空心）和一些文本内容。 但我们鼓励你发挥创意。 用户喜欢直接与应用内容交互。 因此，你可以选择显示提供的实际内容，无论它使用图形呈现还是作为微妙的文本切换按钮。
-   不要在单选按钮组中放入 8 个以上的选项。 当你需要重新更多选项时，使用[下拉列表](lists.md)、[列表框](lists.md)或[列表视图](lists.md)。
-   不要并排放置两个单选按钮组。 当两个两个单选按钮组并排放置时，很难确定哪些按钮属于哪个组。 请使用组标签分隔它们。

## <a name="additional-usage-guidance"></a>其他使用指南

下图展示定位单选按钮和为单选按钮选取位置的正确方法。

![一组单选按钮](images/radiobutton_layout1.png)
## <a name="related-topics"></a>相关主题

**对于设计人员**
- [按钮指南](buttons.md)
- [切换开关指南](toggles.md)
- [复选框指南](checkbox.md)
- [下拉列表指南](lists.md)
- [列表视图和网格视图控件指南](lists.md)
- [滑块指南](slider.md)
- [选择控件指南](lists.md)


**对于开发人员 (XAML)**
- [**Windows.UI.Xaml.Controls RadioButton 类**](https://msdn.microsoft.com/library/windows/apps/br227544)
