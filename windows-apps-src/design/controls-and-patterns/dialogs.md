---
author: mijacobs
Description: Dialogs and flyouts display transient UI elements that appear when the user requests them or when something happens that requires notification or approval.
title: 对话框和浮出控件
label: Dialogs
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: ad6affd9-a3c0-481f-a237-9a1ecd561be8
pm-contact: yulikl
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 7b263fda1de798473f581e2191d3fa01385060e6
ms.sourcegitcommit: e4627686138ec8c885696c4c511f2f05195cf8ff
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/17/2018
ms.locfileid: "1893842"
---
# <a name="dialogs-and-flyouts"></a>对话框和浮出控件



对话框和浮出控件是当发生需要通知、批准或来自用户的其他信息的操作时出现的瞬态 UI 元素。

> **重要 API**：[ContentDialog 类](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog)、[Flyout 类](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Flyout)


:::行::: :::列:::**对话框**
        
        ![Example of a dialog](images/dialogs/dialog_RS2_delete_file.png)

        Dialogs are modal UI overlays that provide contextual app information. Dialogs block interactions with the app window until being explicitly dismissed. They often request some kind of action from the user.
    :::column-end:::
    :::column::: 
        **Flyouts**

        ![Example of a flyout](images/flyout-example2.png)

        A flyout is a lightweight contextual popup that displays UI related to what the user is doing. It includes placement and sizing logic, and can be used to reveal a secondary control or show more detail about an item.

        Unlike a dialog, a flyout can be quickly dismissed by tapping or clicking somewhere outside the flyout, pressing the Escape key or Back button, resizing the app window, or changing the device's orientation.
    :::column-end:::
:::行末:::


## <a name="is-this-the-right-control"></a>这是正确的控件吗？

* 使用对话框通知用户重要信息或在可以完成某个操作之前请求确认或其他信息。
* 不要使用浮出控件替代[工具提示](tooltips.md)或[上下文菜单](menus.md)。 使用工具提示显示在指定时间后隐藏的简短描述。 针对与 UI 元素相关的上下文操作（如复制和粘贴）使用上下文菜单。  


对话框和浮出控件确保用户知道重要信息，但它们也会干扰用户体验。 由于对话框是模式对话框（阻止），因此它们会干扰用户，在用户与该对话框交互前阻止它们执行任何其他操作。 浮出控件提供较和谐的体验，但显示过多的浮出控件可能令人分心。

请考虑要共享的信息的重要性：它的重要性是否足以要干扰用户？ 此外，请考虑需要显示该信息的频率；如果你要每几分钟显示一个对话框或通知，你可能希望改为在主 UI 中为此信息分配空间。 例如，在聊天客户端中，你可能显示当前在线的好友列表，并在好友登录时突出显示他们，而不是每次有好友登录时都显示浮出控件。

对话框经常用于在执行某个操作（如删除某个文件）前确认该操作。 如果你预期用户经常执行某个特定操作，请考虑为用户提供在犯错时撤销操作的方法，而不是强迫用户每次都确认该操作。

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果已安装 <strong style="font-weight: semi-bold">XAML 控件库</strong>应用，请单击此处打开该应用，了解 <a href="xamlcontrolsgallery:/item/ContentDialog">ContentDialog</a> 或 <a href="xamlcontrolsgallery:/item/Flyout">Flyout</a> 的实际应用。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="dialogs-vs-flyouts"></a>浮出控件与对话框

确定要使用对话框还是浮出控件后，你需要选择使用哪一个。

鉴于对话框会阻止交互而浮出控件不会，因此对话框应专门用于你希望用户放下一切事务专注于特定的部分信息或回答问题的情况。 另一方面，当你希望将注意力吸引到某些内容，但如果用户想要忽略它也无妨时，可以使用浮出控件。

:::行::: :::列:::
   <p><b>将对话框用于...</b> <br/>
<ul>
<li>表明用户在继续操作之前<b>必须</b>阅读并确认的重要信息。 示例包括：
<ul>
  <li>当用户的安全可能受到威胁时</li>
  <li>当用户准备永久改变宝贵资产时</li>
  <li>当用户准备删除宝贵资产时</li>
  <li>确认应用内购买</li>
</ul>

</li>
<li>应用于整个应用上下文的错误消息，如连接错误。</li>
<li>问题，在应用需要询问用户阻止问题时（例如当应用不能代表用户进行选择时）。 阻止问题不能忽略或延迟，并且应该向用户提供明确定义的选项。</li>
</ul>
</p>
:::列末::: :::列::: <p><b>将浮出控件用于...</b> <br/>
<ul>
<li>收集在可以完成某个操作前所需的其他信息。</li>
<li>显示仅在一些时间内相关的信息。 例如，在照片库应用中，当用户单击某个图像缩略图时，你可以使用浮出控件显示图像的大型版本。</li>
<li>显示详细信息，例如页面上某个项目的详细信息或较长说明。</li>
</ul></p>
:::列末::: :::行末:::



## <a name="dialogs"></a>对话框
### <a name="general-guidelines"></a>一般指南

-   在对话框的第一行文本中清楚地标识问题或用户的目标。
-   对话框标题是主要说明并且是可选的。
    -   使用简短标题说明用户需要怎样处理对话框。
    -   如果你使用对话框来传达简单的消息、错误或问题，则可以省略标题。 可依赖内容文本来传达这样的核心信息。
    -   确保标题与按钮选项直接相关。
-   对话框内容包含描述性文本，并且是必需的。
    -   提供尽可能简单的消息、错误或阻止问题。
    -   如果使用对话框标题，请使用内容区域提供更多详情或定义术语。 不要只是修改几个措词来重复标题。
-   必须至少显示一个对话框按钮。
    -   确保你的对话框至少有一个按钮对应于安全、没有破坏性的操作，如“明白了!”、“关闭”或“取消”。 使用 CloseButton API 可添加该按钮。
    -   使用主要说明或内容的特定响应作为按钮文本。 例如，“你是否希望允许 AppName 访问你的位置”，后跟“允许”和“拒绝”按钮。 具体的响应可以使用户更快速的理解，以便进行高效的决策。
    - 确保操作按钮的文本保持简明。 简短的字符串使用户能够快速、自信地做出选择。
    - 除了安全、无破坏性的操作外，你还可以选择为用户提供一个或两个与主要说明相关的操作按钮。 这些“执行”操作按钮用于确认对话框的重点。 使用 PrimaryButton 和 SecondaryButton API 可添加这些“执行”操作。
    - “执行”操作按钮应显示为最左侧按钮。 安全、无破坏性的操作应显示为最右侧的按钮。
    - 你可以选择三个按钮之一作为对话框的默认按钮。 使用 DefaultButton API 可区分其中一个按钮。  
-   不要为与页面上的特定位置具有上下文关系的错误（例如，密码字段等位置的验证错误）使用对话框，请使用应用的画布本身显示内联错误。
- 使用 [ContentDialog 类](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog)生成对话框体验。 不要使用已弃用的 MessageDialog API。

### <a name="dialog-scenarios"></a>对话框方案
由于对话框会阻止用户交互，而且按钮是用户消除对话框的主要机制，因此请确保你的对话框至少包含一个“安全”且无破坏性的按钮，如“关闭”或“明白!”。 **所有对话应都至少应包含一个安全操作按钮来关闭对话框。** 这可以确保用户能自信地关闭对话框，而未执行操作。<br>![一个按钮对话框](images/dialogs/dialog_RS2_one_button.png)

```csharp
private async void DisplayNoWifiDialog()
{
    ContentDialog noWifiDialog = new ContentDialog
    {
        Title = "No wifi connection",
        Content = "Check your connection and try again.",
        CloseButtonText = "Ok"
    };

    ContentDialogResult result = await noWifiDialog.ShowAsync();
}
```

当对话框用于显示阻止问题时，你的对话框应向用户显示与该问题相关的操作按钮。 这个“安全”且无破坏性的按钮可能会伴随一个或两个“执行”操作按钮。 在向用户提供多个选项时，请确保按钮可清晰地说明与所提出问题相关的“执行”和安全的“不执行”操作。

![两个按钮对话框](images/dialogs/dialog_RS2_two_button.png)

```csharp
private async void DisplayLocationPromptDialog()
{
    ContentDialog locationPromptDialog = new ContentDialog
    {
        Title = "Allow AppName to access your location?",
        Content = "AppName uses this information to help you find places, connect with friends, and more.",
        CloseButtonText = "Block",
        PrimaryButtonText = "Allow"
    };

    ContentDialogResult result = await locationPromptDialog.ShowAsync();
}
```

当你向用户提供两个“执行”操作和一个“不执行”操作时，可使用三个按钮对话框。 应慎用三个按钮对话框，辅助操作与安全/关闭操作之间应有清晰的区别。

![三个按钮对话框](images/dialogs/dialog_RS2_three_button.png)

```csharp
private async void DisplaySubscribeDialog()
{
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

### <a name="the-three-dialog-buttons"></a>三个对话框按钮
ContentDialog 有三种不同类型的按钮可用于构建对话框体验。

- **CloseButton** - 必需 - 表示允许用户退出对话框的安全、无破坏性操作。 显示为最右侧的按钮。
- **PrimaryButton** - 可选 - 表示第一个“执行”操作。 显示为最左侧的按钮。
- **SecondaryButton** - 可选 - 表示第二个“执行”操作。 显示为中间的按钮。

使用内置按钮可相应放置按钮、确保按钮可正确响应键盘事件、确保在屏幕键盘启动时命令区域仍保持可见并使该对话框与其他对话框看起来一致。

#### <a name="closebutton"></a>CloseButton
每个对话框都应包含一个可使用户安心退出对话框的安全、无破坏性的操作按钮。

使用 ContentDialog.CloseButton API 可创建此按钮。 这样你便可以为包括鼠标、键盘、触控和游戏板在内的所有输入创建适当的用户体验。 此体验将在以下情况下发生：
<ol>
    <li>用户单击或点击关闭按钮 </li>
    <li>用户按下系统后退按钮 </li>
    <li>用户按下键盘上的 ESC 按钮 </li>
    <li>用户按下游戏板 B </li>
</ol>

当用户单击某个对话框按钮时，[ShowAsync](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) 方法返回一个 [ContentDialogResult](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialogResult) 来通知你用户单击了哪个按钮。 按 CloseButton 返回 ContentDialogResult.None。

#### <a name="primarybutton-and-secondarybutton"></a>PrimaryButton 和 SecondaryButton
除了 CloseButton，你还可以选择向用户提供与主要说明相关的一个或两个操作按钮。
将 PrimaryButton 用于第一个“执行”操作，将 SecondaryButton 用于第二个“执行”操作。 在三个按钮的对话框中，PrimaryButton 通常表示肯定的“执行”操作，而 SecondaryButton 通常表示中性或辅助“执行”操作。
例如，应用可能会提示用户订阅服务。 作为肯定“执行”操作的 PrimaryButton 将托管“订阅”文本，而作为中性“执行”操作的 SecondaryButton 将托管“尝试”文本。 CloseButton 允许用户取消，而不执行任何操作。

当用户单击 PrimaryButton 时，[ShowAsync](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) 方法将返回 ContentDialogResult.Primary。
当用户单击 SecondaryButton 时，[ShowAsync](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) 方法将返回 ContentDialogResult.Secondary。

![三个按钮对话框](images/dialogs/dialog_RS2_three_button.png)

#### <a name="defaultbutton"></a>DefaultButton
你可以选择区分三个按钮之一，使其作为默认按钮。 指定默认按钮将导致发生以下情况：
- 该按钮接收“突出”按钮视觉处理
- 该按钮将自动响应 ENTER 键
    - 当用户在键盘上按 ENTER 键时，与“默认”按钮关联的单击处理程序将激发，并且 ContentDialogResult 将返回与“默认”按钮关联的值
    - 如果用户已将键盘焦点放在处理 ENTER 的控件上，默认按钮将不响应 ENTER 按下操作
- 打开对话框时，按钮将自动接收焦点，除非对话框内容包含可聚焦的 UI

使用 ContentDialog.DefaultButton 属性指示默认按钮。 默认情况下，不设置默认按钮。

![具有默认按钮的三个按钮对话框](images/dialogs/dialog_RS2_three_button_default.png)

```csharp
private async void DisplaySubscribeDialog()
{
    ContentDialog subscribeDialog = new ContentDialog
    {
        Title = "Subscribe to App Service?",
        Content = "Listen, watch, and play in high definition for only $9.99/month. Free to try, cancel anytime.",
        CloseButtonText = "Not Now",
        PrimaryButtonText = "Subscribe",
        SecondaryButtonText = "Try it",
        DefaultButton = ContentDialogButton.Primary
    };

    ContentDialogResult result = await subscribeDialog.ShowAsync();
}
```

### <a name="confirmation-dialogs-okcancel"></a>确认对话框（确定/取消）
通过确认对话框，用户可以确认他们是否要执行操作。 他们可以确认操作，也可以选择取消。  
典型的确认对话框有两个按钮：一个确认（“确定”）按钮和一个取消按钮。  

<ul>
    <li>
        <p>一般情况下，确认按钮（主要按钮）应位于左侧，取消按钮（辅助按钮）应位于右侧。</p>
        <img alt="An OK/cancel dialog" src="images/dialogs/dialog_RS2_delete_file.png" />
    </li>
    <li>如一般建议部分中所述，使用带有文本的按钮，该文本可标识特定于主要说明或内容的响应。
    </li>
</ul>

> 某些平台将确认按钮放置在右侧，而不是左侧。 那么，为什么我们建议将它放在左侧呢？  假设大多数用户惯用右手并且他们用右手拿着手机，则他们按位于左侧的确认按钮实际上更为舒适，因为该按钮更有可能处于用户的拇指弧范围内。位于屏幕右侧的按钮需要用户将拇指向内收缩到不太舒适的位置。

### <a name="create-a-dialog"></a>创建对话框
若要创建对话框，你使用 [ContentDialog 类](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog)。 你可以使用代码或标记创建对话框。 尽管使用 XAML 定义 UI 元素通常更容易，但对于简单对话框，实际上只使用代码更容易。 此示例创建一个对话框来通知用户没有 WiFi 连接，然后使用 [ShowAsync](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) 方法显示它。

```csharp
private async void DisplayNoWifiDialog()
{
    ContentDialog noWifiDialog = new ContentDialog
    {
        Title = "No wifi connection",
        Content = "Check your connection and try again.",
        CloseButtonText = "Ok"
    };

    ContentDialogResult result = await noWifiDialog.ShowAsync();
}
```

当用户单击某个对话框按钮时，[ShowAsync](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) 方法返回一个 [ContentDialogResult](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialogResult) 来通知你用户单击了哪个按钮。

此示例中的对话框提出一个问题，并使用返回的 [ContentDialogResult](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialogResult) 确定用户的响应。

```csharp
private async void DisplayDeleteFileDialog()
{
    ContentDialog deleteFileDialog = new ContentDialog
    {
        Title = "Delete file permanently?",
        Content = "If you delete this file, you won't be able to recover it. Do you want to delete it?",
        PrimaryButtonText = "Delete",
        CloseButtonText = "Cancel"
    };

    ContentDialogResult result = await deleteFileDialog.ShowAsync();

    // Delete the file if the user clicked the primary button.
    /// Otherwise, do nothing.
    if (result == ContentDialogResult.Primary)
    {
        // Delete the file.
    }
    else
    {
        // The user clicked the CLoseButton, pressed ESC, Gamepad B, or the system back button.
        // Do nothing.
    }
}
```

## <a name="flyouts"></a>浮出控件
###  <a name="create-a-flyout"></a>创建浮出控件

浮出控件是可显示任意 UI 作为其内容的轻型消除容器。 浮出控件可能包含其他浮出控件或上下文菜单，用于创建嵌套体验。

![嵌套在浮出控件内的上下文菜单](images/flyout-nested.png)

浮出控件附加到特定控件。 你可以使用 [Placement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutBase.Placement) 属性指定浮出控件显示的位置：顶部、左侧、底部、右侧或完整。 如果你选择[完整放置模式](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutPlacementMode)，应用将拉伸浮出控件，并使其在应用窗口中居中。 某些控件（如 [Button](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button)）提供可用于关联浮出控件或[上下文菜单](menus.md) 的 [Flyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button.Flyout) 属性。

此示例创建一个在按下按钮时显示一些文本的简单浮出控件。
````xaml
<Button Content="Click me">
  <Button.Flyout>
     <Flyout>
        <TextBlock Text="This is a flyout!"/>
     </Flyout>
  </Button.Flyout>
</Button>
````

如果控件没有浮出控件属性，可以改用 [FlyoutBase.AttachedFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.AttachedFlyoutProperty) 附加属性。 当你这样做时，还需要调用 [FlyoutBase.ShowAttachedFlyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutBase#Windows_UI_Xaml_Controls_Primitives_FlyoutBase_ShowAttachedFlyout_Windows_UI_Xaml_FrameworkElement_) 方法来显示浮出控件。

此示例将一个简单的浮出控件添加到图像。 当用户点击该图像时，应用将显示该浮出控件。

````xaml
<Image Source="Assets/cliff.jpg" Width="50" Height="50"
  Margin="10" Tapped="Image_Tapped">
  <FlyoutBase.AttachedFlyout>
    <Flyout>
      <TextBlock Text="This is some text in a flyout."  />
    </Flyout>        
  </FlyoutBase.AttachedFlyout>
</Image>
````

````csharp
private void Image_Tapped(object sender, TappedRoutedEventArgs e)
{
    FlyoutBase.ShowAttachedFlyout((FrameworkElement)sender);
}
````

之前的示例已内联定义其浮出控件。 你还可以定义一个浮出控件作为静态资源，然后将其用于多个元素。 此示例创建一个更复杂的浮出控件，可在点击其缩略图时显示图像的较大版本。

````xaml
<!-- Declare the shared flyout as a resource. -->
<Page.Resources>
    <Flyout x:Key="ImagePreviewFlyout" Placement="Right">
        <!-- The flyout's DataContext must be the Image Source
             of the image the flyout is attached to. -->
        <Image Source="{Binding Path=Source}"
            MaxHeight="400" MaxWidth="400" Stretch="Uniform"/>
    </Flyout>
</Page.Resources>
````

````xaml
<!-- Assign the flyout to each element that shares it. -->
<StackPanel>
    <Image Source="Assets/cliff.jpg" Width="50" Height="50"
           Margin="10" Tapped="Image_Tapped"
           FlyoutBase.AttachedFlyout="{StaticResource ImagePreviewFlyout}"
           DataContext="{Binding RelativeSource={RelativeSource Mode=Self}}"/>
    <Image Source="Assets/grapes.jpg" Width="50" Height="50"
           Margin="10" Tapped="Image_Tapped"
           FlyoutBase.AttachedFlyout="{StaticResource ImagePreviewFlyout}"
           DataContext="{Binding RelativeSource={RelativeSource Mode=Self}}"/>
    <Image Source="Assets/rainier.jpg" Width="50" Height="50"
           Margin="10" Tapped="Image_Tapped"
           FlyoutBase.AttachedFlyout="{StaticResource ImagePreviewFlyout}"
           DataContext="{Binding RelativeSource={RelativeSource Mode=Self}}"/>
</StackPanel>
````

````csharp
private void Image_Tapped(object sender, TappedRoutedEventArgs e)
{
    FlyoutBase.ShowAttachedFlyout((FrameworkElement)sender);  
}
````

### <a name="style-a-flyout"></a>设置浮出控件的样式
若要设置浮出控件的样式，请修改其 [FlyoutPresenterStyle](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Flyout.FlyoutPresenterStyle)。 此示例显示一个环绕文本段落，并使屏幕阅读器可以访问该文本块。

![带有环绕文本的可访问浮出控件](images/flyout-wrapping-text.png)

````xaml
<Flyout>
  <Flyout.FlyoutPresenterStyle>
    <Style TargetType="FlyoutPresenter">
      <Setter Property="ScrollViewer.HorizontalScrollMode"
          Value="Disabled"/>
      <Setter Property="ScrollViewer.HorizontalScrollBarVisibility" Value="Disabled"/>
      <Setter Property="IsTabStop" Value="True"/>
      <Setter Property="TabNavigation" Value="Cycle"/>
    </Style>
  </Flyout.FlyoutPresenterStyle>
  <TextBlock Style="{StaticResource BodyTextBlockStyle}" Text="Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat."/>
</Flyout>
````

#### <a name="styling-flyouts-for-10-foot-experience"></a>设置浮出控件样式，以实现 10 英尺体验

轻型消除控件（如浮出控件）会捕获瞬态 UI 内的键盘焦点和游戏板焦点，直到消除为止。 若要为此行为提供视觉提示，Xbox 上的轻型消除控件将绘制覆盖，以便使 UI 范围之外的对比度和可见性变暗。 可以使用 [`LightDismissOverlayMode`](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutBase.LightDismissOverlayMode) 属性修改此行为。 默认情况下，浮出控件将在 Xbox（而非其他设备系列）上绘制轻型消除覆盖，不过应用可以选择强制使覆盖始终**打开**或始终**关闭**。

![带有变暗覆盖的浮出控件](images/flyout-smoke.png)

```xaml
<MenuFlyout LightDismissOverlayMode="On">
```

### <a name="light-dismiss-behavior"></a>轻型消除行为
可通过快速的轻型消除操作来关闭浮出控件，这些操作包括
-   在浮出控件之外点击
-   按 Esc 键盘键
-   按硬件或软件系统后退按钮
-   按游戏板 B 按钮

当通过点击消除时，此手势通常会被吸收，不会传递到下面的 UI。 例如，如果在开放式浮出控件后有一个按钮可见，则用户的第一次点击会消除浮出控件，但不会激活此按钮。 按下该按钮需要第二次点击。

可通过将该按钮指定为浮出控件的输入直通元素来更改此行为。 浮出控件将因上述轻型消除操作而关闭，还会将点击事件传递给它的指定 `OverlayInputPassThroughElement`。 请考虑采用此行为，以加快功能类似项目上的用户交互。 如果你的应用有收藏夹集合并且该集合中的每个项目都包括一个附加的浮出控件，用户可能希望快速连续地与多个浮出控件进行交互。

[!NOTE] 请注意不要指定会导致破坏性操作的覆盖输入直通元素。 用户已习惯谨慎执行不激活主要 UI 的轻型消除操作。 关闭、删除或相似的破坏性按钮不应激活轻型消除，以避免不期望的中断行为。

在下面的示例中，将在第一次点击时激活 FavoritesBar 内的所有三个按钮。

````xaml
<Page>
    <Page.Resources>
        <Flyout x:Name="TravelFlyout" x:Key="TravelFlyout"
                OverlayInputPassThroughElement="{x:Bind FavoritesBar}">
            <StackPanel>
                <HyperlinkButton Content="Washington Trails Association"/>
                <HyperlinkButton Content="Washington Cascades - Go Northwest! A Travel Guide"/>  
            </StackPanel>
        </Flyout>
    </Page.Resources>

    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel x:Name="FavoritesBar" Orientation="Horizontal">
            <HyperlinkButton x:Name="PageLinkBtn">Bing</HyperlinkButton>  
            <Button x:Name="Folder1" Content="Travel" Flyout="{StaticResource TravelFlyout}"/>
            <Button x:Name="Folder2" Content="Entertainment" Click="Folder2_Click"/>
        </StackPanel>
        <ScrollViewer Grid.Row="1">
            <WebView x:Name="WebContent"/>
        </ScrollViewer>
    </Grid>
</Page>
````
````csharp
private void Folder2_Click(object sender, RoutedEventArgs e)
{
     Flyout flyout = new Flyout();
     flyout.OverlayInputPassThroughElement = FavoritesBar;
     ...
     flyout.ShowAt(sender as FrameworkElement);
{
````

## <a name="get-the-sample-code"></a>获取示例代码

- [XAML 控件库示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 以交互式格式查看所有 XAML 控件。

## <a name="related-articles"></a>相关文章
- [工具提示](tooltips.md)
- [菜单和上下文菜单](menus.md)
- [Flyout 类](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Flyout)
- [ContentDialog 类](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog)
