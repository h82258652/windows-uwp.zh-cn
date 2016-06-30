---
author: Jwmsft
label: Buttons
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: c183f7390c5b4f99cf0f31426c1431066e1bc96d
ms.openlocfilehash: 91399060e129df18acd38e18d98cad848667a5ad

---
# 按钮
按钮为用户提供了触发即时操作的方法。

![按钮示例](images/controls/button.png)


<span class="sidebar_heading" style="font-weight: bold;">重要的 API</span>

-   [**Button 类**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx)
-   [**RepeatButton 类**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.aspx)
-   [**Click 事件**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.click.aspx)

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

[
            **RepeatButton**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.aspx) 是一个从按下到释放为止重复引发 [**Click**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.click.aspx) 事件的按钮。 设置 [**Delay**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.delay.aspx) 属性来指定 RepeatButton 在其被按下后和开始重复单击操作之间等待的时间。 设置 [**Interval**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.interval.aspx) 属性来指定重复单击操作之间的时间。 两个属性的时间都以毫秒为单位指定。

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
后退按钮是一项系统提供的 UI 提示，支持在后退堆栈或用户导航历史记录中向后导航。

导航历史记录的范围（应用内或全局）取决于设备和设备模式。

## <span id="examples"></span><span id="EXAMPLES"></span>示例


系统后退按钮的 UI 将针对每种设备和输入类型进行优化，但设备和通用 Windows 平台 (UWP) 应用上的导航体验却具有全局性和一致性。 这些不同的体验包括：

手机设备 ![手机上的系统后退功能](images/nav-back-phone.png)
-   始终显示。
-   设备底部的软件或硬件按钮。
-   应用内和应用间的全局后退导航。

<span id="Tablet"></span><span id="tablet"></span><span id="TABLET"></span>平板电脑 ![平板电脑上的系统后退功能（在平板电脑模式下）](images/nav-back-tablet.png)
-   始终在平板电脑模式下显示。

    在桌面模式下不可用。 但可启用标题栏后退按钮。 请参阅[PC、笔记本电脑、平板电脑](#PC)。

    用户可以在平板电脑模式下运行和桌面模式下运行之间切换，方法是转到“设置”&gt;“系统”&gt;“平板电脑模式”****，然后设置“在将设备用作平板电脑时使 Windows 更易于触摸”****。

-   设备底部的导航栏中的软件按钮。
-   应用内和应用间的全局后退导航。

<span id="PC"></span><span id="pc"></span>PC、笔记本电脑、平板电脑 ![PC 或笔记本电脑上的系统后退功能](images/nav-back-pc.png)
-   在桌面模式下可选。

    在平板电脑模式下不可用。 请参阅[平板电脑](#Tablet)。

    默认情况下处于禁用状态。 必须选择启用它。

    用户可以在平板电脑模式下运行和桌面模式下运行之间切换，方法是转到“设置”&gt;“系统”&gt;“平板电脑模式”****，然后设置“在将设备用作平板电脑时使 Windows 更易于触摸”****。

-   应用标题栏中的软件按钮。
-   仅应用内的后退导航。 不支持应用间的导航。

Surface Hub ![Surface Hub 上的系统后退功能](images/nav-back-surfacehub.png)
-   始终显示。
-   设备底部的软件按钮。
-   应用内和应用间的后退导航。

 

## 应做事项和禁止事项


-   启用后退导航。

    如果未启用后退导航，你的应用将包括在全局后退堆栈中，但不保留应用内页面导航历史记录。

-   启用桌面模式下的标题栏后退按钮。

    将保留应用内页面导航历史记录，不支持应用间的后退导航。

    **注意** 在平板电脑模式下，在用户从设备顶部向下轻扫或在设备顶部附件移动鼠标指针时，会显示标题栏。 若要避免重复和混淆，平板电脑模式下不显示标题栏后退按钮。

     

-   在应用内导航历史记录耗尽或不可用时，隐藏或禁用桌面模式下的标题栏后退按钮。

    清楚告诉用户，它们已经尽可能地向后导航。

-   每个后退命令应该返回到后退堆栈中的一个页面，或者如果不是处于桌面模式下，应转到前面紧挨着的应用。

    用户可能会对不直观、不一致和不可预测的后退导航感到疑惑。

## 相关文章

- [单选按钮](radio-button.md)
- [切换开关](toggles.md)
- [复选框](checkbox.md)

**对于开发人员 (XAML)**
- [**Button 类**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx)





<!--HONumber=Jun16_HO4-->


