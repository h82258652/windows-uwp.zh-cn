---
ms.assetid: ''
description: 演示如何在 UWP 应用中录制游戏音频、视频和元数据。
title: 捕获游戏音频、视频、屏幕截图和元数据
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, 游戏, 捕获, 音频, 视频, 元数据
ms.localizationpriority: medium
ms.openlocfilehash: c4d4d764395d7f383e9cefcb9d8b1121db098780
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57601932"
---
# <a name="capture-game-audio-video-screenshots-and-metadata"></a>捕获游戏音频、视频、屏幕截图和元数据
本文介绍如何捕获游戏视频、音频和屏幕截图，以及如何提交元数据。系统将该元数据嵌入到捕获和广播的媒体中，使你的应用和其他人可以创建被同步到游戏事件的动态体验。 

有两种不同方法可以在 UWP 应用中捕获游戏。 用户可以使用内置的系统 UI 启动捕获。 使用此方法捕获的媒体被引入到 Microsoft 游戏生态系统中，可以通过 Xbox 应用等第一方体验进行查看和共享，并且不直接提供给你的应用或用户。 本文的第一部分将向你演示如何启用和禁用由系统实现的应用捕获以及当应用捕获开始或停止时如何接收通知。

捕获媒体的另外一个方法是使用 **[Windows.Media.AppRecording](https://docs.microsoft.com/uwp/api/windows.media.apprecording)** 命名空间的 API。 如果在设备上启用了捕获，你的应用可以开始捕获游戏，在经过一段时间后，你可以停止捕获，此时媒体被写入文件。 如果用户已启用历史捕获，你也可以通过指定过去的开始时间以及要录制的持续时间来录制已经发生的游戏。 这两种方法都可以生成一个可供你的应用访问的视频文件，并且根据你选择保存文件的位置，也可供用户访问。 本文的中间部分向你演示这些方案的实现。

**[Windows.Media.Capture](https://docs.microsoft.com/uwp/api/windows.media.capture)** 命名空间用于创建元数据的 API，这些元数据描述被捕获或广播的游戏。 它可以包括文本或数字值，并且具有一个标识各数据项的文本标签。 元数据可以表示发生在某一时刻的“事件”，如当用户在赛车游戏中完成一圈时，或者可以表示持续一段时间的“状态”，如用户当前所处的游戏地图。 元数据被写入到缓存中，由系统为你的应用进行分配和管理。 元数据被嵌入到广播流和捕获的视频文件中，包括内置系统捕获或自定义应用捕获方法。 本文的最后部分向你演示如何写入游戏元数据。

> [!NOTE] 
> 游戏元数据可以嵌入到媒体文件中，而且这些媒体文件有可能不受用户控制而通过网络进行共享，因此你不能在元数据中包括个人身份信息或其他潜在的敏感数据。


## <a name="enable-and-disable-system-app-capture"></a>启用和禁用系统应用捕获
系统应用捕获由用户使用内置系统 UI 启动。 文件由 Windows 游戏生态系统引入，不提供给你的应用或用户，除非通过 Xbox 应用等第一方体验。 你的应用可以禁用和启用由系统启动的应用捕获，这允许你阻止用户捕获某些内容或游戏。 

若要启用或禁用系统应用捕获，只需调用静态方法 **[AppCapture.SetAllowedAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapture.setallowedasync)** 并传递 **false** 禁用捕获或传递 **true** 启用捕获。

[!code-cpp[SetAppCaptureAllowed](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetSetAppCaptureAllowed)]


## <a name="receive-notifications-when-system-app-capture-starts-and-stops"></a>当系统应用捕获开始和停止时接收通知
若要在系统应用捕获开始或结束时接收通知，请先通过调用工厂方法 **[GetForCurrentView](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapture.GetForCurrentView)** 获取 **[AppCapture](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapture)** 类的一个实例。 接下来，为 **[CapturingChanged](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapture.CapturingChanged)** 事件注册处理程序。

[!code-cpp[RegisterCapturingChanged](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetRegisterCapturingChanged)]

在 **CapturingChanged** 事件的处理程序中，你可以查看 **[IsCapturingAudio](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapture.IsCapturingAudio)** 和 **[IsCapturingVideo](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapture.IsCapturingVideo)** 属性，以确定是否正在分别捕获音频或视频。 你可能想要更新你的应用的 UI，以指示当前的捕获状态。

[!code-cpp[OnCapturingChanged](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetOnCapturingChanged)]

## <a name="add-the-windows-desktop-extensions-for-the-uwp-to-your-app"></a>将适用于 UWP 的 Windows 桌面扩展添加到你的应用
录制音频和视频以及直接从你的应用捕获屏幕截图的 API 位于 **[Windows.Media.AppRecording](https://docs.microsoft.com/uwp/api/windows.media.apprecording)** 命名空间中，不包括在通用 API 协定中。 若要访问 API，你必须按照以下步骤将适用于 UWP 的 Windows 桌面扩展的引用添加到你的应用。

1. 在 Visual Studio 的**解决方案资源管理器**中，展开你的 UWP 项目并右键单击**引用**，然后选择**添加引用...** 。 
2. 展开**通用 Windows** 节点并选择**扩展**。
3. 在扩展列表中，选中与你的项目的目标版本匹配的**适用于 UWP 的 Windows 桌面扩展**条目旁边的复选框。 对于应用广播功能，版本必须为 1709 或更高版本。
4. 单击“确定”  。

## <a name="get-an-instance-of-apprecordingmanager"></a>获取 AppRecordingManager 的一个实例
**[AppRecordingManager](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingmanager)** 类是用于管理应用录制的中央 API。 通过调用工厂方法 **[GetDefault](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingmanager.GetDefault)** 来获取此类的一个实例。 使用 **Windows.Media.AppRecording** 命名空间中的任何 API 之前，应检查该 API 在当前设备上是否存在。 在操作系统版本早于 Windows 10 版本 1709 的设备上，这些 API 不可用。 也可以不检查具体的操作系统版本，改用 **[ApiInformation.IsApiContractPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent)** 方法查询 *Windows.Media.AppBroadcasting.AppRecordingContract* 版本 1.0。 如果此协定存在，则录制 API 在该设备上可用。 本文中的示例代码检查 API 一次，然后在执行后续操作前检查 **AppRecordingManager** 是否为空。

[!code-cpp[GetAppRecordingManager](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetGetAppRecordingManager)]

## <a name="determine-if-your-app-can-currently-record"></a>确定你的应用当前是否可以录制
有几种原因导致你的应用当前可能无法捕获音频或视频，包括当前设备不满足录制所需的硬件要求，或另一个应用当前正在广播。 在开始录制前，你可以检查你的应用当前是否能够录制。 调用 **AppRecordingManager** 对象的 **[GetStatus](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingmanager.GetStatus)** 方法，然后检查返回的 **[AppRecordingStatus](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingstatus)** 对象的 **[CanRecord](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingstatus.CanRecord)** 属性。 如果 **CanRecord** 返回 **false**，这意味着您的应用程序不能当前记录，你可以检查 **[详细信息](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingstatus.Details)** 属性来确定原因。 根据具体原因，你可能要向用户显示状态或显示启用应用录制的说明。



[!code-cpp[CanRecord](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetCanRecord)]

## <a name="manually-start-and-stop-recording-your-app-to-a-file"></a>手动开始和停止将你的应用录制到文件

验证你的应用能够录制后，你可以通过调用 **AppRecordingManager** 对象的 **[StartRecordingToFileAsync](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingmanager.startrecordingtofileasync)** 方法开始新的录制。

在以下示例中，当异步任务失败时，执行第一个**然后**块。 第二个**然后**块尝试访问任务结果，如果结果为空，则表示任务已完成。 在这两种情况下，调用下方显示的 **OnRecordingComplete** 帮助程序方法处理结果。 

[!code-cpp[StartRecordToFile](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetStartRecordToFile)]

录制操作完成后，检查返回的 **[AppRecordingResult](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingresult)** 对象的 **[已成功](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingresult.Succeeded)** 属性以确定录制操作是否成功。 如果成功，你可以检查 **[IsFileTruncated](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingresult.IsFileTruncated)** 属性以确定系统是否因为存储原因而被强制截断捕获的文件。 你可以检查 **[持续时间](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingresult.Duration)** 属性，如果文件被截断，你会发现录制文件的实际持续时间可能短于录制操作的持续时间。

[!code-cpp[OnRecordingComplete](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetOnRecordingComplete)]

下面的示例显示了一些基本代码，这些代码用于开始和停止在上一示例中显示的录制操作。

[!code-cpp[CallStartRecordToFile](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetCallStartRecordToFile)]

[!code-cpp[FinishRecordToFile](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetFinishRecordToFile)]

## <a name="record-a-historical-time-span-to-a-file"></a>将历史时间跨度录制到文件
如果用户已经在系统设置中为你的应用启用历史录制，你可以录制之前已经发生的游戏的时间跨度。 本文的上一个示例演示了如何确认你的应用当前可以录制游戏。 你还可以通过其他检查确定是否已启用历史捕获。 再一次调用 **[GetStatus](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingmanager.GetStatus)** 并检查返回的 **AppRecordingStatus** 对象的 **[CanRecordTimeSpan](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingstatus.CanRecordTimeSpan)** 属性。 此示例也返回 **AppRecordingStatus** 的 **[HistoricalBufferDuration](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingstatus.HistoricalBufferDuration)** 属性，它将用于确定录制操作的有效开始时间。

[!code-cpp[CanRecordTimeSpan](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetCanRecordTimeSpan)]

若要捕获历史时间跨度，你必须指定录制的开始时间和持续时间。 开始时间作为 **[DateTime](https://docs.microsoft.com/uwp/api/windows.foundation.datetime)** 结构提供。 开始时间必须早于当前时间，且在历史录制缓冲区的长度内。 在此示例中，检查是否已启用历史录制时要检索缓冲区长度，如上一个代码示例中所示。 历史录制的持续时间作为 **[TimeSpan](https://docs.microsoft.com/uwp/api/windows.foundation.timespan)** 结构提供，它也应等于或小于历史缓冲区的持续时间。 一旦你确定了所需的开始时间和持续时间，则调用 **[RecordTimeSpanToFileAsync](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingmanager.recordtimespantofileasync)** 开始录制操作。

同手动开始和停止录制一样，当历史录制完成后，你可以检查返回的 **[AppRecordingResult](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingresult)** 对象的 **[已成功](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingresult.Succeeded)** 属性以确定录制操作是否成功，还可以检查 **[IsFileTruncated](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingresult.IsFileTruncated)** 和 **[持续时间](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingresult.Duration)** 属性，如果文件被截断，你会发现录制文件的实际持续时间可能短于请求的时间窗口的持续时间。

[!code-cpp[RecordTimeSpanToFile](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetRecordTimeSpanToFile)]

下面的示例显示了一些基本代码，这些代码用于启动在上一示例中显示的历史录制操作。

[!code-cpp[CallRecordTimeSpanToFile](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetCallRecordTimeSpanToFile)]

## <a name="save-screenshot-images-to-files"></a>将屏幕截图图像保存到文件
你的应用可以启动屏幕截图捕获，将应用窗口的当前内容保存到一个图像文件或使用不同的图像编码保存到多个图像文件。 若要指定你希望使用的图像编码，可以创建一个字符串列表，其中每个字符串表示一个图像类型。 **[ImageEncodingSubtypes](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingsubtypes)** 的属性为每个受支持的图像类型提供正确的字符串，如 **MediaEncodingSubtypes.Png** 或 **MediaEncodingSubtypes.JpegXr**。

通过调用 **AppRecordingManager** 对象的 **[SaveScreenshotToFilesAsync](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingmanager.savescreenshottofilesasync)** 方法启动屏幕捕获。 此方法的第一个参数是 **StorageFolder**，即图像文件的保存位置。 第二个参数是文件名前缀，系统会将保存的每个图像类型的扩展名（如“.png”）附加到该前缀后面。

**SaveScreenshotToFilesAsync** 的第三个参数是当要捕获的当前窗口显示 HDR 内容时，系统能够执行正确的颜色空间转换所需的必要参数。 如果存在 HDR 内容，此参数应设置为 **AppRecordingSaveScreenshotOption.HdrContentVisible**。 否则，请使用 **AppRecordingSaveScreenshotOption.None**。 此方法的最后一个参数是应将屏幕捕获到的图像格式的列表。

当异步调用 **SaveScreenshotToFilesAsync** 完成时，它将返回 **[AppRecordingSavedScreenshotInfo](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingsavedscreenshotinfo)** 对象，该对象提供 **StorageFile** 和关联的 **MediaEncodingSubtypes** 值，指示每个保存的图像的图像类型。

[!code-cpp[SaveScreenShotToFiles](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetSaveScreenShotToFiles)]

下面的示例显示了一些基本代码，这些代码用于启动在上一示例中显示的屏幕截图操作。

[!code-cpp[CallSaveScreenShotToFiles](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetCallSaveScreenShotToFiles)]

## <a name="add-game-metadata-for-system-and-app-initiated-capture"></a>为系统和应用启动的捕获添加游戏元数据
本文的以下部分介绍如何提供系统将嵌入到已捕获游戏或已广播游戏的 MP4 流中的元数据。 元数据可以嵌入到使用内置系统 UI 捕获的媒体中或由应用使用 **AppRecordingManager** 捕获的媒体中。 你的应用和其他应用在媒体播放过程中可以提取此元数据，以提供与捕获或广播的游戏同步的依赖于上下文的体验。

### <a name="get-an-instance-of-appcapturemetadatawriter"></a>获取 AppCaptureMetadataWriter 的一个实例
用于管理应用捕获元数据的主要类是 **[AppCaptureMetadataWriter](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatawriter)** 。 在初始化此类的实例时，使用 **[ApiInformation.IsApiContractPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent)** 方法查询 *Windows.Media.Capture.AppCaptureMetadataContract* 1.0，以验证 API 在当前设备上可用。

[!code-cpp[GetMetadataWriter](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetGetMetadataWriter)]

### <a name="write-metadata-to-the-system-cache-for-your-app"></a>将元数据写入到你的应用的系统缓存
每个元数据项都有一个字符串标签，用于标识元数据项、关联的数据值（可能是字符串、整数或双值），以及一个来自 **[AppCaptureMetadataPriority](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatapriority)** 枚举的值，该值指示数据项的相对优先级。 元数据项可以被视为发生在单个时间点的“事件”或在一个时间窗口内维持一个值的“状态”。 元数据被写入到内存缓存中，由系统为你的应用进行分配和管理。 系统对元数据的内存缓存强制执行大小限制，并且当达到此限制时，系统将基于每个元数据项被写入时的优先级清除数据。 本文的下一部分演示如何管理你的应用的元数据内存分配。

一个典型的应用可以选择在捕获会话开始时写入一些元数据，以便为后续数据提供一些上下文。 在此方案中，建议你使用即时“事件”数据。 此示例调用 **[AddStringEvent](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatawriter.addstringevent)** 、 **[AddDoubleEvent](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatawriter.adddoubleevent)** 和 **[AddInt32Event](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatawriter.addint32event)** 设置每个数据类型的即时值。

[!code-cpp[StartSession](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetStartSession)]

使用在一段时间内持续存在的“状态”数据的一个常见方案是跟踪玩家目前所在的游戏地图。 此示例调用 **[StartStringState](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatawriter.startstringstate)** 设置状态值。 

[!code-cpp[StartMap](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetStartMap)]

调用 **[StopState](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatawriter.stopstate)** 记录特定状态已结束。

[!code-cpp[EndMap](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetEndMap)]

你可以通过使用现有状态标签设置新值的方式覆盖状态。

[!code-cpp[LevelUp](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetLevelUp)]

你可以通过调用 **[StopAllStates](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatawriter.StopAllStates)** 的方式结束所有当前打开的状态。

[!code-cpp[RaceComplete](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetRaceComplete)]

### <a name="manage-metadata-cache-storage-limit"></a>管理元数据缓存存储限制
你使用 **AppCaptureMetadataWriter** 写入的元数据由系统进行缓存，直到该元数据被写入到关联的媒体流。 系统定义每个应用的元数据缓存的大小限制。 一旦达到缓存大小限制，系统将开始清除缓存的元数据。 系统将会删除元数据与写入 **[AppCaptureMetadataPriority.Informational](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatapriority)**  删除元数据与之前的优先级值 **[AppCaptureMetadataPriority.Important](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatapriority)**  优先级。

在任何时候，你都可以通过调用 **[RemainingStorageBytesAvailable](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatawriter.RemainingStorageBytesAvailable)** 的方式检查在你的应用的元数据缓存中可用的字节数。 你可以选择设置你自己的应用定义的阈值，之后可以选择减少你写入到缓存的元数据量。 下面的示例演示了此模式的简单实现。

[!code-cpp[CheckMetadataStorage](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetCheckMetadataStorage)]

[!code-cpp[ComboExecuted](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetComboExecuted)]

### <a name="receive-notifications-when-the-system-purges-metadata"></a>当系统清除元数据时收到通知
您可以注册时系统开始您的应用程序的元数据清除由注册的处理程序接收通知 **[MetadataPurged](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatawriter.MetadataPurged)** 事件。

[!code-cpp[RegisterMetadataPurged](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetRegisterMetadataPurged)]

在 **MetadataPurged** 事件的处理程序中，你可以通过结束低优先级状态的方式清除元数据缓存中的一些空间，可以实施由应用定义的减少写入到缓存的元数据量的逻辑，或者不执行任何操作，让系统继续基于写入的优先级清除缓存。

[!code-cpp[OnMetadataPurged](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetOnMetadataPurged)]

## <a name="related-topics"></a>相关主题

* [游戏](index.md)
 

 




