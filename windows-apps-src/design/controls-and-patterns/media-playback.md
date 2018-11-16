---
author: Jwmsft
Description: The media player is used to view and listen to video, audio, and images.
title: 媒体播放器
ms.assetid: 9AABB5DE-1D81-4791-AB47-7F058F64C491
dev.assetid: AF2F2008-9B53-430C-BBC3-8888F631B0B0
label: Media player
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6df7d7dc7d35ed46f3f741bd1783b5af2755f0a2
ms.sourcegitcommit: 9f8010fe67bb3372db1840de9f0be36097ed6258
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/16/2018
ms.locfileid: "7100660"
---
# <a name="media-player"></a>媒体播放器



媒体播放器用于观看和收听视频和音频。 媒体播放可以是嵌入式的（嵌入在页面中或使用一组其他控件嵌入），也可以位于专用全屏视图中。 你可以根据需要修改播放器的按钮组合、更改控件栏的背景以及排列布局。 只需记住，用户预期获得基本的控件集（播放/暂停、快退、快进）。

![具有传输控件的媒体播放器元素](images/controls/mtc_double_video_inprod.png)

> **重要 API**：[MediaPlayerElement 类](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx)，[MediaTransportControls 类](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediatransportcontrols)


> [!NOTE]
> **MediaPlayerElement** 仅在 Windows 10 1607 版本及更高版本中可用。 如果要针对早期版本的 Windows 10 开发应用，你将需要改用 [MediaElement](https://msdn.microsoft.com/library/windows/apps/br242926)。 此页面上的所有建议均同样适用于 MediaElement。

## <a name="is-this-the-right-control"></a>这是正确的控件吗？

当你想要在应用中播放音频或视频时，请使用媒体播放器。 若要显示图像集锦，请使用[翻转视图](flipview.md)。

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果已安装 <strong style="font-weight: semi-bold">XAML 控件库</strong>应用，请单击此处打开该应用，了解 <a href="xamlcontrolsgallery:/item/MediaPlayerElement">MediaPlayerElement</a> 或 <a href="xamlcontrolsgallery:/item/MediaPlayer">MediaPlayer</a> 的实际应用。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Windows 10 入门应用中的媒体播放器。

![Windows 10 入门应用中的媒体元素](images/control-examples/mtc_getstarted_example.png)

## <a name="create-a-media-player"></a>创建媒体播放器
通过使用 XAML 创建 [MediaPlayerElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) 对象来将媒体添加到你的应用，并将 [Source](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx) 设置为指向某个音频或视频文件的 [MediaSource](https://msdn.microsoft.com/library/windows/apps/windows.media.core.mediasource.aspx)。

该 XAML 将创建一个 [MediaPlayerElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx)，并将其 [Source](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx) 属性设置为应用的某个本地视频文件的 URI。 当加载页面时，**MediaPlayerElement** 开始播放。 若要防止媒体立即启动，可以将 [AutoPlay](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.autoplay.aspx) 属性设置为 **false**。

```xaml
<MediaPlayerElement x:Name="mediaSimple"
                    Source="Videos/video1.mp4"
                    Width="400" AutoPlay="True"/>
```

此 XAML 会创建一个 [MediaPlayerElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx)，同时启用内置传输控件并将 [AutoPlay](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.autoplay.aspx) 属性设置为 **False**。


```xaml
<MediaPlayerElement x:Name="mediaPlayer"
                    Source="Videos/video1.mp4"
                    Width="400"
                    AutoPlay="False"
                    AreTransportControlsEnabled="True"/>
```

### <a name="media-transport-controls"></a>媒体传输控件
[MediaPlayerElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) 具有处理播放、停止、暂停、音量、静音、搜寻/进度、隐藏式字幕和音轨选择的内置传输控件。 若要启用这些控件，请将 [AreTransportControlsEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.AreTransportControlsEnabled.aspx) 设置为 **true**。 若要禁用它们，请将 **AreTransportControlsEnabled** 设置为 **false**。 传输控件由 [MediaTransportControls](https://msdn.microsoft.com/library/windows/apps/dn831962) 类表示。 你可以按原样使用传输控件，或通过各种方法自定义它们。 有关详细信息，请参阅 [MediaTransportControls](https://msdn.microsoft.com/library/windows/apps/dn831962) 类引用和[创建自定义传输控件](custom-transport-controls.md)。

传输控件支持单行和双行布局。 此处的第一个示例是单行布局，播放/暂停按钮位于媒体时间线左侧。 此布局最好专用于媒体播放和紧凑屏幕。

![单行 MTC 控件示例](images/controls/mtc_single_inprod_02.png)

对于大多数使用方案，尤其是在较大的屏幕上，建议使用双行控件布局（下图）。 此布局为控件提供更多的空间，并且使用户可以更轻松地操作时间线。

![手机上的双行 MTC 控件示例](images/controls/mtc_double_inprod.png)

**系统媒体传输控件**

[MediaPlayerElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) 自动与系统媒体传输控件集成。 系统媒体传输控件是在按下硬件媒体键时弹出的控件，例如键盘上的媒体按钮。 有关详细信息，请参阅 [SystemMediaTransportControls](https://msdn.microsoft.com/library/windows/apps/dn278677)。

> **注意**&nbsp;&nbsp; [MediaElement](https://msdn.microsoft.com/library/windows/apps/br242926) 不会与系统媒体传输控件自动集成，因此你必须自行连接它们。 有关详细信息，请参阅[系统媒体传输控件](https://msdn.microsoft.com/library/windows/apps/mt228338)。


### <a name="set-the-media-source"></a>设置媒体源
若要播放网络上的文件或嵌入在应用中的文件，请将 [Source](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx) 属性设置为带有该文件路径的 [MediaSource](https://msdn.microsoft.com/library/windows/apps/windows.media.core.mediasource.aspx)。

**提示**若要打开 internet 中的文件，你需要声明你的应用的清单 (Package.appxmanifest) 中的**Internet （客户端）** 功能。 有关声明功能的详细信息，请参阅[应用功能声明](https://msdn.microsoft.com/library/windows/apps/mt270968)。

 

此代码尝试将使用 XAML 定义的 [MediaPlayerElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx) 的 [Source](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) 属性设置为输入到 [TextBox](https://msdn.microsoft.com/library/windows/apps/br209683) 中的文件路径。

```xaml
<TextBox x:Name="txtFilePath" Width="400"
         FontSize="20"
         KeyUp="TxtFilePath_KeyUp"
         Header="File path"
         PlaceholderText="Enter file path"/>
```

```csharp
private void TxtFilePath_KeyUp(object sender, KeyRoutedEventArgs e)
{
    if (e.Key == Windows.System.VirtualKey.Enter)
    {
        TextBox tbPath = sender as TextBox;

        if (tbPath != null)
        {
            LoadMediaFromString(tbPath.Text);
        }
    }
}

private void LoadMediaFromString(string path)
{
    try
    {
        Uri pathUri = new Uri(path);
        mediaPlayer.Source = MediaSource.CreateFromUri(pathUri);
    }
    catch (Exception ex)
    {
        if (ex is FormatException)
        {
            // handle exception.
            // For example: Log error or notify user problem with file
        }
    }
}
```

若要将媒体源设置为嵌入在应用中的媒体文件，请初始化一个路径以 ms-appx:/// 为前缀的 [**Uri**](https://msdn.microsoft.com/library/windows/apps/br226017)、创建带有该 Uri 的 [MediaSource](https://msdn.microsoft.com/library/windows/apps/windows.media.core.mediasource.aspx)，然后将 [Source](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx) 设置为该 Uri。 例如，对于 **Videos** 子文件夹中名为 **video1.mp4** 的文件，路径将如下所示：**ms-appx:///Videos/video1.mp4**

此代码将之前使用 XAML 定义的 [MediaPlayerElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) 的 [Source](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx) 属性设置为 **ms-appx:///Videos/video1.mp4**。

```csharp
private void LoadEmbeddedAppFile()
{
    try
    {
        Uri pathUri = new Uri("ms-appx:///Videos/video1.mp4");
        mediaPlayer.Source = MediaSource.CreateFromUri(pathUri);
    }
    catch (Exception ex)
    {
        if (ex is FormatException)
        {
            // handle exception.
            // For example: Log error or notify user problem with file
        }
    }
}
```

### <a name="open-local-media-files"></a>打开本地媒体文件
若要打开本地系统上或 OneDrive 中的文件，你可以使用 [FileOpenPicker](https://msdn.microsoft.com/library/windows/apps/br207847) 获取文件并使用 [Source](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx) 设置媒体源，或者可以以编程方式访问用户媒体文件夹。

如果你的应用需要在不进行用户交互的情况下访问**音乐**或**视频**文件夹（例如，如果要枚举用户集锦中的所有音乐或视频文件并将它们显示在你的应用中），则你需要声明**音乐库**和**视频库**功能。 有关详细信息，请参阅[音乐、图片和视频库中的文件和文件夹](https://msdn.microsoft.com/library/windows/apps/mt188703)。

[FileOpenPicker](https://msdn.microsoft.com/library/windows/apps/br207847) 不需要特殊功能即可访问本地文件系统上的文件（例如用户的**音乐**或**视频**文件夹），因为用户对所访问的文件具有完全控制权。 从安全性和隐私性角度来看，最好尽量减少你的应用使用的功能数。

**使用 FileOpenPicker 打开本地媒体**

1.  调用 [FileOpenPicker](https://msdn.microsoft.com/library/windows/apps/br207847) 以使用户选取媒体文件。

    使用 [FileOpenPicker](https://msdn.microsoft.com/library/windows/apps/br207847) 类选择媒体文件。 设置 [FileTypeFilter](https://msdn.microsoft.com/library/windows/apps/br207850) 以指定 **FileOpenPicker** 显示哪些文件类型。 调用 [PickSingleFileAsync](https://msdn.microsoft.com/library/windows/apps/jj635275) 来启动文件选取器并获取文件。

2.  使用 [SetSource](https://msdn.microsoft.com/library/windows/apps/windows.media.core.mediasource.aspx) 将所选媒体文件设置为 [MediaPlayerElement.Source](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx)。

    若要使用从 [FileOpenPicker](https://msdn.microsoft.com/library/windows/apps/br227171) 返回的 [StorageFile](https://msdn.microsoft.com/library/windows/apps/br207847)，你需要在 [MediaSource](https://msdn.microsoft.com/library/windows/apps/windows.media.core.mediasource.createfromstoragefile.aspx) 上调用 [CreateFromStorageFile](https://msdn.microsoft.com/library/windows/apps/windows.media.core.mediasource.aspx) 方法，并将其设置为[MediaPlayerElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx) 的 [Source](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx)。 然后对 [MediaPlayerElement.MediaPlayer](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.mediaplayer.aspx) 调用 [Play](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplayer.play.aspx) 以启动媒体。


此示例演示如何使用 [FileOpenPicker](https://msdn.microsoft.com/library/windows/apps/br207847) 选择文件并将该文件设置为 [MediaPlayerElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) 的 [Source](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx)。

```xaml
<MediaPlayerElement x:Name="mediaPlayer"/>
...
<Button Content="Choose file" Click="Button_Click"/>
```

```csharp
private async void Button_Click(object sender, RoutedEventArgs e)
{
    await SetLocalMedia();
}

async private System.Threading.Tasks.Task SetLocalMedia()
{
    var openPicker = new Windows.Storage.Pickers.FileOpenPicker();

    openPicker.FileTypeFilter.Add(".wmv");
    openPicker.FileTypeFilter.Add(".mp4");
    openPicker.FileTypeFilter.Add(".wma");
    openPicker.FileTypeFilter.Add(".mp3");

    var file = await openPicker.PickSingleFileAsync();

    // mediaPlayer is a MediaPlayerElement defined in XAML
    if (file != null)
    {
        mediaPlayer.Source = MediaSource.CreateFromStorageFile(file);

        mediaPlayer.MediaPlayer.Play();
    }
}
```

### <a name="set-the-poster-source"></a>设置海报源
你可以使用 [PosterSource](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.PosterSource.aspx) 属性在媒体加载前为 [MediaPlayerElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) 提供可视表示形式。 **PosterSource** 是一个屏幕截图或电影海报之类的图像，用于代替媒体显示。 **PosterSource** 将在以下情况下显示：

-   在没有设置有效的源时。 例如，未设置 [Source](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx)、**Source** 已设置为 **Null** 或者源无效（类似于发生 [MediaFailed](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplayer.mediafailed.aspx) 事件的情况）。
-   加载媒体时。 例如，设置了有效源，但尚未发生 [MediaOpened](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplayer.mediaopened.aspx) 事件。
-   当媒体流式传输到其他设备时。
-   当媒体仅限音频时。

以下是一个 [MediaPlayerElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx)，其 [Source](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx) 设置为唱片集，其 [PosterSource](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.PosterSource.aspx) 设置为唱片封面的图像。

```xaml
<MediaPlayerElement Source="Media/Track1.mp4" PosterSource="Media/AlbumCover.png"/>
```

### <a name="keep-the-devices-screen-active"></a>使设备的屏幕保持活动状态
通常，设备在用户离开时会降低屏幕亮度（并最终关闭屏幕）以延长电池使用时间，但视频应用需要保持屏幕打开状态以便用户可以观看视频。 若要阻止屏幕在不再检测到用户操作时（例如在应用播放视频时）被停用，可以调用 [DisplayRequest.RequestActive](https://msdn.microsoft.com/library/windows/apps/br241818)。 [DisplayRequest](https://msdn.microsoft.com/library/windows/apps/br241816) 类允许你指示 Windows 保持屏幕打开状态，以便用户可以观看视频。

若要节省电源并延长电池使用时间，应调用 [DisplayRequest.RequestRelease](https://msdn.microsoft.com/library/windows/apps/br241819) 以在不再需要显示请求时释放显示请求。 当你的应用移离屏幕时，Windows 会自动停用应用的活动显示请求，当你的应用回到前台时，会再次激活显示请求。

在以下情况下应该释放显示请求：

-   例如，由于用户操作、缓冲或有限带宽引起的调整需要暂停视频播放。
-   播放停止。 例如，视频播放完毕或完成演示文稿。
-   出现播放错误。 例如，存在网络连接问题或损坏的文件。

> **注意**&nbsp;&nbsp;如果 [MediaPlayerElement.IsFullWindow](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.IsFullWindow.aspx) 设置为 true 并且正在播放媒体，将自动阻止屏幕停用。

**使屏幕保持活动状态**

1.  创建一个全局 [DisplayRequest](https://msdn.microsoft.com/library/windows/apps/br241816) 变量。 将它初始化为 null。
```csharp
// Create this variable at a global scope. Set it to null.
private DisplayRequest appDisplayRequest = null;
```

2.  调用 [RequestActive](https://msdn.microsoft.com/library/windows/apps/br241818) 来通知 Windows 此应用需要屏幕保持打开状态。

3.  当视频播放停止、暂停或由于播放错误而中断时，调用 [RequestRelease](https://msdn.microsoft.com/library/windows/apps/br241819) 来释放显示请求。 当你的应用不再包含任何活动的显示请求时，Windows 将在设备未使用时通过降低屏幕亮度（并最终关闭屏幕）来延长电池使用时间。

    每个 [MediaPlayerElement.MediaPlayer](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.mediaplayer.aspx) 都有 [MediaPlaybackSession](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplaybacksession.aspx) 类型的 [PlaybackSession](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplayer.playbacksession.aspx)，用于控制媒体播放的各个方面，如 [PlaybackRate](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplaybacksession.playbackrate.aspx)、[PlaybackState](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplaybacksession.playbackstate.aspx) 和 [Position](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplaybacksession.position.aspx)。 此处，你对 [MediaPlayer.PlaybackSession](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplayer.playbacksession.aspx) 使用 [PlaybackStateChanged](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplaybacksession.playbackstatechanged.aspx) 事件检测应释放显示请求的情况。 然后，使用 [NaturalVideoHeight](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplaybacksession.naturalvideoheight.aspx) 属性确定某个音频或视频文件是否正在播放，并且仅在视频正在播放时使屏幕保持活动状态。
    ```xaml
<MediaPlayerElement x:Name="mpe" Source="Media/video1.mp4"/>
    ```

    ```csharp
    protected override void OnNavigatedTo(NavigationEventArgs e)
    {
        mpe.MediaPlayer.PlaybackSession.PlaybackStateChanged += MediaPlayerElement_CurrentStateChanged;
        base.OnNavigatedTo(e);
    }

    private void MediaPlayerElement_CurrentStateChanged(object sender, RoutedEventArgs e)
    {
        MediaPlaybackSession playbackSession = sender as MediaPlaybackSession;
        if (playbackSession != null && playbackSession.NaturalVideoHeight != 0)
        {
            if(playbackSession.PlaybackState == MediaPlaybackState.Playing)
            {
                if(appDisplayRequest == null)
                {
                    // This call creates an instance of the DisplayRequest object
                    appDisplayRequest = new DisplayRequest();
                    appDisplayRequest.RequestActive();
                }
            }
            else // PlaybackState is Buffering, None, Opening or Paused
            {
                if(appDisplayRequest != null)
                {
                      // Deactivate the displayr request and set the var to null
                      appDisplayRequest.RequestRelease();
                      appDisplayRequest = null;
                }
            }
        }

    }
    ```

### <a name="control-the-media-player-programmatically"></a>以编程方式控制媒体播放器
[MediaPlayerElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) 提供大量属性、方法和事件用于通过 [MediaPlayerElement.MediaPlayer](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.mediaplayer.aspx) 属性控制音频和视频播放。 有关属性、方法和事件的完整列表，请参阅 [MediaPlayer](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplayer.aspx) 参考页。

### <a name="advanced-media-playback-scenarios"></a>高级媒体播放方案
对于较复杂的媒体播放方案（如播放一个播放列表、在音频语言之间切换或创建自定义元数据轨），请将 [MediaPlayerElement.Source](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx) 设置为 [MediaPlaybackItem](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplaybackitem.aspx) 或 [MediaPlaybackList](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplaybacklist.aspx)。 有关如何启用各种高级的媒体功能，请参阅的详细信息的[媒体播放](https://msdn.microsoft.com/windows/uwp/audio-video-camera/media-playback-with-mediasource)页面。

### <a name="enable-full-window-video-rendering"></a>启用全屏视频呈现

设置 [IsFullWindow](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.isfullwindow.aspx) 属性以启用和禁用全屏呈现。 当你在应用中以编程方式设置全屏呈现时，应该始终使用 **IsFullWindow**，而不是手动执行。 **IsFullWindow** 将确保执行系统级优化，从而提高性能和延长电池使用时间。 如果未正确设置全屏呈现，则可能无法实现这些优化。

以下代码创建一个用于切换全屏呈现的 [AppBarButton](https://msdn.microsoft.com/library/windows/apps/dn279244)。

```xaml
<AppBarButton Icon="FullScreen"
              Label="Full Window"
              Click="FullWindow_Click"/>>
```

```csharp
private void FullWindow_Click(object sender, object e)
{
    mediaPlayer.IsFullWindow = !media.IsFullWindow;
}
```

### <a name="resize-and-stretch-video"></a>调整视频大小和拉伸视频

使用 [Stretch](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.stretch.aspx) 属性更改视频内容和/或 [PosterSource](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.postersource.aspx) 填充它所在容器的方式。 这将根据 [Stretch](https://msdn.microsoft.com/library/windows/apps/br242968) 值调整视频大小和拉伸视频。 **Stretch** 状态类似于许多电视机上的图片大小设置。 你可以将它挂起到一个按钮并允许用户选择他们喜欢的设置。

-   [None](https://msdn.microsoft.com/library/windows/apps/br242968) 显示原始大小的内容的原始分辨率。
-   [Uniform](https://msdn.microsoft.com/library/windows/apps/br242968) 在保持纵横比和图像内容的同时填充尽可能多的空间。 这可能会导致在视频的边缘出现水平和垂直黑色条。 这类似于宽屏模式。
-   [UniformToFill](https://msdn.microsoft.com/library/windows/apps/br242968) 在保持纵横比的同时填充整个空间。 这可能会导致某些图像被裁剪。 这类似于全屏模式。
-   [Fill](https://msdn.microsoft.com/library/windows/apps/br242968) 填充整个空间，但不保持纵横比。 图像不会被裁剪，但可能会发生拉伸。 这类似于拉伸模式。

![拉伸枚举值](images/Image_Stretch.jpg)

在此处，使用 [AppBarButton](https://msdn.microsoft.com/library/windows/apps/dn279244) 循环访问 [Stretch](https://msdn.microsoft.com/library/windows/apps/br242968) 选项。 **switch** 语句会检查 [Stretch](https://msdn.microsoft.com/library/windows/apps/br227422) 属性的当前状态并将其设置为 **Stretch** 枚举中的下一个值。 这使用户可以循环访问不同的拉伸状态。

```xaml
<AppBarButton Icon="Switch"
              Label="Resize Video"
              Click="PictureSize_Click" />
```

```csharp
private void PictureSize_Click(object sender, RoutedEventArgs e)
{
    switch (mediaPlayer.Stretch)
    {
        case Stretch.Fill:
            mediaPlayer.Stretch = Stretch.None;
            break;
        case Stretch.None:
            mediaPlayer.Stretch = Stretch.Uniform;
            break;
        case Stretch.Uniform:
            mediaPlayer.Stretch = Stretch.UniformToFill;
            break;
        case Stretch.UniformToFill:
            mediaPlayer.Stretch = Stretch.Fill;
            break;
        default:
            break;
    }
}
```

### <a name="enable-low-latency-playback"></a>启用低延迟播放

在 [MediaPlayerElement.MediaPlayer](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.mediaplayer.aspx) 上将 [RealTimePlayback](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplayer.realtimeplayback.aspx) 属性设置为 **true**，使媒体播放器元素能够减少播放时的初始延迟。 这对于双向通信应用很重要，并且适用于某些游戏方案。 请注意，此模式将占用更多资源，并且能源效率较低。

此示例创建一个 [MediaPlayerElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx)，并将 [RealTimePlayback](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplayer.realtimeplayback.aspx) 设置为 **true**。


```csharp
MediaPlayerElement mp = new MediaPlayerElement();
mp.MediaPlayer.RealTimePlayback = true;
```

## <a name="recommendations"></a>建议

媒体播放器支持浅色和深色主题，但深色主题可为大多数娱乐方案提供更好的体验。 深色背景可提供更好的对比度（特别是在低光照条件下），并限制控件栏以免干扰观看体验。

在播放视频内容时，通过优先使用全屏模式而不是内联模式来鼓励专注的观看体验。 全屏观看体验效果最佳，内联模式中选项受到限制。

如果你有足够的屏幕空间或要针对 10 英尺体验进行设计，请选择双行布局。 它为控件提供比紧凑的单行布局更多的空间，并且更易于在 10 体验中使用游戏板导航。

> **注意**&nbsp;&nbsp;有关针对 10 英尺体验优化应用程序的详细信息，请访问[针对 Xbox 和 TV 进行设计](../devices/designing-for-tv.md)文章。

默认控件已针对媒体播放进行优化，但你能够将所需的自定义选项添加到媒体播放器，以便为应用提供最佳的体验。 若要了解有关添加自定义控件的详细信息，请访问[创建自定义传输控件](custom-transport-controls.md)。

## <a name="get-the-sample-code"></a>获取示例代码

- [XAML 控件库示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 以交互式格式查看所有 XAML 控件。

## <a name="related-articles"></a>相关文章

- [UWP 应用的命令设计基础知识](https://msdn.microsoft.com/library/windows/apps/dn958433)
- [UWP 应用的内容设计基础知识](https://msdn.microsoft.com/library/windows/apps/dn958434)
