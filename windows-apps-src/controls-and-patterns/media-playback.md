---
author: Jwmsft
Description: "媒体播放器用于观看和收听视频、音频和图像。"
title: "媒体播放器"
ms.assetid: 9AABB5DE-1D81-4791-AB47-7F058F64C491
dev.assetid: AF2F2008-9B53-430C-BBC3-8888F631B0B0
label: Media player
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 2dbc4e7fa227de3f37b8a337eded0004496dbe36

---
# 媒体播放器

媒体播放器用于观看和收听视频、音频和图像。 媒体播放可以是嵌入式的（嵌入在页面中或使用一组其他控件嵌入），也可以位于专用全屏视图中。 你可以根据需要修改播放器的按钮组合、更改控件栏的背景以及排列布局。 只需记住用户期望获得基本的控件集（播放/暂停、快退、快进）。

![具有传输控件的媒体元素](images/controls/media-transport-controls.png)

<span class="sidebar_heading" style="font-weight: bold;">重要的 API</span>

-   [**MediaElement 类**](https://msdn.microsoft.com/library/windows/apps/br242926)
-   [**MediaTransportControls 类**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediatransportcontrols)

## 这是正确的控件吗？

当你想要在应用中播放音频或视频时，请使用媒体播放器。 若要显示图像集锦，请使用[翻转视图](flipview.md)。

## 示例

Windows 10 入门应用中的媒体元素。

![Windows 10 入门应用中的媒体元素](images/control-examples/media-element-getstarted.png)

## 创建媒体播放器
通过使用 XAML 创建 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) 对象来将媒体添加到你的应用并将 [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) 设置为指向某个音频或视频文件的统一资源标识符 (URI)。

该 XAML 将创建一个 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926)，并将其 [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) 属性设置为应用的某个本地视频文件的 URI。 当加载页面时，**MediaElement** 开始播放。 若要防止媒体立即启动，可以将 [**AutoPlay**](https://msdn.microsoft.com/library/windows/apps/br227360) 属性设置为 **false**。

```xaml
<MediaElement x:Name="mediaSimple" 
              Source="Videos/video1.mp4" 
              Width="400" AutoPlay="False"/>
```

此 XAML 会创建 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926)，同时启用内置传输控件并将 [**AutoPlay**](https://msdn.microsoft.com/library/windows/apps/br227360) 属性设置为 **false**。


```csharp
<MediaElement x:Name="mediaPlayer" 
              Source="Videos/video1.mp4" 
              Width="400" 
              AutoPlay="False"
              AreTransportControlsEnabled="True"/>
```

### 媒体传输控件
MediaElement 具有内置传输控件，用于处理播放、停止、暂停、音量、静音、定位/前进，以及音轨选择。 若要启用这些控件，请将 [**AreTransportControlsEnabled**](https://msdn.microsoft.com/library/windows/apps/dn298977) 设置为 **true**。 若要禁用它们，请将 **AreTransportControlsEnabled** 设置为 **false**。 传输控件由 [**MediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn831962) 类表示。 你可以按原样使用传输控件，或通过各种方法自定义它们。 有关详细信息，请参阅 [**MediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn831962) 类引用和[创建自定义传输控件](custom-transport-controls.md)。

传输控件可使用户控制 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) 的大多数方面，但 **MediaElement** 还提供可用于控制音频和视频播放的多种属性和方法。 有关详细信息，请参阅本文后面的[以编程方式控制 MediaElement](#control_mediaelement_programmatically) 部分。

传输控件支持单行和双行布局。 此处的第一个示例是单行布局，播放/暂停按钮位于媒体时间线左侧。 此布局最适用于紧凑型屏幕。 

![手机上的单行 MTC 控件示例](images/controls_mtc_singlerow_phone.png)

对于大多数使用方案，尤其是在较大的屏幕上，建议使用双行控件布局（下图）。 此布局为控件提供更多的空间，并且使用户可以更轻松地操作时间线。

![手机上的双行 MTC 控件示例](images/controls_mtc_doublerow_phone.png)

**系统媒体传输控件**

你还可以将 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) 与系统媒体传输控件集成。 系统传输控件是在按下硬件媒体键时弹出的控件，例如键盘上的媒体按钮。 如果用户按下键盘上的暂停键，并且你的应用支持 [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677)，该应用将收到通知，你也可以进行相应操作。 有关详细信息，请参阅[系统媒体传输控件](https://msdn.microsoft.com/library/windows/apps/mt228338)。

### 设置媒体源
若要播放网络上的文件或嵌入在应用中的文件，请将 [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) 属性设置为该文件的路径。

**提示** 若要打开 Internet 中的文件，需要在应用的清单 (Package.appxmanifest) 中声明 “Internet(客户端)”****功能。 有关声明功能的详细信息，请参阅[应用功能声明](https://msdn.microsoft.com/library/windows/apps/mt270968)。

 

此代码尝试将在 XAML 中定义的 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) 的 [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) 属性设置为输入到 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) 中的文件路径。

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
        mediaPlayer.Source = pathUri;
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

若要将媒体源设置为嵌入在应用中的媒体文件，请创建一个其路径以 **ms-appx:///** 为前缀的 [**Uri**](https://msdn.microsoft.com/library/windows/apps/br226017)，然后将 [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) 设置为其值。 例如，对于 **Videos** 子文件夹中名为 **video1.mp4** 的文件，路径将如下所示：**ms-appx:///Videos/video1.mp4**

此代码将之前使用 XAML 定义的 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) 的 [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) 属性设置为 **ms-appx:///Videos/video1.mp4**。

```csharp
private void LoadEmbeddedAppFile()
{
    try
    {
        Uri pathUri = new Uri("ms-appx:///Videos/video1.mp4");
        mediaPlayer.Source = pathUri;
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

### 打开本地媒体文件
若要打开本地系统上或 OneDrive 中的文件，你可以使用 [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) 获取文件和 [**SetSource**](https://msdn.microsoft.com/library/windows/apps/br244338) 以设置媒体源，或者可以以编程方式访问用户媒体文件夹。

如果你的应用需要在不进行用户交互的情况下访问**音乐**或**视频**文件夹（例如，如果要枚举用户集锦中的所有音乐或视频文件并将它们显示在你的应用中），则你需要声明**音乐库**和**视频库**功能。 有关详细信息，请参阅[音乐、图片和视频库中的文件和文件夹](https://msdn.microsoft.com/library/windows/apps/mt188703)。

[**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) 不需要特殊功能即可访问本地文件系统上的文件（例如用户的**音乐**或**视频**文件夹），因为用户对所访问的文件具有完全控制权。 从安全性和隐私性角度来看，最好尽量减少你的应用使用的功能数。

**使用 FileOpenPicker 打开本地媒体**

1.  调用 [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) 以使用户选取媒体文件。

    使用 [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) 类选择媒体文件。 设置 [**FileTypeFilter**](https://msdn.microsoft.com/library/windows/apps/br207850) 以指定 **FileOpenPicker** 显示哪些文件类型。 调用 [**PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/jj635275) 来启动文件选取器并获取文件。

2.  调用 [**SetSource**](https://msdn.microsoft.com/library/windows/apps/br244338) 以将所选媒体文件设置为 [**MediaElement.Source**](https://msdn.microsoft.com/library/windows/apps/br227419)。

    若要将 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) 的 [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) 设置为从 [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) 返回的 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)，你需要打开一个流。 对 **StorageFile** 调用 [**OpenAsync**](https://msdn.microsoft.com/library/windows/apps/dn889851) 方法，这将返回你可以传递到 [**MediaElement.SetSource**](https://msdn.microsoft.com/library/windows/apps/br244338) 方法中的流。 然后对 **MediaElement** 调用 [**Play**](https://msdn.microsoft.com/library/windows/apps/br227402) 以启动媒体。

此示例演示如何使用 [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) 选择文件并将该文件设置为 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) 的 [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419)。

```xaml
<MediaElement x:Name="mediaPlayer"/>
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
    
    // mediaPlayer is a MediaElement defined in XAML
    if (file != null)
    {
        var stream = await file.OpenAsync(Windows.Storage.FileAccessMode.Read);
        mediaPlayer.SetSource(stream, file.ContentType);

        mediaPlayer.Play();
    }
}
```

### 设置海报源
你可以使用 [**PosterSource**](https://msdn.microsoft.com/library/windows/apps/br227409) 属性在媒体加载前为 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) 提供可视表示形式。 **PosterSource** 是一个图像，例如屏幕截图或电影海报，它将代替媒体显示。 **PosterSource** 将在以下情况下显示：

-   在没有设置有效的源时。 例如，未设置 [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419)、**Source** 已设置为 **Null** 或者源无效（类似于发生 [**MediaFailed**](https://msdn.microsoft.com/library/windows/apps/br227393) 事件的情况）。
-   加载媒体时。 例如，设置了有效源，但尚未发生 [**MediaOpened**](https://msdn.microsoft.com/library/windows/apps/br227394) 事件。
-   当媒体流式传输到其他设备时。
-   当媒体仅限音频时。

以下是一个 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926)，其 [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) 设置为唱片集，其 [**PosterSource**](https://msdn.microsoft.com/library/windows/apps/br227409) 设置为唱片封面的图像。

```xaml
<MediaElement Source="Media/Track1.mp4" PosterSource="Media/AlbumCover.png"/> 
```

### 使设备的屏幕保持活动状态
通常，设备在用户离开时会降低屏幕亮度（并最终关闭屏幕）以延长电池使用时间，但视频应用需要保持屏幕打开以便用户可以观看视频。 若要阻止屏幕在不再检测到用户操作时（例如在应用播放全屏视频时）被停用，可以调用 [**DisplayRequest.RequestActive**](https://msdn.microsoft.com/library/windows/apps/br241818)。 [**DisplayRequest**](https://msdn.microsoft.com/library/windows/apps/br241816) 类允许你指示 Windows 保持屏幕打开，以便用户可以观看视频。

若要节省电源并延长电池使用时间，应调用 [**DisplayRequest.RequestRelease**](https://msdn.microsoft.com/library/windows/apps/br241819) 以在不再需要显示请求时释放显示请求。 当你的应用移离屏幕时，Windows 会自动停用应用的活动显示请求，当你的应用回到前台时，会再次激活显示请求。

在以下情况下应该释放显示请求：

-   例如，由于用户操作、缓冲或有限带宽引起的调整需要暂停视频播放。
-   播放停止。 例如，视频播放完毕或完成演示文稿。
-   出现播放错误。 例如，网络连接问题或损坏的文件。

**使屏幕保持活动状态**

1.  创建一个全局 [**DisplayRequest**](https://msdn.microsoft.com/library/windows/apps/br241816) 变量。 将它初始化为 null。
```csharp
// Create this variable at a global scope. Set it to null.
private DisplayRequest appDisplayRequest = null;
```

2.  调用 [**RequestActive**](https://msdn.microsoft.com/library/windows/apps/br241818) 来通知 Windows 此应用需要屏幕保持打开状态。

3.  当视频播放停止、暂停或由于播放错误而中断时，调用 [**RequestRelease**](https://msdn.microsoft.com/library/windows/apps/br241819) 来释放显示请求。 当你的应用不再包含任何活动的显示请求时，Windows 将在设备未使用时通过降低屏幕亮度（并最终关闭屏幕）来延长电池使用时间。

    在此处，你使用 [**CurrentStateChanged**](https://msdn.microsoft.com/library/windows/apps/br227375) 事件检测到这些情况。 然后，使用 [**IsAudioOnly**](https://msdn.microsoft.com/library/windows/apps/hh965334) 属性确定某个音频或视频文件是否正在播放，并且使屏幕仅在视频正在播放时保持活动状态。
    ```xaml
<MediaElement Source="Media/video1.mp4"
              CurrentStateChanged="MediaElement_CurrentStateChanged"/>
    ```
 
    ```csharp
private void MediaElement_CurrentStateChanged(object sender, RoutedEventArgs e)
{
    MediaElement mediaElement = sender as MediaElement;
    if (mediaElement != null && mediaElement.IsAudioOnly == false)
    {
        if (mediaElement.CurrentState == Windows.UI.Xaml.Media.MediaElementState.Playing)
        {                
            if (appDisplayRequest == null)
            {
                // This call creates an instance of the DisplayRequest object. 
                appDisplayRequest = new DisplayRequest();
                appDisplayRequest.RequestActive();
            }
        }
        else // CurrentState is Buffering, Closed, Opening, Paused, or Stopped. 
        {
            if (appDisplayRequest != null)
            {
                // Deactivate the display request and set the var to null.
                appDisplayRequest.RequestRelease();
                appDisplayRequest = null;
            }
        }            
    }
} 
    ```

### 以编程方式控制媒体播放器
[**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) 提供了许多用于控制音频和视频播放的属性、方法和事件。 有关属性、方法和事件的完整列表，请参阅 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) 参考页。
    

### 选择不同语言的音轨

使用 [**AudioStreamIndex**](https://msdn.microsoft.com/library/windows/apps/br227358) 属性和 [**GetAudioStreamLanguage**](https://msdn.microsoft.com/library/windows/apps/br227384) 方法将音频更改到视频上的一个不同语言的音轨。 视频还可以包含使用同一语言的多个音轨，例如有关电影的导演评论。 此示例专门演示如何在不同语言之间进行切换，但你可以修改此代码以在任何音轨之间切换。

**选择使用不同语言的音轨**

1.  获取音轨。

    若要搜索特定语言的音轨，你可以迭代浏览视频上的每个音轨。 使用 [**AudioStreamCount**](https://msdn.microsoft.com/library/windows/apps/br227356) 作为 **for** 循环的最大值。

2.  获取音轨的语言。

    使用 [**GetAudioStreamLanguage**](https://msdn.microsoft.com/library/windows/apps/br227384) 方法获取音轨的语言。 音轨的语言由[语言代码](http://msdn.microsoft.com/library/ms533052(vs.85).aspx)标识，例如 **“en”** 表示英语，**“ja”** 表示日语。

3.  设置活动音轨。

    当你找到具有目标语言的音轨时，将 [**AudioStreamIndex**](https://msdn.microsoft.com/library/windows/apps/br227358) 设置为音轨的索引。 通过将 **AudioStreamIndex** 设置为 **null**，可选择由内容定义的默认音轨。

下面的一些代码尝试将音轨设置为指定的语言。 它将迭代浏览 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) 对象上的音轨，并使用 [**GetAudioStreamLanguage**](https://msdn.microsoft.com/library/windows/apps/br227384) 获取每个音轨的语言。 如果所需的语言音轨存在，则 [**AudioStreamIndex**](https://msdn.microsoft.com/library/windows/apps/br227358) 将设置为该音轨的索引。

```csharp
/// <summary>
/// Attemps to set the audio track of a video to a specific language
/// </summary>
/// <param name="lcid">The id of the language. For example, "en" or "ja"</param>
/// <returns>true if the track was set; otherwise, false.</returns>
private bool SetAudioLanguage(string lcid, MediaElement media)
{
    bool wasLanguageSet = false;

    for (int index = 0; index < media.AudioStreamCount; index++)
    {
        if (media.GetAudioStreamLanguage(index) == lcid)
        {
            media.AudioStreamIndex = index;
            wasLanguageSet = true;
        }
    }

    return wasLanguageSet;
}
```

### 启用全屏视频呈现

设置 [**IsFullWindow**](https://msdn.microsoft.com/library/windows/apps/dn298980) 属性以启用和禁用全屏呈现。 当你在应用中以编程方式设置全屏呈现时，应该始终使用 **IsFullWindow**，而不是手动执行。 **IsFullWindow** 将确保执行系统级优化，从而提高性能和延长电池使用时间。 如果未正确设置全屏呈现，则可能无法实现这些优化。

以下代码创建一个用于切换全屏呈现的 [**AppBarButton**](https://msdn.microsoft.com/library/windows/apps/dn279244)。

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

### 调整视频大小及拉伸视频

使用 [**Stretch**](https://msdn.microsoft.com/library/windows/apps/br227422) 属性更改视频内容填充它所在容器的方式。 这将根据 [**Stretch**](https://msdn.microsoft.com/library/windows/apps/br242968) 值调整视频大小及拉伸视频。 **Stretch** 状态类似于许多电视机上的图片大小设置。 你可以将它挂起到一个按钮并允许用户选择他们喜欢的设置。

-   [**None**](https://msdn.microsoft.com/library/windows/apps/br242968) 显示原始大小的内容的原始分辨率。
-   [**Uniform**](https://msdn.microsoft.com/library/windows/apps/br242968) 在保持纵横比和图像内容的同时填充尽可能多的空间。 这可能会导致在视频的边缘出现水平和垂直黑色条。 这类似于宽屏模式。
-   [**UniformToFill**](https://msdn.microsoft.com/library/windows/apps/br242968) 在保持纵横比的同时填充整个空间。 这可能会导致某些图像被裁剪。 这类似于全屏模式。
-   [**Fill**](https://msdn.microsoft.com/library/windows/apps/br242968) 填充整个空间，但不保持纵横比。 图像不会被裁剪，但可能会发生拉伸。 这类似于拉伸模式。

![拉伸枚举值](images/Image_Stretch.jpg) 在此处，使用 [**AppBarButton**](https://msdn.microsoft.com/library/windows/apps/dn279244) 循环访问 [**Stretch**](https://msdn.microsoft.com/library/windows/apps/br242968) 选项。 **switch** 语句会检查 [**Stretch**](https://msdn.microsoft.com/library/windows/apps/br227422) 属性的当前状态并将其设置为 **Stretch** 枚举中的下一个值。 这使用户可以循环访问不同的拉伸状态。

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

### 启用低延迟播放

在 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) 上将 [**RealTimePlayback**](https://msdn.microsoft.com/library/windows/apps/br227414) 属性设置为 **true**，以使媒体元素能够减少播放时的初始延迟。 这对于双向通信应用很重要，并且适用于某些游戏方案。 请注意，此模式将占用更多资源，并且能源效率较低。

此示例将会创建一个 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) 并将 [**RealTimePlayback**](https://msdn.microsoft.com/library/windows/apps/br227414) 设置为 **true**。

```xaml
<MediaElement x:Name="mediaPlayer" RealTimePlayback="True"/>
```

```csharp
MediaElement mediaPlayer = new MediaElement();
mediaPlayer.RealTimePlayback = true;
```
    
## 建议 

媒体播放器最初采用深色主题和浅色主题，但在大多数情况下都选择深色主题。 深色背景可提供更好的对比度（特别是在低光照条件下），并限制控件栏以免干扰观看体验。

通过优先使用全屏模式而不是内联模式来支持专属观看体验。 全屏观看体验效果最佳，而选项都限制在内联模式中。

如果你有足够的屏幕空间，则使用双行布局。 与紧凑的单行布局相比，它可为控件提供更多空间。

将所需的任何自定义选项都添加到媒体播放器中，为你的应用提供最佳体验，但请牢记以下事项：

-   限制已针对媒体播放体验而进行优化的默认控件的自定义。
-   在手机和其他移动设备上，设备镶边仍是黑色，但在笔记本电脑和台式机上，设备镶边继承用户主题色。
-   尽量不要使控件栏承载过多选项。
-   不要将媒体时间线压缩到小于默认最小大小，这会严重限制其有效性。

## 相关文章

- [UWP 应用的命令设计基础知识](https://msdn.microsoft.com/library/windows/apps/dn958433)
- [UWP 应用的内容设计基础知识](https://msdn.microsoft.com/library/windows/apps/dn958434)



<!--HONumber=Jun16_HO4-->


