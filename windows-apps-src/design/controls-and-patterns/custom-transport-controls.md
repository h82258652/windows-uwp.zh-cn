---
author: Jwmsft
Description: The media player has customizable XAML transport controls to manage control of audio and video content.
title: 创建自定义媒体传输控件
ms.assetid: 6643A108-A6EB-42BC-B800-22EABD7B731B
label: Create custom media transport controls
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0486fbab829b7c4ce501f9ac20dac97e6abef595
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2018
ms.locfileid: "6282238"
---
# <a name="create-custom-transport-controls"></a>创建自定义传输控件



MediaPlayerElement 具有可自定义的 XAML 传输控件来管理通用 Windows 平台 (UWP) 应用中的音频和视频内容的控件。 下面，我们演示如何自定义 MediaTransportControls 模板。 我们将向你演示如何使用溢出菜单、添加自定义按钮和修改滑块。

> **重要 API**：[MediaPlayerElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx)，[MediaPlayerElement.AreTransportControlsEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aretransportcontrolsenabled.aspx)，[MediaTransportControls](https://msdn.microsoft.com/library/windows/apps/dn278677)

在开始操作之前，你应当先熟悉 MediaPlayerElement 和 MediaTransportControls 类。 有关详细信息，请参阅 MediaPlayerElement 控件指南。

> [!TIP]
> 本主题中的示例基于[媒体传输控件示例](http://go.microsoft.com/fwlink/p/?LinkId=620023)。 你可以下载该示例来查看和运行完整代码。

> [!NOTE]
> **MediaPlayerElement** 仅在 Windows 10 1607 版本及更高版本中可用。 如果要针对早期版本的 Windows 10 开发应用，你将需要改用 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926)。 本页面中的所有示例均同样适用于 **MediaElement**。

## <a name="when-should-you-customize-the-template"></a>你应在何时自定义模板？

**MediaPlayerElement** 具有旨在大多数视频和音频播放应用中良好工作的内置传输控件，而无需任何修改。 它们由 [**MediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.mediatransportcontrols.aspx) 类提供，其中包括用于播放、停止和导航媒体、调整音量、切换全屏、强制转换到第二台设备、启用字幕、切换音频轨以及调整播放速率的按钮。 MediaTransportControls 中包含的属性使你可以控制每个按钮的显示和启用状态。 还可以设置 [**IsCompact**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.mediatransportcontrols.iscompact.aspx) 属性，以指定控件显示为一行还是两行。

但是，在某些情况下，可能需要进一步自定义控件的外观或更改其行为。 下面提供了一些示例：
- 更改图标、滑块行为和颜色。
- 将不常使用的命令按钮移动到“溢出”菜单中。
- 更改控件调整大小时命令退出的顺序。
- 提供不在默认集中的命令按钮。

> [!NOTE]
> 如果屏幕上没有足够的控件，屏幕上可见的按钮将按照预定义的顺序退出内置的传输控件。 若要更改此顺序或将无法容纳的命令放入溢出菜单中，你将需要自定义控件。

你可以通过修改默认模板来自定义控件的外观。 若要修改控件的行为或添加新的命令，可以创建从 MediaTransportControls 派生的自定义控件。

> [!TIP]
> 可自定义控件模板是 XAML 平台的一项强大功能，但你还应当考虑所产生的后果。 当你自定义模板时，它会成为你的应用的静态部分，因此它将不会接收到 Microsoft 对该模板进行的任何平台更新。 如果已由 Microsoft 进行模板更新，你应采用新模板并将其重新修改，以便获取已更新模板所带来的好处。

## <a name="template-structure"></a>模板结构

[**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.controltemplate.aspx) 是默认样式的一部分。 传输控件的默认样式在 [**MediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.mediatransportcontrols.aspx) 类引用页中显示。 可以将此默认样式复制到你的项目，以对其进行修改。 该 ControlTemplate 被划分为类似于其他 XAML 控件模板的若干部分。
- 该模板的第一部分包含 MediaTransportControls 的各个组件的 [**Style**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.style.aspx) 定义。
- 第二部分定义 MediaTransportControls 所使用的各个视觉状态。
- 第三部分包含将各种 MediaTransportControls 元素保留在一起并定义组件布局方式的 [**Grid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.aspx)。

> [!NOTE]
> 有关修改模板的详细信息，请参阅 [控制模板]()。 可以在 IDE 中使用文本编辑器或类似编辑器打开 \(*Program Files*)\Windows Kits\10\DesignTime\CommonConfiguration\Neutral\UAP\\(*SDK version*)\Generic 中的 XAML 文件。 每个控件的默认样式和模板都在 **generic.xaml** 文件中定义。 你可以通过搜索“MediaTransportControls”找到 generic.xaml 中的 MediaTransportControls 模板。

在以下部分中，你将了解如何自定义传输控件的几个主要元素：
- [**Slider**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.slider.aspx)：允许用户在其媒体上进行推移，同时显示相关进度
- [**CommandBar**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.aspx)：包含所有按钮。
有关详细信息，请参阅 MediaTransportControls 参考主题的详细分析部分。

## <a name="customize-the-transport-controls"></a>自定义传输控件

如果你希望只修改 MediaTransportControls 的外观，可以创建一个默认控件样式和模板的副本，然后对其进行修改即可。 但是，如果你还想要添加或修改该控件的功能，你需要创建一个派生自 MediaTransportControls 的新类。

### <a name="re-template-the-control"></a>重新模板化控件

**自定义 MediaTransportControls 默认样式和模板**
1. 将默认样式从 MediaTransportControls 样式和模板复制到你的项目中的 ResourceDictionary。
2. 为 Style 提供一个 x:Key 值来标识它，如下所示。

```xaml
<Style TargetType="MediaTransportControls" x:Key="myTransportControlsStyle">
    <!-- Style content ... -->
</Style>
```

3. 向你的 UI 添加 MediaPlayerElement 和 MediaTransportControls。
4. 将 MediaTransportControls 元素的 Style 属性设置为你的自定义 Style 资源，如下所示。

```xaml
<MediaPlayerElement AreTransportControlsEnabled="True">
    <MediaPlayerElement.TransportControls>
        <MediaTransportControls Style="{StaticResource myTransportControlsStyle}"/>
    </MediaPlayerElement.TransportControls>
</MediaPlayerElement>
```

有关修改样式和模板的详细信息，请参阅[样式控件]()和[控件模板]()。

### <a name="create-a-derived-control"></a>创建派生控件

若要添加或修改传输控件的功能，必须创建一个派生自 MediaTransportControls 的新类。 名为 `CustomMediaTransportControls` 的派生类显示在[媒体传输控件示例](http://go.microsoft.com/fwlink/p/?LinkId=620023)和此页面上的其他示例中。

**创建一个派生自 MediaTransportControls 的新类**
1. 向你的项目中添加一个新类文件。
    - 在 Visual Studio 中，选择“项目”&gt;“添加类”。 随即打开“添加新项”对话框。
    - 在“添加新项”对话框中，输入类文件的名称，然后单击“添加”。 （在媒体传输控件示例中，该类命名为 `CustomMediaTransportControls`。）
2. 修改类代码以从 MediaTransportControls 类进行派生。

```csharp
public sealed class CustomMediaTransportControls : MediaTransportControls
{
}
```

3. 在项目中将 [**MediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.mediatransportcontrols.aspx) 的默认样式复制到 [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.resourcedictionary.aspx)。 这是你修改后的样式和模板。
（在媒体传输控件示例中，创建了名为“Themes”的新文件夹，并且向其中添加了名为 generic.xaml 的 ResourceDictionary 文件。）
4. 将样式的 [**TargetType**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.style.targettype.aspx) 更改为新的自定义控件类型。 （在该示例中，TargetType 更改为 `local:CustomMediaTransportControls`。）

```xaml
xmlns:local="using:CustomMediaTransportControls">
...
<Style TargetType="local:CustomMediaTransportControls">
```

5. 设置自定义类的 [**DefaultStyleKey**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.defaultstylekey.aspx)。 这将告知自定义类将 Style 与 `local:CustomMediaTransportControls` 的 TargetType 结合使用。

```csharp
public sealed class CustomMediaTransportControls : MediaTransportControls
{
    public CustomMediaTransportControls()
    {
        this.DefaultStyleKey = typeof(CustomMediaTransportControls);
    }
}
```

6. 将 [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) 添加到你的 XAML 标记并向其添加自定义传输控件。 需要注意的一点是，用于隐藏、显示、禁用和启用默认按钮的 API 仍然适用于自定义模板。

```xaml
<MediaPlayerElement Name="MediaPlayerElement1" AreTransportControlsEnabled="True" Source="video.mp4">
    <MediaPlayerElement.TransportControls>
        <local:CustomMediaTransportControls x:Name="customMTC"
                                            IsFastForwardButtonVisible="True"
                                            IsFastForwardEnabled="True"
                                            IsFastRewindButtonVisible="True"
                                            IsFastRewindEnabled="True"
                                            IsPlaybackRateButtonVisible="True"
                                            IsPlaybackRateEnabled="True"
                                            IsCompact="False">
        </local:CustomMediaTransportControls>
    </MediaPlayerElement.TransportControls>
</MediaPlayerElement>
```

现在可以修改控件样式和模板以更新自定义控件的外观，以及修改控件代码以更新其行为。

### <a name="working-with-the-overflow-menu"></a>使用“溢出”菜单

可以将 MediaTransportControls 命令按钮移动到“溢出”菜单，以便隐藏不常使用的命令，直至用户需要使用这些命令。

在 MediaTransportControls 模板中，命令按钮均包含在 [**CommandBar**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.aspx) 元素中。 命令栏具有主要命令和辅助命令的概念。 主要命令是默认情况下在控件中显示的按钮，并且始终可见（除非禁用、隐藏该按钮或没有足够空间）。 辅助命令显示在用户单击省略号 (…) 按钮时出现的溢出菜单中。 有关详细信息，请参阅[应用栏和命令栏](app-bars.md)文章。

若要将元素从命令栏主要命令移动到溢出菜单，你需要编辑 XAML 控件模板。

**将命令移动到“溢出”菜单：**
1. 在控件模板中，查找名为 `MediaControlsCommandBar` 的 CommandBar 元素。
2. 将 [**SecondaryCommands**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.secondarycommands.aspx) 部分添加到 CommandBar 的 XAML。 将其放置在 [**PrimaryCommands**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.primarycommands.aspx) 的结束标记之后。

```xaml
<CommandBar x:Name="MediaControlsCommandBar" ... >  
  <CommandBar.PrimaryCommands>
...
    <AppBarButton x:Name='PlaybackRateButton'
                    Style='{StaticResource AppBarButtonStyle}'
                    MediaTransportControlsHelper.DropoutOrder='4'
                    Visibility='Collapsed'>
      <AppBarButton.Icon>
        <FontIcon Glyph="&#xEC57;"/>
      </AppBarButton.Icon>
    </AppBarButton>
...
  </CommandBar.PrimaryCommands>
<!-- Add secondary commands (overflow menu) here -->
  <CommandBar.SecondaryCommands>
    ...
  </CommandBar.SecondaryCommands>
</CommandBar>
```

3. 若要使用命令填充此菜单，请剪切所需的 [**AppBarButton**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarbutton.aspx) 对象的 XAML 并将其从 PrimaryCommands 粘贴到 SecondaryCommands。 在此示例中，我们将 `PlaybackRateButton` 移动到溢出菜单。

4. 为按钮添加标签并删除样式设置信息，如下所示。
由于“溢出”菜单由文本按钮组成，因此必须为按钮添加文本标签，另外还要删除设置按钮高度和宽度的样式。 否则，它不会在溢出菜单中正确显示。

```xaml
<CommandBar.SecondaryCommands>
    <AppBarButton x:Name='PlaybackRateButton'
                  Label='Playback Rate'>
    </AppBarButton>
</CommandBar.SecondaryCommands>
```

> [!IMPORTANT]
> 必须仍然使按钮可见并启用它，才能在溢出菜单中使用它。 在此示例中，PlaybackRateButton 元素在溢出菜单中不可见，除非 IsPlaybackRateButtonVisible 属性是 true。 只有 IsPlaybackRateEnabled 属性是 true，才可启用它。 如上一节所示设置这些属性。

### <a name="adding-a-custom-button"></a>添加自定义按钮

你可能想要自定义 MediaTransportControls 的原因之一是要将自定义命令添加到该控件。 无论将其添加为主要命令还是辅助命令，用于创建命令按钮和修改其行为的过程都是相同的。 在[媒体传输控件示例](http://go.microsoft.com/fwlink/p/?LinkId=620023)中，“分级”按钮将添加到主要命令中。

**添加自定义命令按钮**
1. 创建 AppBarButton 对象并将其添加到控件模板中的 CommandBar。

```xaml
<AppBarButton x:Name="LikeButton"
              Icon="Like"
              Style="{StaticResource AppBarButtonStyle}"
              MediaTransportControlsHelper.DropoutOrder="3"
              VerticalAlignment="Center" />
```

    You must add it to the CommandBar in the appropriate location. (For more info, see the Working with the overflow menu section.) How it's positioned in the UI is determined by where the button is in the markup. For example, if you want this button to appear as the last element in the primary commands, add it at the very end of the primary commands list.

    You can also customize the icon for the button. For more info, see the [**AppBarButton**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarbutton.aspx) reference.

2. 在 [**OnApplyTemplate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.onapplytemplate.aspx) 替代中，从模板中获取该按钮并为其 [**Click**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.primitives.buttonbase.click.aspx) 事件注册处理程序。 此代码会写入 `CustomMediaTransportControls` 类。

```csharp
public sealed class CustomMediaTransportControls :  MediaTransportControls
{
    // ...

    protected override void OnApplyTemplate()
    {
        // Find the custom button and create an event handler for its Click event.
        var likeButton = GetTemplateChild("LikeButton") as Button;
        likeButton.Click += LikeButton_Click;
        base.OnApplyTemplate();
    }

    //...
}
```

3. 将代码添加到 Click 事件处理程序以执行在单击按钮后发生的操作。
以下是该类的完整代码。

```csharp
public sealed class CustomMediaTransportControls : MediaTransportControls
{
    public event EventHandler< EventArgs> Liked;

    public CustomMediaTransportControls()
    {
        this.DefaultStyleKey = typeof(CustomMediaTransportControls);
    }

    protected override void OnApplyTemplate()
    {
        // Find the custom button and create an event handler for its Click event.
        var likeButton = GetTemplateChild("LikeButton") as Button;
        likeButton.Click += LikeButton_Click;
        base.OnApplyTemplate();
    }

    private void LikeButton_Click(object sender, RoutedEventArgs e)
    {
        // Raise an event on the custom control when 'like' is clicked.
        var handler = Liked;
        if (handler != null)
        {
            handler(this, EventArgs.Empty);
        }
    }
}
```

**添加了“喜欢”按钮的自定义媒体传输控件**
![带有附加的喜欢按钮的自定义媒体传输控件](images/controls/mtc_double_custom_inprod.png)

### <a name="modifying-the-slider"></a>修改滑块

MediaTransportControls 的“seek”控件由 [**Slider**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.slider.aspx) 元素提供。 你可以自定义它的方法之一是更改查找行为的粒度。

由于默认查找滑块分为 100 个部分，因此查找行为仅限于这些若干区域。 可以通过从 [**MediaPlayerElement.MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplayer.mediaopened.aspx) 上的 [**MediaOpened**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) 事件处理程序的 XAML 可视化树中获取 Slider 来更改查找滑块的粒度。 此示例介绍如何使用 [**VisualTreeHelper**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.visualtreehelper.aspx) 获取对 Slider 的引用，然后在媒体大于 120 分钟时，将滑块的默认步进频率从 1% 更改为 0.1%（1000 步）。 MediaPlayerElement 名为 `MediaPlayerElement1`。

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
  MediaPlayerElement1.MediaPlayer.MediaOpened += MediaPlayerElement_MediaPlayer_MediaOpened;
  base.OnNavigatedTo(e);
}

private void MediaPlayerElement_MediaPlayer_MediaOpened(object sender, RoutedEventArgs e)
{
  FrameworkElement transportControlsTemplateRoot = (FrameworkElement)VisualTreeHelper.GetChild(MediaPlayerElement1.TransportControls, 0);
  Slider sliderControl = (Slider)transportControlsTemplateRoot.FindName("ProgressSlider");
  if (sliderControl != null && MediaPlayerElement1.NaturalDuration.TimeSpan.TotalMinutes > 120)
  {
    // Default is 1%. Change to 0.1% for more granular seeking.
    sliderControl.StepFrequency = 0.1;
  }
}
```
## <a name="related-articles"></a>相关文章

- [媒体播放](media-playback.md)
