---
author: QuinnRadich
Description: Used to select or deselect action items. Can be used for a single list item or for multiple list items.
title: 复选框
ms.assetid: 6231A806-287D-43EE-BD8D-39D2FF761914
label: Check boxes
template: detail.hbs
ms.author: quradic
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 9c926f9dc7a87b83550bb2cd3a5bff856ec27866
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2018
ms.locfileid: "7427904"
---
# <a name="check-boxes"></a>复选框

 

复选框用于选择或取消选择操作项目。 它可用于单个项目或用户可以选择的多个项目列表。 该控件具有三个选择状态：未选中、已选中和不确定。 在子选择集具有未选中和已选中两种状态时，使用不确定状态。

> **重要 API**：[CheckBox 类](https://msdn.microsoft.com/library/windows/apps/br209316)、[Checked 事件](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.togglebutton.checked.aspx)、[IsChecked 属性](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.togglebutton.ischecked.aspx)

![复选框状态示例](images/templates-checkbox-states-default.png)


## <a name="is-this-the-right-control"></a>这是正确的控件吗？

将**单个复选框**用于二元是/否选项，例如“记住我？” 登录方案或服务协议的条款选项。

![为单个选项使用的单个复选框](images/checkbox1.png)

对于二元选项，**复选框**和[切换开关](toggles.md)之间的主要差别是：复选框用于表示状态，而切换开关用于表示操作。 你可以延迟提交复选框交互（例如作为表单提交的一部分），然而应该立即提交切换开关交互。 另外，仅复选框允许多选。

使用多选方案的**多个复选框**，在这些方案中，用户从非相互排斥的选项组中选择一个或多个项目。

当用户需要选择任何选项组合时创建一组复选框。

![使用复选框选择多个选项](images/checkbox2.png)

当选项可以分组时，你可以使用不确定复选框来表示整个分组。 当用户选择分组的一些（而非所有）子项时，请使用复选框的不确定状态。

![用于显示混合选项的复选框](images/checkbox3.png)

**复选框**和**单选按钮**控件都能让用户选择选项列表中的内容。 复选框让用户选择选项组合。 相比之下，单选按钮允许用户在相互排斥的选项中选择单个选项。 当存在多个选项，但只能选择一个选项时，应使用单选按钮。

## <a name="examples"></a>示例

<table>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果已安装 <strong style="font-weight: semi-bold">XAML 控件库</strong>应用，请单击此处<a href="xamlcontrolsgallery:/item/CheckBox">打开此应用，了解 CheckBox 的实际应用</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-checkbox"></a>创建复选框

若要为复选框分配标签，请设置 [Content](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentcontrol.content.aspx) 属性。 标签显示在复选框旁边。

此 XAML 创建用于在提交表格前同意服务条款的单个复选框。 

```xaml
<CheckBox x:Name="termsOfServiceCheckBox" 
          Content="I agree to the terms of service."/>
```

以下是在代码中创建的相同复选框。

```csharp
CheckBox checkBox1 = new CheckBox();
checkBox1.Content = "I agree to the terms of service.";
```

### <a name="bind-to-ischecked"></a>绑定到 IsChecked

使用 [IsChecked](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.togglebutton.ischecked.aspx) 属性确定复选框是已选中还是已清除。 你可以将 IsChecked 属性的值绑定到其他二进制值。 但是，由于 IsChecked 是一个[可空](https://msdn.microsoft.com/library/windows/apps/b3h38hb0.aspx)布尔值，所以必须使用值转换器才能将它绑定到某个布尔值。

在本示例中，同意服务条款的复选框的 **IsChecked** 属性绑定到了“提交”按钮的 [IsEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.control.isenabled.aspx) 属性。 “提交”按钮仅在服务条款得到同意时才启用。

> 注意&nbsp;&nbsp;我们仅在此处显示相关代码。 有关数据绑定和值转换器的详细信息，请参阅[数据绑定概述](../../data-binding/data-binding-quickstart.md)。

```xaml
...
<Page.Resources>
    <local:NullableBooleanToBooleanConverter x:Key="NullableBooleanToBooleanConverter"/>
</Page.Resources>

...

<StackPanel Grid.Column="2" Margin="40">
    <CheckBox x:Name="termsOfServiceCheckBox" Content="I agree to the terms of service."/>
    <Button Content="Submit" 
            IsEnabled="{x:Bind termsOfServiceCheckBox.IsChecked, 
                        Converter={StaticResource NullableBooleanToBooleanConverter}, Mode=OneWay}"/>
</StackPanel>
```

```csharp
public class NullableBooleanToBooleanConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, string language)
    {
        if (value is bool?)
        {
            return (bool)value;
        }
        return false;
    }

    public object ConvertBack(object value, Type targetType, object parameter, string language)
    {
        if (value is bool)
            return (bool)value;
        return false;
    }
}
```

### <a name="handle-click-and-checked-events"></a>处理 Click 和 Checked 事件

若要在复选框状态更改时执行某项操作，可以处理 [Click](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.click.aspx) 事件或 [Checked](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.togglebutton.checked.aspx) 和 [Unchecked](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.togglebutton.unchecked.aspx) 事件。 

每当选中的状态更改时都会发生 **Click** 事件。 如果你处理 Click 事件，请使用 **IsChecked** 属性确定复选框状态。

**Checked** 和 **Unchecked** 事件独立发生。 如果你处理这些事件，应同时处理它们以响应复选框中的状态更改。

下面的示例介绍如何处理 Click 事件以及 Checked 和 Unchecked 事件。

多个复选框可共享相同的事件处理程序。 本示例创建了四个用于选择披萨料的复选框。 这四个复选框共享相同的 **Click** 事件处理程序以更新选择的佐料列表。

```XAML
<StackPanel Margin="40">
    <TextBlock Text="Pizza Toppings"/>
    <CheckBox Content="Pepperoni" x:Name="pepperoniCheckbox"
              Click="toppingsCheckbox_Click"/>
    <CheckBox Content="Beef" x:Name="beefCheckbox" 
              Click="toppingsCheckbox_Click"/>
    <CheckBox Content="Mushrooms" x:Name="mushroomsCheckbox"
              Click="toppingsCheckbox_Click"/>
    <CheckBox Content="Onions" x:Name="onionsCheckbox"
              Click="toppingsCheckbox_Click"/>

    <!-- Display the selected toppings. -->
    <TextBlock Text="Toppings selected:"/>
    <TextBlock x:Name="toppingsList"/>
</StackPanel>
```

以下是 Click 事件的事件处理程序。 每次单击某个复选框时，它会检查所有复选框以查看选中的复选框并更新选择的佐料列表。

```csharp
private void toppingsCheckbox_Click(object sender, RoutedEventArgs e)
{
    string selectedToppingsText = string.Empty;
    CheckBox[] checkboxes = new CheckBox[] { pepperoniCheckbox, beefCheckbox,
                                             mushroomsCheckbox, onionsCheckbox };
    foreach (CheckBox c in checkboxes)
    {
        if (c.IsChecked == true)
        {
            if (selectedToppingsText.Length > 1)
            {
                selectedToppingsText += ", ";
            }
            selectedToppingsText += c.Content;
        }
    }
    toppingsList.Text = selectedToppingsText;
}
```

### <a name="use-the-indeterminate-state"></a>使用不确定状态

CheckBox 控件继承自 [ToggleButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.togglebutton.aspx)，可拥有三种状态： 

状态 | 属性 | 值
------|----------|------
已选中 | IsChecked | **true** 
取消选中 | IsChecked | **false** 
不确定 | IsChecked | **null** 

对于报告不确定状态的复选框，必须将 [IsThreeState](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.togglebutton.isthreestate.aspx) 属性设置为 **true**。 

当选项可以分组时，你可以使用不确定复选框来表示整个分组。 当用户选择分组的一些（而非所有）子项时，请使用复选框的不确定状态。

在以下示例中，“全选”复选框的 IsThreeState 属性已设置为 **true**。 “全选”复选框在所有子元素已选中的情况下为选中，在所有子元素取消选中时为取消选中，而在其他情况下时为不确定。

```xaml
<StackPanel>
    <CheckBox x:Name="OptionsAllCheckBox" Content="Select all" IsThreeState="True" 
              Checked="SelectAll_Checked" Unchecked="SelectAll_Unchecked" 
              Indeterminate="SelectAll_Indeterminate"/>
    <CheckBox x:Name="Option1CheckBox" Content="Option 1" Margin="24,0,0,0" 
              Checked="Option_Checked" Unchecked="Option_Unchecked" />
    <CheckBox x:Name="Option2CheckBox" Content="Option 2" Margin="24,0,0,0" 
              Checked="Option_Checked" Unchecked="Option_Unchecked" IsChecked="True"/>
    <CheckBox x:Name="Option3CheckBox" Content="Option 3" Margin="24,0,0,0"
              Checked="Option_Checked" Unchecked="Option_Unchecked" />
</StackPanel>
```

```csharp
private void Option_Checked(object sender, RoutedEventArgs e)
{
    SetCheckedState();
}

private void Option_Unchecked(object sender, RoutedEventArgs e)
{
    SetCheckedState();
}

private void SelectAll_Checked(object sender, RoutedEventArgs e)
{
    Option1CheckBox.IsChecked = Option2CheckBox.IsChecked = Option3CheckBox.IsChecked = true;
}

private void SelectAll_Unchecked(object sender, RoutedEventArgs e)
{
    Option1CheckBox.IsChecked = Option2CheckBox.IsChecked = Option3CheckBox.IsChecked = false;
}

private void SelectAll_Indeterminate(object sender, RoutedEventArgs e)
{
    // If the SelectAll box is checked (all options are selected), 
    // clicking the box will change it to its indeterminate state.
    // Instead, we want to uncheck all the boxes,
    // so we do this programatically. The indeterminate state should
    // only be set programatically, not by the user.

    if (Option1CheckBox.IsChecked == true &&
        Option2CheckBox.IsChecked == true &&
        Option3CheckBox.IsChecked == true)
    {
        // This will cause SelectAll_Unchecked to be executed, so
        // we don't need to uncheck the other boxes here.
        OptionsAllCheckBox.IsChecked = false;
    }
}

private void SetCheckedState()
{
    // Controls are null the first time this is called, so we just 
    // need to perform a null check on any one of the controls.
    if (Option1CheckBox != null)
    {
        if (Option1CheckBox.IsChecked == true &&
            Option2CheckBox.IsChecked == true &&
            Option3CheckBox.IsChecked == true)
        {
            OptionsAllCheckBox.IsChecked = true;
        }
        else if (Option1CheckBox.IsChecked == false &&
            Option2CheckBox.IsChecked == false &&
            Option3CheckBox.IsChecked == false)
        {
            OptionsAllCheckBox.IsChecked = false;
        }
        else
        {
            // Set third state (indeterminate) by setting IsChecked to null.
            OptionsAllCheckBox.IsChecked = null;
        }
    }
}
```

## <a name="dos-and-donts"></a>应做事项和禁止事项

-   验证复选框的目的和当前状态是否明确。
-   将复选框文本内容限制为小于等于两行。
-   将复选框标签表述为一个语句，如果有复选标记，该句为真，没有复选标记则为假。
-   请使用默认字体，除非你的品牌指南告诉你使用另一种字体。
-   如果文本内容是动态的内容，请考虑如何调整控件大小，以及它周围的视觉对象将发生什么情况。
-   如果有两个或两个以上的相互排斥的选项可选择，请考虑使用[单选按钮](radio-button.md)。
-   不要并排放置两个复选框组。 请使用组标签分隔这些组。
-   不要使用复选框作为开/关控件或执行命令；请改用切换开关。
-   不要使用复选框显示其他控件，如对话框。
-   使用不确定状态指示选项是为部分但不是全部子选择设置的。
-   使用不确定状态时，请使用从属复选框显示选择了哪些选项，未选择哪些。 设计 UI，使用户可以看到子选择。
-   不要使用不确定状态代表第三种状态。 不确定状态用于指示选项是为部分但不是全部子选择设置的。 因此，不要允许用户直接设置不确定状态。 有关哪些未做的示例，此复选框使用不确定状态指示中等辣度：

    ![不确定复选框](images/checkbox4_spicy.png)

    相反，请使用有三个选项的单选按钮组：“不辣”、“辛辣”和“超辣”。

    ![具有三个选项的单选按钮组：“不辣”、“辛辣”和“超辣”](images/spicyoptions.png)

## <a name="get-the-sample-code"></a>获取示例代码

- [XAML 控件库示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 以交互式格式查看所有 XAML 控件。

## <a name="related-articles"></a>相关文章

- [CheckBox 类](https://msdn.microsoft.com/library/windows/apps/br209316) 
- [单选按钮](radio-button.md)
- [切换开关](toggles.md)
