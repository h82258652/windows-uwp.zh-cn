---
author: drewbatgit
ms.assetid: EFCF84D0-2F4C-454D-97DA-249E9EAA806C
description: 通过 SystemMediaTransportControls 类，你的应用可以使用内置于 Windows 的系统媒体传输控件，并更新控件显示的有关你的应用当前播放的媒体的元数据。
title: 系统媒体传输控件
---

# 系统媒体传输控件

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


通过 [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677) 类，你的应用可以使用内置于 Windows 的系统媒体传输控件，并更新控件显示的有关你的应用当前播放的媒体的元数据。

系统传输控件不同于 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) 对象上的传输控件。 系统传输控件是在按下硬件媒体键时弹出的控件，例如耳机上的音量控制器，或者键盘上的媒体按钮。 如果用户按了键盘上的暂停键，并且你的应用支持 [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677)，该应用将收到通知，你也可以进行相应操作。

你的应用还可以更新媒体信息，例如 [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677) 显示的歌曲标题和缩略图。

**注意**  
[系统媒体传输控件 UWP 示例](http://go.microsoft.com/fwlink/?LinkId=619488)可实现在本概述中所讨论的代码。 你可以下载该示例以查看上下文中的代码，或将该示例用作你自己的应用的起点。

## 设置传输控件

在页面的 XAML 文件中，定义将由系统媒体传输控件控制的 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926)。 [
            **CurrentStateChanged**](https://msdn.microsoft.com/library/windows/apps/br227375) 和 [**MediaOpened**](https://msdn.microsoft.com/library/windows/apps/br227394) 事件用于更新系统媒体传输控件，并且将在本文的后面部分中讨论。

[!code-xml[MediaElementSystemMediaTransportControls](./code/SMTCWin10/cs/MainPage.xaml#SnippetMediaElementSystemMediaTransportControls)]

将按钮添加到 XAML 文件，从而允许用户选择要播放的文件。

[!code-xml[OpenButton](./code/SMTCWin10/cs/MainPage.xaml#SnippetOpenButton)]

在代码隐藏页面中，为以下命名空间添加 using 指令。

[!code-cs[命名空间](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetNamespace)]

添加使用 [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) 的按钮单击处理程序以允许用户选择某个文件，然后调用 [**SetSource**](https://msdn.microsoft.com/library/windows/apps/br244338) 以使其成为 **MediaElement** 的活动文件

[!code-cs[OpenMediaFile](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetOpenMediaFile)]

通过调用 [**GetForCurrentView**](https://msdn.microsoft.com/library/windows/apps/dn278708) 获取 [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677) 的实例

通过设置 **SystemMediaTransportControls** 对象的相应“已启用”属性（例如 [**IsPlayEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278714)、[**IsPauseEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278713)、[**IsNextEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278712) 和 [**IsPreviousEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278715)），启用应用将使用的按钮。 有关可用控件的完整列表，请参阅 **SystemMediaTransportControls** 参考文档。

为 [**ButtonPressed**](https://msdn.microsoft.com/library/windows/apps/dn278706) 事件注册处理程序，以在用户按下按钮时接收通知。

[!code-cs[SystemMediaTransportControlsSetup](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetSystemMediaTransportControlsSetup)]

## 处理系统媒体传输控件按钮按下操作

当按下了所启用的某个按钮时，系统传输控件会引发 [**ButtonPressed**](https://msdn.microsoft.com/library/windows/apps/dn278706) 事件。 传入到事件处理程序中的 [**SystemMediaTransportControlsButtonPressedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn278683) 的 [**Button**](https://msdn.microsoft.com/library/windows/apps/dn278685) 属性是 [**SystemMediaTransportControlsButton**](https://msdn.microsoft.com/library/windows/apps/dn278681) 枚举的成员，用于指示按下了哪个已启用的按钮。

要从 [**ButtonPressed**](https://msdn.microsoft.com/library/windows/apps/dn278706) 事件处理程序更新 UI 线程上的对象（例如 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) 对象），你必须通过 [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) 来封送调用。 这是因为 **ButtonPressed** 事件处理程序不会从 UI 线程中调用，因此如果你尝试直接修改 UI，将会引发异常。

[!code-cs[SystemMediaTransportControlsButtonPressed](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetSystemMediaTransportControlsButtonPressed)]

## 使用当前的媒体状态更新系统媒体传输控件

当媒体的状态发生更改时，应该通知 [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677)，以便系统可以更新控件来反映当前状态。 若要执行此操作，请将 [**PlaybackStatus**](https://msdn.microsoft.com/library/windows/apps/dn278719) 属性设置为 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) 的 [**CurrentStateChanged**](https://msdn.microsoft.com/library/windows/apps/br227375) 事件内的适当 [**MediaPlaybackStatus**](https://msdn.microsoft.com/library/windows/apps/dn278665) 值，该事件将在媒体状态更改时引发。

[!code-cs[SystemMediaTransportControlsStateChange](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetSystemMediaTransportControlsStateChange)]

## 使用媒体信息和缩略图更新系统媒体传输控件

使用 [**SystemMediaTransportControlsDisplayUpdater**](https://msdn.microsoft.com/library/windows/apps/dn278686) 类来更新传输控件显示的媒体信息，例如当前播放的媒体项的歌曲标题或唱片集画面。 使用 [**SystemMediaTransportControls.DisplayUpdater**](https://msdn.microsoft.com/library/windows/apps/dn278707) 属性获取此类的实例。 对于典型方案，传递元数据的建议方法是调用 [**CopyFromFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn278694)，从而传入当前播放的媒体文件。 显示更新程序将自动从文件中提取元数据和缩略图图像。

调用 [**Update**](https://msdn.microsoft.com/library/windows/apps/dn278701) 会导致系统媒体传输控件使用新的元数据和缩略图更新其 UI。

[!code-cs[SystemMediaTransportControlsUpdater](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetSystemMediaTransportControlsUpdater)]

如果你的方案需要它，你可以通过设置 [**DisplayUpdater**](https://msdn.microsoft.com/library/windows/apps/dn278707) 类公开的 [**MusicProperties**](https://msdn.microsoft.com/library/windows/apps/dn278696)、[**ImageProperties**](https://msdn.microsoft.com/library/windows/apps/dn278695) 或 [**VideoProperties**](https://msdn.microsoft.com/library/windows/apps/dn278702) 对象的值，手动更新由系统媒体传输控件显示的元数据。

[!code-cs[SystemMediaTransportControlsUpdaterManual](./code/SMTCWin10/cs/MainPage.xaml.cs#SystemMediaTransportControlsUpdaterManual)]

## 更新系统媒体传输控件时间线属性

系统传输控件显示有关当前播放的媒体项时间线的信息，包括媒体项的当前播放位置、开始时间和结束时间。 若要更新系统传输控件时间线属性，请创建一个新的 [**SystemMediaTransportControlsTimelineProperties**](https://msdn.microsoft.com/library/windows/apps/mt218746) 对象。 设置对象的属性以反映正在播放的媒体项的当前状态。 调用 [**SystemMediaTransportControls.UpdateTimelineProperties**](https://msdn.microsoft.com/library/windows/apps/mt218760) 会导致控件更新时间线。

[!code-cs[UpdateTimelineProperties](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetUpdateTimelineProperties)]

-   必须提供 [**StartTime**](https://msdn.microsoft.com/library/windows/apps/mt218751)、[**EndTime**](https://msdn.microsoft.com/library/windows/apps/mt218747) 和 [**Position**](https://msdn.microsoft.com/library/windows/apps/mt218755) 的值，才能使系统控件显示正在播放的项的时间线。

-   [
            **MinSeekTime**](https://msdn.microsoft.com/library/windows/apps/mt218749) 和 [**MaxSeekTime**](https://msdn.microsoft.com/library/windows/apps/mt218748) 允许你指定时间线内用户可以查找的范围。 此操作的典型方案是允许内容提供商在其媒体中包含广告中断。

    你必须设置 [**MinSeekTime**](https://msdn.microsoft.com/library/windows/apps/mt218749) 和 [**MaxSeekTime**](https://msdn.microsoft.com/library/windows/apps/mt218748)，才能引发 [**PositionChangeRequest**](https://msdn.microsoft.com/library/windows/apps/mt218755)。

-   建议你通过在播放期间每隔大约 5 秒更新这些属性，然后在播放状态发生更改（例如暂停或查找一个新位置）时再次执行此操作，使系统控件和媒体播放保持同步。

## 响应播放器属性更改

还有一组与媒体播放器本身（而不是正在播放的媒体项的状态）相关的系统传输控件属性。 其中每个属性都与用户调整关联的控件时引发的事件相匹配。 这些属性和事件包括：

| 属性                                                                  | 事件                                                                                                   |
|---------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|
| [**AutoRepeatMode**](https://msdn.microsoft.com/library/windows/apps/mt218753) | [**AutoRepeatModeChangeRequested**](https://msdn.microsoft.com/library/windows/apps/mt218754) |
| [**PlaybackRate**](https://msdn.microsoft.com/library/windows/apps/mt218756)     | [**PlaybackRateChangeRequested**](https://msdn.microsoft.com/library/windows/apps/mt218757)     |
| [**ShuffleEnabled**](https://msdn.microsoft.com/library/windows/apps/mt218758) | [**ShuffleEnabledChangeRequested**](https://msdn.microsoft.com/library/windows/apps/mt218759) |

 
若要处理用户与其中一个控件的交互，请先为关联的事件注册一个处理程序。

[!code-cs[RegisterPlaybackChangedHandler](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetRegisterPlaybackChangedHandler)]

在该事件的处理程序中，首先确保请求的值在有效的预期范围内。 如果在此范围内，则在 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) 上设置相应的属性，然后在 [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677) 对象上设置相应的属性。

[!code-cs[PlaybackChangedHandler](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetPlaybackChangedHandler)]

-   为了引发其中一个播放器属性事件，必须为该属性设置初始值。 例如，直到你为 [**PlaybackRate**](https://msdn.microsoft.com/library/windows/apps/mt218756) 属性设置了某个值至少一次后，才会引发 [**PlaybackRateChangeRequested**](https://msdn.microsoft.com/library/windows/apps/mt218757)。

## 将系统媒体传输控件用于后台音频

若要将系统媒体传输控件用于后台音频，你必须通过将 [**IsPlayEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278714) 和 [**IsPauseEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278713) 设置为 true 来启用播放和暂停按钮。 你的应用还必须处理 [**ButtonPressed**](https://msdn.microsoft.com/library/windows/apps/dn278706) 事件。

若要从应用的后台任务内获取 [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677) 的实例，你必须使用 [**BackgroundMediaPlayer.Current.SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn926635)（而非 [**GetForCurrentView**](https://msdn.microsoft.com/library/windows/apps/dn278708)），并且仅可以从你的前台应用内使用它。

有关在后台播放音频的详细信息，请参阅[后台音频](background-audio.md)

 

 






<!--HONumber=May16_HO2-->


