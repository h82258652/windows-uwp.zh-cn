---
ms.assetid: eb690f2b-3bf8-4a65-99a4-2a3a8c7760b7
description: 本文介绍如何与系统媒体传输控件交互。
title: 与系统媒体传输控件集成
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c89a1901d15d00c7c102157c8f44d6ab96272ef0
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/28/2018
ms.locfileid: "7966473"
---
# <a name="integrate-with-the-system-media-transport-controls"></a>与系统媒体传输控件集成

本文介绍如何与系统媒体传输控件交互 (SMTC)。 SMTC 是一组通用于所有 Windows 10 设备的控件，并为用户提供一致的方法来控制使用 [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer) 进行播放的所有正运行应用的媒体播放。

有关演示与 SMTC 集成的完整示例，请参阅 [Github 上的系统媒体传输控件示例](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/SystemMediaTransportControls)。
                    
## <a name="automatic-integration-with-smtc"></a>与 SMTC 自动集成
从 Windows 10 版本 1607 开始，使用 [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer) 类播放媒体的 UWP 应用默认自动与 SMTC 集成。 只需实例化 **MediaPlayer** 的新实例，并将 [**MediaSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaSource)、[**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem) 或 [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackList) 分配给玩家的 [**Source**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Source) 属性，然后用户将在 SMTC 中看到你的应用名称，并且可以使用 SMTC 控件播放、暂停和在播放列表中移动。 

你的应用一次可以创建和使用多个 **MediaPlayer** 对象。 对于应用中每个活动的 **MediaPlayer** 实例，将在 SMTC 中创建单独的选项卡，从而允许用户在活动的媒体播放器和其他正在运行的应用的媒体播放器之间切换。 无论 SMTC 中当前选择哪个媒体播放器，该媒体播放器都会受到控件的影响。

有关在应用中使用 **MediaPlayer** 的详细信息，包括在 XAML 页面中将它绑定到 [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement)，请参阅[使用 MediaPlayer 播放音频和视频](play-audio-and-video-with-mediaplayer.md)。 

有关使用 **MediaSource**、**MediaPlaybackItem** 和 **MediaPlaybackList** 的详细信息，请参阅[媒体项、播放列表和曲目](media-playback-with-mediasource.md)。

## <a name="add-metadata-to-be-displayed-by-the-smtc"></a>添加由 SMTC 显示的元数据
如果你希望添加或更改为 SMTC 中的媒体项显示的元数据，你需要为表示媒体项的 **MediaPlaybackItem** 更新显示属性。 首先，通过调用 [**GetDisplayProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem.GetDisplayProperties) 获取对 [**MediaItemDisplayProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaItemDisplayProperties) 对象的引用。 接下来，为带有 [**Type**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaItemDisplayProperties.Type) 属性的项设置媒体类型（音乐或视频）。 然后，你可以填充 [**MusicProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaItemDisplayProperties.MusicProperties) 或 [**VideoProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaItemDisplayProperties.VideoProperties) 的字段，具体取决于你指定的媒体类型。 最后，通过调用 [**ApplyDisplayProperties**](https://msdn.microsoft.com/library/windows/apps/mt489923) 更新媒体项的元数据。

[!code-cs[SetVideoProperties](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetSetVideoProperties)]

[!code-cs[SetMusicProperties](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetSetMusicProperties)]

## <a name="use-commandmanager-to-modify-or-override-the-default-smtc-commands"></a>使用 CommandManager 修改或替代默认的 SMTC 命令。
你的应用可以使用 [**MediaPlaybackCommandManager**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager) 类修改或完全替代 SMTC 控件的行为。 可以通过访问 [**CommandManager**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.CommandManager) 属性来为 **MediaPlayer** 类的每个实例获取命令管理器实例。

对于每个命令，如默认跳到 **MediaPlaybackList** 中的下一项的 *Next* 命令，命令管理器会公开一个收到事件（如 [**NextReceived**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager.NextReceived)）和一个管理命令行为的对象（如 [**NextBehavior**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager.NextBehavior)）。 

以下示例为 **NextReceived** 事件和 **NextBehavior** 的 [**IsEnabledChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManagerCommandBehavior.IsEnabledChanged) 事件注册处理程序。

[!code-cs[AddNextHandler](./code/SMTC_RS1/cs/MainPage.xaml.cs#SnippetAddNextHandler)]

以下示例介绍了如下方案：应用希望在用户单击播放列表中的五个项后禁用 *Next* 命令，在继续播放内容前可能需要进行一些用户交互。 每次引发 **NextReceived** 事件时，计数器便递增。 计数器达到目标数之后，*Next* 命令的 [**EnablingRule**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManagerCommandBehavior.EnablingRule) 设置为 [**Never**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaCommandEnablingRule)，从而禁用该命令。 

[!code-cs[NextReceived](./code/SMTC_RS1/cs/MainPage.xaml.cs#SnippetNextReceived)]

你还可以将该命令设置为 **Always**，这意味着该命令将始终处于启用状态，即使在 *Next* 命令示例中，播放列表中没有更多的项也是如此。 或者你可以将该命令设置为 **Auto**，在此情况下系统会根据当前播放的内容确定是否要启用该命令。

对于上述方案，在某一时刻应用将要重新启用 *Next* 命令，并通过将 **EnablingRule** 设置为 **Auto** 来执行此操作。

[!code-cs[EnableNextButton](./code/SMTC_RS1/cs/MainPage.xaml.cs#SnippetEnableNextButton)]

由于你的应用可能有其自己的 UI，用于在应用处于前台时控制播放，你可以使用 [**IsEnabledChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManagerCommandBehavior.IsEnabledChanged) 事件来更新你自己的 UI，以便在通过访问传递到处理程序的 [**MediaPlaybackCommandManagerCommandBehavior**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManagerCommandBehavior) 的 [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManagerCommandBehavior.IsEnabled) 来启用或禁用命令时与 SMTC 匹配。

[!code-cs[IsEnabledChanged](./code/SMTC_RS1/cs/MainPage.xaml.cs#SnippetIsEnabledChanged)]

在某些情况下，可能希望完全替代 SMTC 命令的行为。 以下示例演示的方案为，应用使用 *Next* 和 *Previous* 命令在 Internet 广播电台之间切换，而不是在当前播放列表中的曲目之间跳过。 和上一个示例中一样，在接收命令时注册一个处理程序，在此情况下它是 [**PreviousReceived**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager.PreviousReceived) 事件。

[!code-cs[AddPreviousHandler](./code/SMTC_RS1/cs/MainPage.xaml.cs#SnippetAddPreviousHandler)]

在 **PreviousReceived** 处理程序中，先通过调用传递到处理程序中的 [**MediaPlaybackCommandManagerPreviousReceivedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManagerPreviousReceivedEventArgs) 的 [**GetDeferral**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManagerPreviousReceivedEventArgs.GetDeferral) 来获取 [**Deferral**](https://msdn.microsoft.com/library/windows/apps/Windows.Foundation.Deferral)。 这将告知系统等到 deferall 完成之后再执行该命令。 如果你打算在处理程序中进行异步调用，这极其重要。 此时，示例调用一个自定义方法，该方法返回一个表示上一个广播电台的 **MediaPlaybackItem**。

接下来，检查 [**Handled**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManagerPreviousReceivedEventArgs.Handled) 属性，以确保该事件未由另其他处理程序处理。 否则，**Handled** 属性设置为 true。 这使 SMTC 以及任何其他已订阅的处理程序知道它们不应采取任何措施来执行此命令，因为它已经过处理。 然后，代码为媒体播放器设置新源，并启动播放器。

最后，在 deferral 对象上调用 [**Complete**](https://msdn.microsoft.com/library/windows/apps/Windows.Foundation.Deferral.Complete) 以通知系统已完成命令处理。

[!code-cs[PreviousReceived](./code/SMTC_RS1/cs/MainPage.xaml.cs#SnippetPreviousReceived)]
                
## <a name="manual-control-of-the-smtc"></a>SMTC 的手动控制
正如本文前面提到的，SMTC 将针对你的应用创建的 **MediaPlayer** 的每个实例检测并显示信息。 如果你希望使用 **MediaPlayer** 的多个实例，但希望 SMTC 为应用提供单个条目，则必须手动控制 SMTC 的行为，而不是依靠自动集成。 此外，如果你将使用 [**MediaTimelineController**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaTimelineController) 控制一个或多个媒体播放器，则必须使用手动 SMTC 集成。 此外，如果你的应用使用 API 而不是 **MediaPlayer**（如 [**AudioGraph**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Audio.AudioGraph) 类）来播放媒体，则必须实现手动 SMTC 集成，以便使用户使用 SMTC 控制你的应用。 有关如何手动控制 SMTC 的信息，请参阅[系统媒体传输控件的手动控制](system-media-transport-controls.md)。



## <a name="related-topics"></a>相关主题
* [媒体播放](media-playback.md)
* [使用 MediaPlayer 播放音频和视频](play-audio-and-video-with-mediaplayer.md)
* [手动控制系统媒体传输控件](system-media-transport-controls.md)
* [Github 上的系统媒体传输控件示例](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/SystemMediaTransportControls)
 

 




