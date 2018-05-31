---
author: serenaz
Description: A button gives the user a way to trigger an immediate action.
title: 按钮
label: Buttons
template: detail.hbs
ms.author: sezhen
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp
ms.assetid: f04d1a3c-7dcd-4bc8-9586-3396923b312e
pm-contact: kisai
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: f663ce9da6453922e35f850a8cd039f33770f434
ms.sourcegitcommit: 4b522af988273946414a04fbbd1d7fde40f8ba5e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2018
ms.locfileid: "1494164"
---
# <a name="buttons"></a>按钮


按钮为用户提供了触发即时操作的方法。

> **重要 API**：[Button 类](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx)、[RepeatButton 类](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.aspx)、[Click 事件](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.click.aspx)

![按钮示例](images/controls/button.png)

## <a name="is-this-the-right-control"></a>这是正确的控件吗？

用户可以使用按钮启动即时操作，如提交窗体。

当操作是导航到另一个页面时，不要使用按钮，应改用链接。 有关详细信息，请参阅[超链接](hyperlinks.md)。
> 例外：对于向导导航，请使用标记为“上一步”和“下一步”的按钮。 对于其他类型的向后导航或向上导航，请使用“上一步”按钮。

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果已安装 <strong style="font-weight: semi-bold">XAML 控件库</strong>应用，请单击此处<a href="xamlcontrolsgallery:/item/Button">打开此应用，了解 Button 的实际应用</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

此示例在请求位置访问权限的对话框中使用两个按钮，即 Allow 和 Block。

![在对话框中使用的按钮示例](images/dialogs/dialog_RS2_two_button.png)

## <a name="create-a-button"></a>创建按钮

此示例显示一个响应单击操作的按钮。

在 XAML 中创建按钮。

```xaml
<Button Content="Subscribe" Click="SubscribeButton_Click"/>
```

或在代码中创建按钮。

```csharp
Button subscribeButton = new Button();
subscribeButton.Content = "Subscribe";
subscribeButton.Click += SubscribeButton_Click;

// Add the button to a parent container in the visual tree.
stackPanel1.Children.Add(subscribeButton);
```

处理 Click 事件。

```csharp
private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
{
    // Call app specific code to subscribe to the service. For example:
    ContentDialog subscribeDialog = new ContentDialog
    {
        Title = "Subscribe to App Service?",
        Content = "Listen, watch, and play in high definition for only $9.99/month. Free to try, cancel anytime.",
        CloseButtonText = "Not Now",
        PrimaryButtonText = "Subscribe",
        SecondaryButtonText = "Try it"
    };

    ContentDialogResult result = await subscribeDialog.ShowAsync();
}
```

### <a name="button-interaction"></a>按钮交互

当你用手指或触笔点击某个按钮或在指针位于其上时按鼠标左键时，按钮会引发 [Click](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.click.aspx) 事件。 如果按钮具有键盘焦点，则按 Enter 键或空格键也会引发 Click 事件。

你通常无法处理按钮上的低级别 [PointerPressed](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.pointerpressed.aspx) 事件，因为它具有 Click 行为。 有关详细信息，请参阅[事件和路由事件概述](https://msdn.microsoft.com/library/windows/apps/mt185584.aspx)。

你可以通过更改 [ClickMode](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.clickmode) 属性来更改按钮引发 Click 事件的方式。 ClickMode 默认值为 **Release**，但你也可以将按钮的 ClickMode 设置为 **Hover** 或 **Press**。 如果 ClickMode 为 **Hover**，则无法通过键盘或触摸引发 Click 事件。


### <a name="button-content"></a>按钮内容

按钮是 [ContentControl](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.contentcontrol.aspx)。 它的 XAML 内容属性为 [Content](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.contentcontrol.content.aspx)，这对于 XAML 支持如下所示的语法：`<Button>A button's content</Button>`。 可以将任何对象设置为按钮的内容。 如果内容是一个 [UIElement](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.aspx)，则会在按钮中呈现它。 如果该内容是另一种类型的对象，则会在按钮中显示其字符串表示形式。

按钮内容通常为文本。 下面是对具有文本内容的按钮的设计建议：
-   使用简洁具体而又明晰易懂的文本来清楚地描述按钮可以执行的操作。 通常，按钮文本内容是一个字词（动词）。
-   请使用默认字体，除非你的品牌指南告诉你使用不同的字体。
-   对于较短的文本，通过使用最小按钮宽度 120px 避免命令按钮过窄。
- 对于较长的文本，通过将文本最大长度限制为 26 个字符避免命令按钮过宽。
-   如果按钮的文本内容是动态的（例如，它已[本地化](../globalizing/globalizing-portal.md)），应考虑按钮大小将如何调整以及其周围控件将有哪些影响。

<table>
<tr>
<td> <b>需要修复：</b><br> 具有溢出文本的按钮。 </td>
<td> <img src="images/button-wraptext.png"/> </td>
</tr>
<tr>
<td> <b>选项 1：</b><br> 如果文本长度大于 26 个字符，请增加按钮宽度、堆叠按钮和换行。 </td>
<td> <img src="images/button-wraptext1.png"> </td>
</tr>
<tr>
<td> <b>选项 2：</b><br> 增加按钮高度并对文本换行。 </td>
<td> <img src="images/button-wraptext2.png"> </td>
</tr>
</table>

你还可以自定义构成按钮外观的视觉效果。 例如，可以将文本替换为图标，或使用图标加文本。

在这里，我们将一个包含图像和文本的 **StackPanel** 设置为按钮内容。

```xaml
<Button Click="Button_Click"
        Background="LightGray"
        Height="100" Width="80">
    <StackPanel>
        <Image Source="Assets/Photo.png" Height="62"/>
        <TextBlock Text="Photos" Foreground="Black"
                   HorizontalAlignment="Center"/>
    </StackPanel>
</Button>
```

按钮如下所示。

![包含图像和文本内容的按钮](images/button-orange.png)

## <a name="create-a-repeat-button"></a>创建重复按钮

[RepeatButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.aspx) 是一个从按下到释放为止重复引发 [Click](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.click.aspx) 事件的按钮。 设置 [Delay](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.delay.aspx) 属性来指定 RepeatButton 在其被按下后和开始重复单击操作之间等待的时间。 设置 [Interval](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.interval.aspx) 属性来指定重复单击操作之间的时间。 两个属性的时间都以毫秒为单位指定。

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

## <a name="recommendations"></a>建议
-   请确保用户清楚地了解按钮的目的和状态。
-   有多个按钮用于相同决策时（例如，在确认对话框中），请按以下顺序显示提交按钮，其中，[执行]和[不执行]是对主要说明的具体响应：
    -   确定/[执行]/是
    -   [不执行]/否
    -   取消
-   一次仅向用户显示一两个按钮，例如，“接受”和“取消”。 如果你需要为用户显示更多操作，请考虑使用用户可从中选择操作的[复选框](checkbox.md)或[单选按钮](radio-button.md)，并通过一个命令按钮来触发这些操作。
-   对于需要在应用的多个页面上提供的操作，请考虑使用[底部应用栏](app-bars.md)，而不要在多个页面上重复设置按钮。

### <a name="recommended-single-button-layout"></a>推荐单按钮布局

如果布局只需一个按钮，则它应基于其容器上下文进行左对齐或右对齐。

-   只有一个按钮的对话框应使按钮**右对齐**。 如果对话框只包含一个按钮，请确保该按钮执行安全、无破坏性的操作。 如果使用 [ContentDialog](dialogs.md) 并指定单个按钮，则它会自动右对齐。

![对话框中的按钮](images/pushbutton_doc_dialog.png)

-   如果按钮出现在容器 UI 中（例如，在 toast 通知、浮出控件或列表视图项目中），则应在容器中使按钮**右对齐**。

![容器中的按钮](images/pushbutton_doc_container.png)

-   在包含单个按钮的页面中（例如，处于设置页面底部的“应用”按钮），应使按钮**左对齐**。 这会确保按钮与页面内容的其余部分对齐。

![页面上的按钮](images/pushbutton_doc_page.png)

## <a name="back-buttons"></a>后退按钮

后退按钮是一种系统提供的 UI 元素，可支持在后退堆栈或用户导航历史记录中向后导航。 你无需创建自己的后退按钮，但可能必须进行一些工作才能支持良好的后退导航体验。 有关详细信息，请参阅 [历史记录和向后导航](../basics/navigation-history-and-backwards-navigation.md)

## <a name="get-the-sample-code"></a>获取示例代码

- [XAML 控件库示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 以交互式格式查看所有 XAML 控件。


## <a name="related-articles"></a>相关文章
- [Button 类](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx)
- [单选按钮](radio-button.md)
- [复选框](checkbox.md)
- [切换开关](toggles.md)
