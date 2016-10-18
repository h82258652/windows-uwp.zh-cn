---
author: Jwmsft
Description: "按钮为用户提供了触发即时操作的方法。"
label: Buttons
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: eb6744968a4bf06a3766c45b73b428ad690edc06
ms.openlocfilehash: 845aa9935908aa68b64c856ee5e263490a3340c4

---
# 按钮
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

按钮为用户提供了触发即时操作的方法。

![按钮示例](images/controls/button.png)

<div class="important-apis" >
<b>重要的 API</b><br/>
<ul>
<li><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx"><strong>Button 类</strong></a></li>
<li><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.aspx"><strong>RepeatButton 类</strong></a></li>
<li><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.click.aspx"><strong>Click 事件</strong></a></li>
</ul>

</div>
</div>





## 这是正确的控件吗？

用户可以使用按钮启动即时操作，如提交窗体。

当操作是导航到另一个页面时，不要使用按钮，应改用链接。 有关详细信息，请参阅[超链接](hyperlinks.md)。
    
> 例外：对于向导导航，请使用标记为“上一步”和“下一步”的按钮。 对于其他类型的向后导航或向上导航，请使用“上一步”按钮。

## 示例

此示例在 Microsoft Edge 浏览器对话框中使用两个按钮“全部关闭”和“取消”。 

![在对话框中使用的按钮示例](images/control-examples/buttons-edge.png)

## 创建按钮

此示例显示一个响应单击操作的按钮。 

在 XAML 中创建按钮。

```xaml
<Button Content="Submit" Click="SubmitButton_Click"/>
```

或在代码中创建按钮。

```csharp
Button submitButton = new Button();
submitButton.Content = "Submit";
submitButton.Click += SubmitButton_Click;

// Add the button to a parent container in the visual tree.
stackPanel1.Children.Add(submitButton);
```

处理 Click 事件。

```csharp
private async void SubmitButton_Click(object sender, RoutedEventArgs e)
{
    // Call app specific code to submit form. For example:
    // form.Submit();
    Windows.UI.Popups.MessageDialog messageDialog = 
        new Windows.UI.Popups.MessageDialog("Thank you for your submission.");
    await messageDialog.ShowAsync();
}
```

### 按钮交互

当你用手指或触笔点击某个按钮或在指针位于其上时按鼠标左键时，按钮会引发 [**Click**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.click.aspx) 事件。 如果按钮具有键盘焦点，则按 Enter 键或空格键也会引发 Click 事件。

你通常无法处理按钮上的低级别 [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.pointerpressed.aspx) 事件，因为它具有 Click 行为。 有关详细信息，请参阅[事件和路由事件概述](https://msdn.microsoft.com/en-us/library/windows/apps/mt185584.aspx)。

你可以通过更改 [**ClickMode**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.clickmode.aspx) 属性来更改按钮引发 Click 事件的方式。 默认的 ClickMode 值为 **Release**。 如果 ClickMode 为 **Hover**，则无法通过键盘或触摸引发 Click 事件。 


### 按钮内容

按钮是 [**ContentControl**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.contentcontrol.aspx)。 它的 XAML 内容属性为 [**Content**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.contentcontrol.content.aspx)，这对于 XAML 支持如下所示的语法：`<Button>A button's content</Button>`。 可以将任何对象设置为按钮的内容。 如果内容是一个 [UIElement](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.aspx)，则会在按钮中呈现它。 如果该内容是另一种类型的对象，则会在按钮中显示其字符串表示形式。

在此处，将一个包含香蕉图像和文本的 **StackPanel** 设置为按钮的内容。

```xaml
<Button Click="Button_Click" 
        Background="#FF0D6AA3" 
        Height="100" Width="80">
    <StackPanel>
        <Image Source="Assets/Slices.png" Height="62"/>
        <TextBlock Text="Orange"  Foreground="White"
                   HorizontalAlignment="Center"/>
    </StackPanel>
</Button>
```

按钮如下所示。

![包含图像和文本内容的按钮](images/button-orange.png)

## 创建重复按钮

[**RepeatButton**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.aspx) 是一个从按下到释放为止重复引发 [**Click**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.click.aspx) 事件的按钮。 设置 [**Delay**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.delay.aspx) 属性来指定 RepeatButton 在其被按下后和开始重复单击操作之间等待的时间。 设置 [**Interval**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.interval.aspx) 属性来指定重复单击操作之间的时间。 两个属性的时间都以毫秒为单位指定。

以下示例显示两个 RepeatButton 控件，两者各自的 Click 事件用于增加和减少文本块中显示的值。

```xaml
<StackPanel>
    <RepeatButton Width="100" Delay="500" Interval="100" Click="Increase_Click">Increase</RepeatButton>
    <RepeatButton Width="100" Delay="500" Interval="100" Click="Decrease_Click">Decrease</RepeatButton>
    <TextBlock x:Name="clickTextBlock" Text="Number of Clicks:" />
</StackPanel>
```

```csharp
private static int _clicks = 0;
private void Increase_Click(object sender, RoutedEventArgs e)
{
    _clicks += 1;
    clickTextBlock.Text = "Number of Clicks: " + _clicks;
}

private void Decrease_Click(object sender, RoutedEventArgs e)
{
    if(_clicks > 0)
    {
        _clicks -= 1;
        clickTextBlock.Text = "Number of Clicks: " + _clicks;
    }
}
```

## 建议

-   请确保用户清楚地了解按钮的目的和状态。
-   使用简洁具体而又明白易懂的文本来清楚地描述按钮可以执行的操作。 通常，按钮文本内容是一个字词（动词）。
-   如果按钮文本内容是动态的（例如，它被本地化），请考虑如何调整按钮大小，以及它周围的控件将发生什么情况。
-   对于具有文本内容的命令按钮，请使用最小按钮宽度。
-   避免将窄、短、高命令按钮与文本内容一起使用。
-   请使用默认字体，除非你的品牌指南告诉你使用不同的字体。
-   对于需要在应用的多个页面上提供的操作，请考虑使用[底部应用栏](app-bars.md)，而不要在多个页面上重复设置按钮。
-   一次仅向用户显示一两个按钮，例如，“接受”和“取消”。 如果你需要为用户显示更多操作，请考虑使用用户可从中选择操作的[复选框](checkbox.md)或[单选按钮](radio-button.md)，并通过一个命令按钮来触发这些操作。
-   使用默认命令按钮指示最常见或最受推荐的操作。
-   考虑自定义你的按钮。 默认情况下，按钮的形状为矩形，但你可以自定义构成按钮外观的视觉对象。 按钮的内容通常是文本（例如，“接受”或“取消”），但你可以使用图标或使用图标加文本来替代文本。
-   确认在用户与按钮交互时，该按钮将改变状态和外观以向用户提供反馈。 按钮状态的例子包括正常、已按和已禁用。
-   当用户点击或按下按钮时，触发按钮的操作。 操作通常在用户释放按钮时触发，但你也可以设置按钮的操作，使其在手指刚按到它时触发。
-   不要使用命令按钮来设置状态。
-   应用在运行时，不要更改按钮文本。例如，不要将标有“下一步”的按钮文本更改为“继续”。
-   不要交换默认的提交、重置和按钮样式。
-   不要在按钮中放入太多内容。 使内容简洁且易于理解（仅使用一张图片和一些文本）。

## 后退按钮
后退按钮是一种系统提供的 UI 元素，可支持在后退堆栈或用户导航历史记录中向后导航。 你无需创建自己的后退按钮，但可能必须进行一些工作才能支持良好的后退导航体验。 有关详细信息，请参阅[历史记录和向后导航](../layout/navigation-history-and-backwards-navigation.md)

## 获取示例
*   [XAML UI 基本示例](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlUIBasics)<br/>
    以交互式格式查看所有 XAML 控件。


## 相关文章

- [单选按钮](radio-button.md)
- [切换开关](toggles.md)
- [复选框](checkbox.md)
- [**Button 类**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx)





<!--HONumber=Aug16_HO3-->


