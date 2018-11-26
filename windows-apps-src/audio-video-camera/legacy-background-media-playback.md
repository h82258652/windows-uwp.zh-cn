---
ms.assetid: 3848cd72-eccd-400e-93ff-13649cd81b6c
description: 本文提供对使用传统后台媒体模型进行播放的应用的支持，并提供有关迁移到新模型的指南。
title: 传统后台媒体播放
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ea8d387becaef171175fd5e91bfc3a1402e79faa
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2018
ms.locfileid: "7692968"
---
# <a name="legacy-background-media-playback"></a>传统后台媒体播放


本文介绍了传统双进程模型，用于向 UWP 应用添加后台音频支持。 从 Windows 10 版本 1607 开始，后台音频的单进程模型更易于实现。 有关后台音频的当前建议的详细信息，请参阅[在后台播放媒体](background-audio.md)。 本文旨在为已使用传统双进程模型进行开发的应用提供支持。

> [!NOTE]
> 从 Windows 版本 1703 中，开始**BackgroundMediaPlayer**已弃用，并且可能无法在将来版本的 Windows。

## <a name="background-audio-architecture"></a>后台音频体系结构

执行后台播放的应用包含两个进程。 第一个进程是在前台运行的主要应用，包含了应用 UI 和客户端逻辑。 第二个进程是后台播放任务，可实现 [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794)，如所有 UWP 应用后台任务。 后台任务包含音频播放逻辑和后台服务。 后台任务通过系统媒体传输控件与系统通信。

下图是系统设计方式的概述。

![Windows 10 后台音频体系结构](images/backround-audio-architecture-win10.png)
## <a name="mediaplayer"></a>MediaPlayer

[**Windows.Media.Playback**](https://msdn.microsoft.com/library/windows/apps/dn640562) 命名空间包含用于在后台播放音频的 API。 每个应用都存在一个用于进行播放的 [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/dn652535) 实例。 你的后台音频应用将调用 **MediaPlayer** 类上的方法并设置相关属性，以设置当前曲目、开始播放、暂停、快进、快退等等。 始终可通过 [**BackgroundMediaPlayer.Current**](https://msdn.microsoft.com/library/windows/apps/dn652528) 属性访问媒体播放器对象实例。

## <a name="mediaplayer-proxy-and-stub"></a>MediaPlayer 代理和存根

当从应用的后台进程访问 **BackgroundMediaPlayer.Current** 时，将激活后台任务主机中的 **MediaPlayer** 实例并且可对其进行直接操作。

当从前台应用程序访问 **BackgroundMediaPlayer.Current** 时，返回的 **MediaPlayer** 实例实际是一个可与后台进程中的存根通信的代理。 此存根将与实际的 **MediaPlayer** 实例通信，后者也托管在后台进程中。

前台和后台进程都可以访问 **MediaPlayer** 实例的大多数属性，但 [**MediaPlayer.Source**](https://msdn.microsoft.com/library/windows/apps/dn987010) 和 [**MediaPlayer.SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn926635) 是例外，仅可从后台进程访问它们。 前台应用和后台进程都可以接收特定于媒体的事件（例如 [**MediaOpened**](https://msdn.microsoft.com/library/windows/apps/dn652609)、[**MediaEnded**](https://msdn.microsoft.com/library/windows/apps/dn652603) 和 [**MediaFailed**](https://msdn.microsoft.com/library/windows/apps/dn652606)）的通知。

## <a name="playback-lists"></a>播放列表

后台音频应用程序的常见方案是连续播放多个项目。 这最容易通过使用 [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/dn930955) 对象在你的后台进程中完成，可通过将该对象分配给 [**MediaPlayer.Source**](https://msdn.microsoft.com/library/windows/apps/dn987010) 属性来将其设置为 **MediaPlayer** 上的源。

不可以从前台进程访问在后台进程中设置的 **MediaPlaybackList**。

## <a name="system-media-transport-controls"></a>系统媒体传输控件

用户可以通过蓝牙设备、SmartGlass 和系统媒体传输控件等方式，在不直接使用你的应用 UI 的情况下控制音频播放。 你的后台任务使用 [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677) 类订阅这些用户引发的系统事件。

若要从后台进程中获取 **SystemMediaTransportControls** 实例，请使用 [**MediaPlayer.SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn926635) 属性。 前台应用通过调用 [**SystemMediaTransportControls.GetForCurrentView**](https://msdn.microsoft.com/library/windows/apps/dn278708) 获取类的实例，但返回的实例是与后台任务无关的仅限于前台的实例。

## <a name="sending-messages-between-tasks"></a>在任务之间发送消息

有时，你将希望在后台音频应用的两个进程之间进行通信。 例如，你可能希望后台任务在开始播放新歌曲时通知前台任务，然后将新歌曲标题发送到要在屏幕上显示的前台任务。

简单的通信机制可同时在前台和后台进程中引发事件。 [**SendMessageToForeground**](https://msdn.microsoft.com/library/windows/apps/dn652533) 和 [**SendMessageToBackground**](https://msdn.microsoft.com/library/windows/apps/dn652532) 方法分别调用相应进程中的事件。 消息可通过订阅 [**MessageReceivedFromBackground**](https://msdn.microsoft.com/library/windows/apps/dn652530) 和 [**MessageReceivedFromForeground**](https://msdn.microsoft.com/library/windows/apps/dn652531) 事件来接收。

可将数据作为参数传递给发送消息方法，这些方法随后会传入消息接收的事件处理程序中。 使用 [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) 类传递数据。 此类是一个字典，包含了作为键的字符串和作为值的其他值类型。 你可以传递整数、字符串和布尔值等简单的值类型。

## <a name="background-task-life-cycle"></a>后台任务生命周期

后台任务的生命周期与应用的当前播放状态紧密相连。 例如，当用户暂停音频播放时，系统可能会根据情况终止或取消你的应用。 在音频播放停止一段时间后，系统可能会自动关闭后台任务。

在你的应用第一次通过在前台应用中运行的代码访问 [**MessageReceivedFromBackground**](https://msdn.microsoft.com/library/windows/apps/dn652530) 时，或当你为 [**BackgroundMediaPlayer.Current**](https://msdn.microsoft.com/library/windows/apps/dn652528) 事件注册处理程序时（以先发生的为准），将调用 [**IBackgroundTask.Run**](https://msdn.microsoft.com/library/windows/apps/br224811) 方法。 建议你第一次先注册消息接收的处理程序，再调用 **BackgroundMediaPlayer.Current**，以便前台应用不会错过从后台进程发来的任何消息。

若要使后台任务保持活动状态，你的应用必须请求 **Run** 方法中的 [**BackgroundTaskDeferral**](https://msdn.microsoft.com/library/windows/apps/hh700499) 并在任务实例接收 [**Canceled**](https://msdn.microsoft.com/library/windows/apps/br224798) 或 [**Completed**](https://msdn.microsoft.com/library/windows/apps/br224788) 事件时调用 [**BackgroundTaskDeferral.Complete**](https://msdn.microsoft.com/library/windows/apps/hh700504)。 不要使用 **Run** 方法进行循环或等待，因为这将使用资源，并且可能会导致应用的后台任务被系统终止。

当完成 **Run** 方法且未请求延迟时，你的后台任务将获取 **Completed** 事件。 在某些情况下，当你的应用获取 **Canceled** 事件时，该事件后面还跟有 **Completed** 事件。 在执行 **Run** 时，你的任务可能会收到 **Canceled** 事件，因此请务必管理此潜在的并发情况。

可取消后台任务的情况包括：

-   带有音频播放功能的新应用在强制执行独有性子策略的系统上启动。 请参阅下面的[适用于后台音频任务生命周期的系统策略](#system-policies-for-background-audio-task-lifetime)部分。

-   后台任务已启动但还未播放音乐，接着前台应用暂停。

-   其他媒体中断，例如来电或 VoIP 呼叫。

可终止后台任务而不另行通知的情况包括：

-   VoIP 呼叫接入，而系统上没有使后台任务保持活动状态的足够可用内存。

-   违反资源策略。

-   任务取消或完成操作不会顺利结束。

## <a name="system-policies-for-background-audio-task-lifetime"></a>适用于后台音频任务生命周期的系统策略

以下策略将帮助确定系统管理后台音频任务生命周期的方式。

### <a name="exclusivity"></a>独有性

如果启用，则此子策略会将在任何给定时间的后台音频任务数上限都限制为 1。 它在移动 SKU 和其他非桌面 SKU 上处于启用状态。

### <a name="inactivity-timeout"></a>不活动超时

由于资源限制，系统可能会在后台任务处于非活动状态一段时间后终止它。

如果同时满足以下这两个条件，则将后台任务视为“非活动”：

-   前台应用不可见（暂停或终止）。

-   后台媒体播放器未处于播放状态。

如果同时满足这两个条件，则后台媒体系统策略将启动一个计时器。 如果在计时器到期时这两个条件都未发生更改，则后台媒体系统策略将终止后台任务。

### <a name="shared-lifetime"></a>共享生命周期

如果启用，则此子策略将强制后台任务依赖前台任务的生命周期。 如果前台任务关闭（无论是由用户还是由系统关闭），后台任务也会关闭。

但是，请注意这并不意味着前台依赖后台。 如果后台任务关闭，这并不会强制关闭前台任务。

下表列出了对哪些设备类型强制执行哪些策略。

| 子策略             | 桌面  | 移动   | 其他    |
|------------------------|----------|----------|----------|
| **独有性**        | 已禁用 | 已启用  | 已启用  |
| **不活动超时** | 已禁用 | 已启用  | 已禁用 |
| **共享生命周期**    | 已启用  | 已禁用 | 已禁用 |


 

 




