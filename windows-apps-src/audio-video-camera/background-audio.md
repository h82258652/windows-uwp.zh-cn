---
author: drewbatgit
ms.assetid: 
description: "本文将向你介绍当应用在后台运行时如何播放媒体。"
title: "在后台播放媒体"
translationtype: Human Translation
ms.sourcegitcommit: c8cbc538e0979f48b657197d59cb94a90bc61210
ms.openlocfilehash: a477827553ac1780ac625deeee08d84ab638d4c2

---

# 在后台播放媒体
本文介绍了如何配置应用，以便在应用从前台移至后台后，媒体可以继续播放。 这意味着，即使在用户已最小化你的应用、返回到主屏幕，或已以其他方式离开你的应用后，你的应用仍可继续播放音频。 

后台音频播放的方案包括：

-   **长期运行的播放列表：**用户可以简单地弹出前台应用以选择并启动一个播放列表，在此之后，用户期望该播放列表在后台继续播放。

-   **使用任务切换器：**用户可以简单地打开前台应用以开始播放音频，然后使用任务切换程序切换到另一个打开的应用。 用户期望该音频在后台继续播放。

本文所述的后台音频实现将使你的应用通常在所有 Windows 设备（包括移动设备、桌面设备和 Xbox）上运行。

> [!NOTE]
> 本文中的代码源自 UWP [后台音频示例](http://go.microsoft.com/fwlink/p/?LinkId=800141)。

## 单进程模型说明。
Windows 10 版本 1607 引入的全新单进程模型极大地简化了启用后台音频的进程。 以前，需要你的应用管理后台进程以及前台应用，然后在两个进程之间手动通知状态更改。 在新的模型下，只需将后台音频功能添加到应用部件清单，该应用就会在移至后台之后自动继续播放音频。 [**EnteredBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.EnteredBackground) 和 [**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground) 这两个新的应用程序生命周期事件使应用可以得知它何时进入和退出后台。 当你的应用移至、过渡到后台或从中移出时，系统强制执行的内存约束可能会发生改变，因此你可以使用这些事件检查当前内存消耗并释放资源，以便保持在限制之下。

通过消除复杂的进程间通信和状态管理，新模型使你可以使用明显减少的代码极快地实现后台音频。 但是，当前版本中仍支持双进程模型，以实现向后兼容性。 有关详细信息，请参阅[传统后台音频模型](background-audio.md)。

## 后台音频要求
当应用在后台运行时，该应用必须满足音频播放的以下要求。

* 将**后台媒体播放**功能添加到应用部件清单，如本文中的后面部分所述。
* 如果应用禁止自动将 **MediaPlayer** 与系统媒体传输控件 (SMTC) 集成（例如，将 [**CommandManager.IsEnabled**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager.IsEnabled) 属性设置为 false），那么必须手动实现与 SMTC 的集成，才可以支持后台媒体播放。 如果使用的是 **MediaPlayer** 之外的 API（如 [**AudioGraph**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Audio.AudioGraph)），还必须手动与 SMTC 集成，才可以播放音频（如果你希望应用移至后台之后音频继续播放）。 [手动控制系统媒体传输控件](system-media-transport-controls.md)的“将系统媒体传输控件用于后台音频”部分介绍了 SMTC 集成的最低要求。
* 应用处于后台时，不得超出系统为后台应用设置的内存使用量限制。 管理后台内存的指南将在文本后面部分提供。

## 后台媒体播放清单功能
若要支持后台音频，必须将后台媒体播放功能添加到应用部件清单文件（即 Package.appxmanifest）。 

**使用清单设计器将功能添加到应用部件清单**

1.  在 Microsoft Visual Studio 的“解决方案资源管理器”****中，通过双击“package.appxmanifest”****项，打开应用程序清单的设计器。
2.  选择“功能”****选项卡。
3.  选择“后台媒体播放”****复选框。

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

##处理前台和后台之间的转换
当应用从前台移至后台时，将引发 [**EnteredBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.EnteredBackground) 事件。 并且当应用返回前台时，将引发 [**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground) 事件。 由于这些事件都是应用生命周期事件，因此应该在创建应用时为这些事件注册处理程序。 在默认项目模板中，这意味着将它添加到 App.xaml.cs 中的 **App** 类构造函数。 由于在后台运行会减少系统允许应用保留的内存资源，因此还应该注册 [**AppMemoryUsageIncreased**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageIncreased) 和 [**AppMemoryUsageLimitChanging**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageLimitChanging) 事件，这两个事件将用于检查应用的当前内存使用量和当前限制。 这些事件的处理程序如以下示例中所示。 有关 UWP 应用的应用程序生命周期的详细信息，请参阅[应用生命周期](../\launch-resume\app-lifecycle.md)。

[!code-cs[RegisterEvents](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetRegisterEvents)]

添加一个变量以跟踪你当前是否在后台运行。

[!code-cs[DeclareBackgroundMode](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetDeclareBackgroundMode)]

当引发 [**EnteredBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.EnteredBackground) 事件时，请设置跟踪变量以表明你当前在后台运行。 不得在 **EnteredBackground** 事件中执行长时间运行的任务，因为这可能会导致用户感觉过渡到后台非常慢。

[!code-cs[EnteredBackground](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetEnteredBackground)]

当应用过渡到后台时，系统会降低针对该应用的内存限制，以确保当前的前台应用具有的资源足以提供响应迅速的用户体验。 [**AppMemoryUsageLimitChanging**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageLimitChanging) 事件处理程序使你的应用可以得知其分配的内存已减少，同时在传递给该处理程序的事件参数中提供新限制。 将 [**MemoryManager.AppMemoryUsage**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsage) 属性（提供应用的当前使用量）与事件参数的 [**NewLimit**](https://msdn.microsoft.com/library/windows/apps/Windows.System.AppMemoryUsageLimitChangingEventArgs.NewLimit) 属性（指定新限制）比较。 如果内存使用量超过该限制，则需要减少内存使用量。 在此示例中，使用帮助程序方法 **ReduceMemoryUsage**（将在本文后面部分定义）执行此操作。

[!code-cs[MemoryUsageLimitChanging](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetMemoryUsageLimitChanging)]

> [!NOTE] 
> 某些设备配置会允许应用程序在超出新内存限制的情况下继续运行，直到系统遇到资源压力，而有些设备则不允许。 尤其是在 Xbox 上，如果应用在 2 秒内未将内存使用量降低至新限制下，就会被暂停或终止。 这意味着，通过使用此事件在引发该事件的 2 秒内将资源使用量降低至限制下，就可以在大部分设备上实现最佳体验。


当应用首次过渡到后台时，其内存使用量可能低于后台应用的内存限制，但在之后的某个时间点，其使用量就会增加，开始接近限制。 处理程序 [**AppMemoryUsageIncreased**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageIncreased) 使你有机会检查使用量增加时的当前使用量，并释放内存（如果需要）。 查看 [**AppMemoryUsageLevel**](https://msdn.microsoft.com/library/windows/apps/Windows.System.AppMemoryUsageLevel) 是否为 **High** 或 **OverLimit**，如果是，请降低内存使用量。 此外，在此示例中，本进程由帮助程序方法 **ReduceMemoryUsage** 进行处理。 还可以订阅 [**AppMemoryUsageDecreased**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageDecreased) 事件，查看应用是否低于限制，如果是，你便知晓可以分配其他资源。

[!code-cs[MemoryUsageIncreased](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetMemoryUsageIncreased)]

**ReduceMemoryUsage** 是一个帮助程序方法，当应用超过在后台运行的应用的使用量限制时，它可以实现内存释放。 释放内存的方法因应用的特性而异，但用于释放内存的一个建议方法是释放 UI 以及与应用视图关联的其他资源。 首先，确保在后台模式下运行，然后将应用窗口的 [**Content**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Window.Content) 属性设置为 null。 调用 **GC.Collect** 指示系统立即回收释放的内存。

[!code-cs[UnloadViewContent](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetUnloadViewContent)]

当窗口内容收集完成时，每个框架都开始其断开连接处理。 如果窗口内容下的可视化对象树中存在页面，这些页面将开始引发其 [**Unloaded**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.FrameworkElement.Unloaded) 事件。 除非已删除对页面的所有引用，否则不会从内存中彻底清除它们。 在 **Unloaded** 回调中，执行以下操作以确保快速释放该内存：
* 清除页面中任何较大的数据结构，并将它们设置为 null。
* 注销页面内具有回调方法的所有事件处理程序。 确保在页面的 Loaded 事件处理程序期间注册这些回调。 当 UI 已完成重建，并且页面已添加到可视化对象树时，将引发 Loaded 事件。
* 在 Unloaded 回调的末尾调用 **GC.Collect**，以快速垃圾回收刚刚设置为 null 的任何较大数据结构。

[!code-cs[Unloaded](./code/BackgroundAudio_RS1/cs/MainPage.xaml.cs#SnippetUnloaded)]

在 [**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground) 事件处理程序中，应该设置跟踪变量，以指示应用不再在后台运行。 接下来，查看当前窗口的 [**Content**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Window.Content) 是否为 null，如果你释放应用视图以清除内存（处于后台运行时），它就会为 null。 如果窗口内容为 null，请重新生成应用视图。 在此示例中，使用帮助程序方法 **CreateRootFrame** 创建窗口内容。

[!code-cs[LeavingBackground](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetLeavingBackground)]

**CreateRootFrame** 帮助程序方法将重新创建应用的视图内容。 此方法中的代码等效于默认项目模板中提供的 [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) 处理程序代码。 一个区别是：**Launching** 处理程序确定 [**LaunchActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Activation.LaunchActivatedEventArgs) 的 [**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Activation.LaunchActivatedEventArgs.PreviousExecutionState) 属性中的之前执行状态，而 **CreateRootFrame** 方法只是获取作为参数传入的之前执行状态。 若要最大程度地减少重复代码，可以重构默认的 **Launching** 事件处理程序代码以调用 **CreateRootFrame**（如果你愿意）。

[!code-cs[CreateRootFrame](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetCreateRootFrame)]

## 后台媒体应用的网络可用性
不会从流或文件创建的所有网络感知的媒体源将使网络连接保持活动状态（在检索远程内容时），不需要检索时会释放网络连接。 尤其是 [**MediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaStreamSource)，依赖应用程序使用 [**SetBufferedRange**](https://msdn.microsoft.com/library/windows/apps/dn282762) 向平台正确报告已正确缓存的范围。 完全缓存整个内容后，网络不再以应用的名义保留。

如果需要在媒体不在下载时在后台执行网络调用，则必须在诸如 [**ApplicationTrigger**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Background.ApplicationTrigger)、[**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Background.MaintenanceTrigger) 或 [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Background.TimeTrigger) 等相应任务中包括这些网络调用。 有关详细信息，请参阅[使用后台任务支持应用](https://msdn.microsoft.com/en-us/windows/uwp/launch-resume/support-your-app-with-background-tasks)。

## 相关主题
* [媒体播放](media-playback.md)
* [使用 MediaPlayer 播放音频和视频](play-audio-and-video-with-mediaplayer.md)
* [与系统媒体传输控件集成](integrate-with-systemmediatransportcontrols.md)
* [后台音频示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundMediaPlayback)

 

 







<!--HONumber=Aug16_HO3-->


