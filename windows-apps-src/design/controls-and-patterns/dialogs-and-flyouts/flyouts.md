---
author: mijacobs
Description: Dialogs and flyouts display transient UI elements that appear when the user requests them or when something happens that requires notification or approval.
title: 浮出控件
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: ad6affd9-a3c0-481f-a237-9a1ecd561be8
pm-contact: yulikl
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: e68f8f48ca9ba67a29c8a52a5d59767a080f642b
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/09/2018
ms.locfileid: "6196187"
---
# <a name="flyouts"></a>浮出控件

浮出控件是可显示任意 UI 作为其内容的轻型消除容器。 浮出控件可能包含其他浮出控件或上下文菜单，用于创建嵌套体验。

![嵌套在浮出控件内的上下文菜单](../images/flyout-nested.png)

> **重要的 Api**：[浮出控件类](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Flyout)

## <a name="is-this-the-right-control"></a>这是正确的控件吗？

* 不要使用浮出控件替代[工具提示](../tooltips.md)或[上下文菜单](../menus.md)。 使用工具提示显示在指定时间后隐藏的简短描述。 针对与 UI 元素相关的上下文操作（如复制和粘贴）使用上下文菜单。

建议在何时使用浮出控件与何时使用对话框 （类似控件），请参阅[对话框和浮出控件](index.md)。 

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="../images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果已安装 <strong style="font-weight: semi-bold">XAML 控件库</strong>应用，请单击此处打开该应用，了解 <a href="xamlcontrolsgallery:/item/ContentDialog">ContentDialog</a> 或 <a href="xamlcontrolsgallery:/item/Flyout">Flyout</a> 的实际应用。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

##  <a name="how-to-create-a-flyout"></a>如何创建一个浮出控件


浮出控件附加到特定控件。 你可以使用 [Placement](/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutBase.Placement) 属性指定浮出控件显示的位置：顶部、左侧、底部、右侧或完整。 如果你选择[完整放置模式](/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutPlacementMode)，应用将拉伸浮出控件，并使其在应用窗口中居中。 某些控件（如 [Button](/uwp/api/Windows.UI.Xaml.Controls.Button)）提供可用于关联浮出控件或[上下文菜单](../menus.md) 的 [Flyout](/uwp/api/Windows.UI.Xaml.Controls.Button.Flyout) 属性。

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

## <a name="style-a-flyout"></a>设置浮出控件的样式
若要设置浮出控件的样式，请修改其 [FlyoutPresenterStyle](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Flyout.FlyoutPresenterStyle)。 此示例显示一个环绕文本段落，并使屏幕阅读器可以访问该文本块。

![带有环绕文本的可访问浮出控件](../images/flyout-wrapping-text.png)

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

## <a name="styling-flyouts-for-10-foot-experiences"></a>针对 10 英尺体验的样式设置浮出控件

轻型消除控件（如浮出控件）会捕获瞬态 UI 内的键盘焦点和游戏板焦点，直到消除为止。 若要为此行为提供视觉提示，Xbox 上的轻型消除控件将绘制覆盖，以便使 UI 范围之外的对比度和可见性变暗。 可以使用 [`LightDismissOverlayMode`](/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutBase.LightDismissOverlayMode) 属性修改此行为。 默认情况下，浮出控件将在 Xbox（而非其他设备系列）上绘制轻型消除覆盖，不过应用可以选择强制使覆盖始终**打开**或始终**关闭**。

![带有变暗覆盖的浮出控件](../images/flyout-smoke.png)

```xaml
<MenuFlyout LightDismissOverlayMode="On">
```

## <a name="light-dismiss-behavior"></a>轻型消除行为
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
- [工具提示](../tooltips.md)
- [菜单和上下文菜单](../menus.md)
- [Flyout 类](/uwp/api/Windows.UI.Xaml.Controls.Flyout)
- [ContentDialog 类](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog)