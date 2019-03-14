---
ms.assetid: b7333924-d641-4ba5-92a2-65925b44ccaa
description: 本文将向你介绍当应用在后台运行时如何播放媒体。
title: 在后台播放媒体
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3f5fe7cad12193b409c4923f876b47cae0852aa9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57645812"
---
# <a name="play-media-in-the-background"></a>在后台播放媒体
本文介绍了如何配置应用，以便在应用从前台移至后台后，媒体可以继续播放。 这意味着，即使在用户已最小化你的应用、返回到主屏幕，或已以其他方式离开你的应用后，你的应用仍可继续播放音频。 

后台音频播放的方案包括：

-   **长时间运行的播放列表：** 用户简要引出了前台应用程序以选择并开始播放列表，此后用户需要继续在后台播放的播放列表。

-   **使用任务切换器：** 用户简要引出前台应用程序开始播放音频，然后切换到另一个打开的应用程序使用的任务切换器。 用户期望该音频在后台继续播放。

本文所述的后台音频实现将使你的应用通常在所有 Windows 设备（包括移动设备、桌面设备和 Xbox）上运行。

> [!NOTE]
> 本文中的代码改编自 UWP [后台音频示例](https://go.microsoft.com/fwlink/p/?LinkId=800141)。

## <a name="explanation-of-one-process-model"></a>单进程模型说明
Windows 10 版本 1607 引入的全新单进程模型极大地简化了启用后台音频的进程。 以前，需要你的应用管理后台进程以及前台应用，然后在两个进程之间手动通知状态更改。 在新的模型下，只需将后台音频功能添加到应用部件清单，该应用就会在移至后台之后自动继续播放音频。 [  **EnteredBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.EnteredBackground) 和 [**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground) 这两个新的应用程序生命周期事件使应用可以得知它何时进入和退出后台。 当你的应用移至、过渡到后台或从中移出时，系统强制执行的内存约束可能会发生改变，因此你可以使用这些事件检查当前内存消耗并释放资源，以便保持在限制之下。

通过消除复杂的进程间通信和状态管理，新模型使你可以使用明显减少的代码极快地实现后台音频。 但是，当前版本中仍支持双进程模型，以实现向后兼容性。 有关详细信息，请参阅[传统后台音频模型](legacy-background-media-playback.md)。

## <a name="requirements-for-background-audio"></a>后台音频要求
当应用在后台运行时，该应用必须满足音频播放的以下要求。

* 将**后台媒体播放**功能添加到应用部件清单，如本文中的后面部分所述。
* 如果应用禁止自动将 **MediaPlayer** 与系统媒体传输控件 (SMTC) 集成（例如，将 [**CommandManager.IsEnabled**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager.IsEnabled) 属性设置为 false），那么必须手动实现与 SMTC 的集成，才可以支持后台媒体播放。 如果使用的是 **MediaPlayer** 之外的 API（如 [**AudioGraph**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Audio.AudioGraph)），还必须手动与 SMTC 集成，才可以播放音频（如果你希望应用移至后台之后音频继续播放）。 [手动控制系统媒体传输控件](system-media-transport-controls.md)的“将系统媒体传输控件用于后台音频”部分介绍了 SMTC 集成的最低要求。
* 应用处于后台时，不得超出系统为后台应用设置的内存使用量限制。 管理后台内存的指南将在文本后面部分提供。

## <a name="background-media-playback-manifest-capability"></a>后台媒体播放清单功能
若要支持后台音频，必须将后台媒体播放功能添加到应用部件清单文件（即 Package.appxmanifest）。 

**若要将功能添加到应用程序清单使用清单设计器**

1.  在 Microsoft Visual Studio 的“解决方案资源管理器”中，通过双击“package.appxmanifest”项，打开应用程序清单的设计器。
2.  选择**功能**选项卡。
3.  选择**后台媒体播放**复选框。

若要通过手动编辑应用部件清单 xml 来设置功能，请首先确保已在 **Package** 元素中定义 *uap3* 命名空间前缀。 如果尚未定义，请如下所示进行添加。
```xml
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap uap3 mp">
```

接下来，将 *backgroundMediaPlayback* 功能添加到 **Capabilities** 元素：
```xml
<Capabilities>
    <uap3:Capability Name="backgroundMediaPlayback"/>
</Capabilities>
```

## <a name="handle-transitioning-between-foreground-and-background"></a>处理前台和后台之间的转换
当应用从前台移至后台时，将引发 [**EnteredBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.EnteredBackground) 事件。 并且当应用返回前台时，将引发 [**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground) 事件。 由于这些事件都是应用生命周期事件，因此应该在创建应用时为这些事件注册处理程序。 在默认项目模板中，这意味着将它添加到 App.xaml.cs 中的 **App** 类构造函数。 

[!code-cs[RegisterEvents](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetRegisterEvents)]

添加一个变量以跟踪你当前是否在后台运行。

[!code-cs[DeclareBackgroundMode](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetDeclareBackgroundMode)]

当引发 [**EnteredBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.EnteredBackground) 事件时，请设置跟踪变量以表明你当前在后台运行。 不得在 **EnteredBackground** 事件中执行长时间运行的任务，因为这可能会导致用户感觉过渡到后台非常慢。

[!code-cs[EnteredBackground](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetEnteredBackground)]

在 [**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground) 事件处理程序中，应该设置跟踪变量，以指示应用不再在后台运行。

[!code-cs[LeavingBackground](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetLeavingBackground)]

### <a name="memory-management-requirements"></a>内存管理要求
处理前台和后台之间转换的最重要部分是管理你的应用使用的内存。 由于在后台运行会减少系统允许应用保留的内存资源，因此还应该注册 [**AppMemoryUsageIncreased**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageIncreased) 和 [**AppMemoryUsageLimitChanging**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageLimitChanging) 事件。 引发这些事件时，你应该检查应用的当前内存使用量和当前限制，然后根据需要减少内存使用量。 有关如何在后台运行时减少内存使用量的信息，请参阅[在将应用移动到后台时释放内存](../launch-resume/reduce-memory-usage.md)。

## <a name="network-availability-for-background-media-apps"></a>后台媒体应用的网络可用性
不会从流或文件创建的所有网络感知的媒体源将使网络连接保持活动状态（在检索远程内容时），不需要检索时会释放网络连接。 [**MediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaStreamSource)，具体而言，依赖于应用程序以正确地报告为平台使用正确的缓冲的范围[ **SetBufferedRange**](https://msdn.microsoft.com/library/windows/apps/dn282762)。 完全缓存整个内容后，网络不再以应用的名义保留。

如果需要在媒体不在下载时在后台执行网络调用，则必须在诸如 [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Background.MaintenanceTrigger) 或 [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Background.TimeTrigger) 等相应任务中包括这些网络调用。 有关详细信息，请参阅[使用后台任务支持应用](https://msdn.microsoft.com/windows/uwp/launch-resume/support-your-app-with-background-tasks)。

## <a name="related-topics"></a>相关主题
* [媒体播放](media-playback.md)
* [播放音频和视频使用 MediaPlayer](play-audio-and-video-with-mediaplayer.md)
* [将与集成系统媒体传输控件](integrate-with-systemmediatransportcontrols.md)
* [背景音频示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundMediaPlayback)

 

 




