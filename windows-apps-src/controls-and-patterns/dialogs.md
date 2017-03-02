---
author: mijacobs
Description: "对话框和浮出控件显示当用户请求这些元素或发生需要通知或批准的操作时出现的瞬态 UI 元素。"
title: "对话框和浮出控件"
label: Dialogs
template: detail.hbs
ms.author: mijacobs
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: ad6affd9-a3c0-481f-a237-9a1ecd561be8
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: e76ae1e85f1512a939f2b7ee50ed205c0c55605b
ms.lasthandoff: 02/08/2017

---
# <a name="dialogs-and-flyouts"></a>对话框和浮出控件

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

对话框和浮出控件是当发生需要通知、批准或来自用户的其他信息的操作时出现的瞬态 UI 元素。

<div class="important-apis" >
<b>重要的 API</b><br/>
<ul>
<li>[ContentDialog 类](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentdialog.aspx)</li>
<li>[Flyout 类](https://msdn.microsoft.com/library/windows/apps/dn279496)</li>
</ul>
</div>


<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
   <p><b>对话框</b> <br/><br/>
    ![对话框示例](images/dialogs/dialog-delete-file-example.png)</p>
<p>对话框是用于提供上下文应用信息的模式 UI 覆盖。 除非明确取消对话框，否则它会阻止与应用窗口的交互。 它们通常会请求用户进行某种类型的操作。   
</p><br/>

  </div>
  <div class="side-by-side-content-right">
   <p><b>浮出控件</b> <br/><br/>
   ![浮出控件示例](images/flyout-example.png)</p>
<p>浮出控件是轻量级上下文弹出窗口，用于显示与用户正在执行的操作相关的 UI。 它包含放置和大小调整逻辑，可用于显示隐藏的控件、显示关于某个项目的更多详细信息，或者请求用户确认某个操作。 
</p><p>和对话框不同，浮出控件可通过点击或单击浮出控件之外的某处、按 Escape 键或后退按钮、调整应用窗口大小或更改设备的方向来快速取消。
</p><br/>

  </div>
</div>
</div>

## <a name="is-this-the-right-control"></a>这是正确的控件吗？

* 使用对话框和浮出控件通知用户重要信息或在可以完成某个操作之前请求确认或其他信息。 
* 不要使用浮出控件替代[工具提示](tooltips.md)或[上下文菜单](menus.md)。 使用工具提示显示在指定时间后隐藏的简短描述。 针对与 UI 元素相关的上下文操作（如复制和粘贴）使用上下文菜单。  


对话框和浮出控件确保用户知道重要信息，但它们也会干扰用户体验。 由于对话框是模式对话框（阻止），因此它们会干扰用户，在用户与该对话框交互前阻止它们执行任何其他操作。 浮出控件提供较和谐的体验，但显示过多的浮出控件可能令人分心。 

请考虑要共享的信息的重要性：它的重要性是否足以要干扰用户？ 此外，请考虑需要显示该信息的频率；如果你要每几分钟显示一个对话框或通知，你可能希望改为在主 UI 中为此信息分配空间。 例如，在聊天客户端中，你可能显示当前在线的好友列表，并在好友登录时突出显示他们，而不是每次有好友登录时都显示浮出控件。 

浮出控件和对话框经常用于在执行某个操作（如删除某个文件）前确认该操作。 如果你预期用户经常执行某个特定操作，请考虑为用户提供在犯错时撤销操作的方法，而不是强迫用户每次都确认该操作。 



## <a name="dialogs-vs-flyouts"></a>浮出控件与对话框

确定要使用对话框还是浮出控件后，你需要选择使用哪一个。 

鉴于对话框会阻止交互而浮出控件不会，因此对话框应专门用于你希望用户放下一切事务专注于特定的部分信息或回答问题的情况。 另一方面，当你希望将注意力吸引到某些内容，但如果用户想要忽略它也无妨时，可以使用浮出控件。 

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
   <p><b>将对话框用于...</b> <br/>
<ul>
<li>表明用户在继续操作之前**必须**阅读并确认的重要信息。 示例包括：
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
  </div>
  <div class="side-by-side-content-right">
   <p><b>将浮出控件用于...</b> <br/>
<ul>
<li>收集在可以完成某个操作前所需的其他信息。</li>
<li>显示仅在一些时间内相关的信息。 例如，在照片库应用中，当用户单击某个图像缩略图时，你可以使用浮出控件显示图像的大型版本。</li>
<li>警告和确认，包括与可能有破坏性的操作相关的警告和确认。</li>
<li>显示详细信息，例如页面上某个项目的详细信息或较长说明。</li>
</ul></p>
  </div>
</div>
</div>

<div class="microsoft-internal-note">
轻型消除控件会捕获瞬态 UI 内的键盘焦点和游戏板焦点，直到消除为止。 若要为此行为提供视觉提示，Xbox 上的轻型消除控件将绘制覆盖，以便使 UI 范围之外的可见性变暗。 可以使用新的 `LightDismissOverlayMode` 属性修改此行为。 默认情况下，瞬态 UI 将在 Xbox（而非其他设备系列）上绘制轻型消除覆盖，不过应用可以选择强制使覆盖始终**打开**或始终**关闭**。

```xaml
<MenuFlyout LightDismissOverlayMode=\"Off\">
```
</div>

## <a name="dialogs"></a>对话框
### <a name="general-guidelines"></a>一般指南

-   在对话框的第一行文本中清楚地标识问题或用户的目标。
-   对话框标题是主要说明并且是可选的。
    -   使用简短标题说明用户需要怎样处理对话框。 长标题不会换行而且将被截断。
    -   如果你使用对话框来传达简单的消息、错误或问题，则可以省略标题。 可依赖内容文本来传达这样的核心信息。
    -   确保标题与按钮选项直接相关。
-   对话框内容包含描述性文本，并且是必需的。
    -   提供尽可能简单的消息、错误或阻止问题。
    -   如果使用对话框标题，请使用内容区域提供更多详情或定义术语。 不要只是修改几个措词来重复标题。
-   必须至少显示一个对话框按钮。
    -   按钮是用户消除对话框的唯一机制。
    -   使用带有文本的按钮，该文本可标识对于主要说明或内容的响应。 例如，“你是否希望允许 AppName 访问你的位置”，后跟“允许”和“拒绝”按钮。 具体的响应可以使用户更快速的理解，以便进行高效的决策。
    - 按以下顺序显示提交按钮： 
        -   确定/[执行]/是
        -   [不执行]/否
        -   取消
        
        （其中，[执行]和[不执行]是对主要说明的具体响应。）
   
-   错误对话框在对话框中显示错误消息，以及任何相关的信息。 在错误对话框中使用的唯一按钮应为“关闭”或类似操作。
-   不要为与页面上的特定位置具有上下文关系的错误（例如，密码字段等位置的验证错误）使用对话框，请使用应用的画布本身显示内联错误。

### <a name="confirmation-dialogs-okcancel"></a>确认对话框（确定/取消）
通过确认对话框，用户可以确认他们是否要执行操作。 他们可以确认操作，也可以选择取消。  
典型的确认对话框有两个按钮：一个确认（“确定”）按钮和一个取消按钮。  

<ul>
    <li>
        <p>一般情况下，确认按钮（主要按钮）应位于左侧，取消按钮（辅助按钮）应位于右侧。</p>
         ![确定/取消对话框](images/dialogs/dialog-delete-file-example.png)
        
    </li>
    <li>如一般建议部分中所述，使用带有文本的按钮，该文本可标识特定于主要说明或内容的响应。
    </li>
</ul>

> 某些平台将确认按钮放置在右侧，而不是左侧。 那么，为什么我们建议将它放在左侧呢？  如果你假设大多数用户惯用右手并且他们用右手拿着手机，按位于左侧的确认按钮实际上更为舒适，因为该按钮更有可能处于用户的拇指弧范围内。 位于屏幕右侧的按钮需要用户将拇指向内收缩到不太舒适的位置。

### <a name="create-a-dialog"></a>创建对话框
若要创建对话框，你使用 [ContentDialog 类](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentdialog.aspx)。 你可以使用代码或标记创建对话框。 尽管使用 XAML 定义 UI 元素通常更容易，但对于简单对话框，实际上只使用代码更容易。 此示例创建一个对话框来通知用户没有 WiFi 连接，然后使用 [ShowAsync](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.contentdialog.showasync.aspx) 方法显示它。

```csharp
private async void displayNoWifiDialog()
{
    ContentDialog noWifiDialog = new ContentDialog()
    {
        Title = "No wifi connection",
        Content = "Check connection and try again",
        PrimaryButtonText = "Ok"
    };

    ContentDialogResult result = await noWifiDialog.ShowAsync();
}
```

当用户单击某个对话框按钮时，[ShowAsync](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.contentdialog.showasync.aspx) 方法返回一个 [ContentDialogResult](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.contentdialogresult.aspx) 来通知你用户单击了哪个按钮。 

此示例中的对话框提出一个问题，并使用返回的 [ContentDialogResult](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.contentdialogresult.aspx) 确定用户的响应。 

```csharp
private async void displayDeleteFileDialog()
{
    ContentDialog deleteFileDialog = new ContentDialog()
    {
        Title = "Delete file permanently?",
        Content = "If you delete this file, you won't be able to recover it. Do you want to delete it?",
        PrimaryButtonText = "Delete",
        SecondaryButtonText = "Cancel"
    };

    ContentDialogResult result = await deleteFileDialog.ShowAsync();
    
    // Delete the file if the user clicked the primary button. 
    /// Otherwise, do nothing. 
    if (result == ContentDialogResult.Primary)
    {
        // Delete the file. 
    }
}
```

## <a name="flyouts"></a>浮出控件
###  <a name="create-a-flyout"></a>创建浮出控件

浮出控件是可显示任意 UI 作为其内容的开放式容器。 

<div class="microsoft-internal-note">
其中包括浮出控件和上下文菜单（可以嵌套在其他浮出控件内）。
</div>

浮出控件附加到特定控件。 你可以使用 [Placement](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.placement.aspx) 属性指定浮出控件显示的位置：顶部、左侧、底部、右侧或完整。 如果你选择[完整放置模式](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutplacementmode.aspx)，应用将拉伸浮出控件，并使其在应用窗口中居中。 当可见时，它们应固定到调用对象，并指定它们相对于对象的首选位置：顶部、左侧、底部或右侧。 浮出控件还有一种完整放置模式，该模式尝试拉伸浮出控件，并使其在应用窗口内居中。 某些控件（如 [Button](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx)）提供可用于关联浮出控件的 [Flyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.flyout.aspx) 属性。 

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

如果控件没有浮出控件属性，可以改用 [FlyoutBase.AttachedFlyout](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.attachedflyout.aspx) 附加属性。 当你这样做时，还需要调用 [FlyoutBase.ShowAttachedFlyout](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.showattachedflyout) 方法来显示浮出控件。 

此示例将一个简单的浮出控件添加到图像。 当用户点击该图像时，应用将显示该浮出控件。 

````xaml
<Image Source="Assets/cliff.jpg" Width="50" Height="50" 
  Margin="10" Tapped="Image_Tapped">
  <FlyoutBase.AttachedFlyout>
    <Flyout>
      <TextBlock TextWrapping="Wrap" Text="This is some text in a flyout."  />
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
        <Flyout.FlyoutPresenterStyle>
            <Style TargetType="FlyoutPresenter">
                <Setter Property="ScrollViewer.ZoomMode" Value="Enabled"/>
                <Setter Property="Background" Value="Black"/>
                <Setter Property="BorderBrush" Value="Gray"/>
                <Setter Property="BorderThickness" Value="5"/>
                <Setter Property="MinHeight" Value="300"/>
                <Setter Property="MinWidth" Value="300"/>
            </Style>
        </Flyout.FlyoutPresenterStyle>
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
若要设置浮出控件的样式，请修改其 [FlyoutPresenterStyle](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.flyout.flyoutpresenterstyle.aspx)。 此示例显示一个环绕文本段落，并使屏幕阅读器可以访问该文本块。

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

## <a name="get-the-samples"></a>获取示例
*   [XAML UI 基础知识](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlUIBasics)<br/>
    以交互式格式查看所有 XAML 控件。

## <a name="related-articles"></a>相关文章
- [工具提示](tooltips.md)
- [菜单和上下文菜单](menus.md)
- [**Flyout 类**](https://msdn.microsoft.com/library/windows/apps/dn279496)
- [**ContentDialog 类**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentdialog.aspx)

