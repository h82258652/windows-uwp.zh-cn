---
description: 本文介绍如何使用 AudioPlaybackConnection 来启用连接蓝牙的远程设备，以便在本地计算机上播放音频。
title: 启用通过远程蓝牙连接的设备播放音频
ms.date: 05/03/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f03fc963e533ff29d49c326611c45437baa14f6c
ms.sourcegitcommit: 87fd0ec1e706a460832b67f936a3014f0877a88c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/12/2020
ms.locfileid: "83234950"
---
# <a name="enable-audio-playback-from-remote-bluetooth-connected-devices"></a>启用通过远程蓝牙连接的设备播放音频

本文介绍如何使用[AudioPlaybackConnection](/uwp/api/windows.media.audio.audioplaybackconnection)来启用连接蓝牙的远程设备，以便在本地计算机上播放音频。

从 Windows 10 开始，版本2004远程音频源可以将音频流式传输到 Windows 设备，从而实现诸如将 PC 配置为与蓝牙扬声器的行为类似的方案，并允许用户从手机接收音频。 实现使用 OS 中的蓝牙组件来处理传入的音频数据，并将其播放到系统上系统的音频终结点上，例如内置 PC 扬声器或有线耳机。 启用基础蓝牙 A2DP 接收器由应用管理，这些应用负责最终用户方案，而不是由系统使用。

[AudioPlaybackConnection](/uwp/api/windows.media.audio.audioplaybackconnection)类用于启用和禁用来自远程设备的连接，并用于创建连接，允许开始远程音频播放。

## <a name="add-a-user-interface"></a>添加用户界面

对于本文中的示例，我们将使用以下简单的 XAML UI，该 UI 定义**ListView**控件以显示可用的远程设备、显示连接状态的**TextBlock**和三个用于启用、禁用和打开连接的按钮。

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/MainPage.xaml" id="snippet_AudioPlaybackConnectionXAML":::

## <a name="use-devicewatcher-to-monitor-for-remote-devices"></a>使用 DeviceWatcher 监视远程设备

[DeviceWatcher](/uwp/api/windows.devices.enumeration.devicewatcher)类允许检测连接的设备。 [AudioPlaybackConnection. GetDeviceSelector](/uwp/api/windows.media.audio.audioplaybackconnection.getdeviceselector)方法返回一个字符串，该字符串告知设备观察器要监视何种类型的设备。 将此字符串传递到**DeviceWatcher**构造函数。 

启动设备观察程序时，将为每个已连接的设备引发[DeviceWatcher](/uwp/api/windows.devices.enumeration.devicewatcher.added)事件，并为设备观察程序运行期间连接的任何设备引发该事件。 如果以前连接的设备断开连接，则会引发[DeviceWatcher](/uwp/api/windows.devices.enumeration.devicewatcher.removed)事件。 

调用[DeviceWatcher](/uwp/api/windows.devices.enumeration.devicewatcher.start)开始监视支持音频播放连接的连接设备。 在此示例中，当加载 UI 中的主**网格**控件时，将启动设备管理器。 有关使用**DeviceWatcher**的详细信息，请参阅[枚举设备](/windows/uwp/devices-sensors/enumerate-devices)。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/MainPage.xaml.cs" id="snippet_MainGridLoaded":::


在设备观察程序的**添加**事件中，每个发现的设备均由一个[DeviceInformation](/uwp/api/Windows.Devices.Enumeration.DeviceInformation)对象表示。 将发现的每个设备添加到在 UI 中绑定到**ListView**控件的可观察集合。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/MainPage.xaml.cs" id="snippet_DeclareDevices":::


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/MainPage.xaml.cs" id="snippet_DeviceWatcher_Added":::


## <a name="enable-and-release-audio-playback-connections"></a>启用和发布音频播放连接

在打开与设备的连接之前，必须启用连接。 这会通知系统存在一个新的应用程序，该应用程序需要在计算机上播放远程设备的音频，但在打开连接之前音频不会开始播放，这会在后面的步骤中显示。

在 "**启用音频播放连接**" 按钮的单击处理程序中，获取与**ListView**控件中当前所选设备关联的设备 ID。 此示例维护已启用的**AudioPlaybackConnection**对象的字典。 此方法首先检查字典中是否已存在所选设备的条目。 接下来，该方法通过调用[TryCreateFromId](/uwp/api/windows.media.audio.audioplaybackconnection.trycreatefromid)并传入选定的设备 ID，尝试为选定的设备创建**AudioPlaybackConnection** 。 

如果成功创建连接，请将新的**AudioPlaybackConnection**对象添加到应用的字典，为对象的[StateChanged](/uwp/api/windows.media.audio.audioplaybackconnection.statechanged)事件注册处理程序，并调用[channel.startasync](/uwp/api/windows.media.audio.audioplaybackconnection.startasync)通知系统新连接已启用。 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/MainPage.xaml.cs" id="snippet_DeclareConnections":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/MainPage.xaml.cs" id="snippet_EnableAudioPlaybackConnection":::


## <a name="open-the-audio-playback-connection"></a>打开音频播放连接

在上一步中，已创建音频播放连接，但在通过调用[Open](/uwp/api/windows.media.audio.audioplaybackconnection.open)或[OpenAsync](/uwp/api/windows.media.audio.audioplaybackconnection.openasync)打开连接之前，不会开始播放声音。 在 "**打开音频播放连接**" 按钮上，单击 "处理程序"，获取当前所选的设备，并使用该 ID 从应用的连接字典中检索**AudioPlaybackConnection** 。 等待对**OpenAsync**的调用，并检查返回的[AudioPlaybackConnectionOpenResultStatus](/uwp/api/windows.media.audio.audioplaybackconnectionopenresult)对象的**状态**值，以查看连接是否已成功打开，如果是，则更新 "连接状态" 文本框。


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/MainPage.xaml.cs" id="snippet_OpenAudioPlaybackConnectionButton":::

## <a name="monitor-audio-playback-connection-state"></a>监视音频播放连接状态

当连接的状态发生更改时，将引发[AudioPlaybackConnection ConnectionStateChanged](/uwp/api/windows.media.audio.audioplaybackconnection.statechanged)事件。 在此示例中，此事件的处理程序更新状态文本框。 请记住在对[RunAsync](/uwp/api/windows.ui.core.coredispatcher.runasync)的调用中更新 ui，以确保在 UI 线程上进行更新。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/MainPage.xaml.cs" id="snippet_ConnectionStateChanged":::

## <a name="release-connections-and-handle-removed-devices"></a>发布连接并处理删除的设备

此示例提供了 "**发布音频播放" 连接**按钮，以允许用户释放音频播放连接。 在此事件的处理程序中，我们获取当前选定的设备，并使用设备的 ID 在字典中查找**AudioPlaybackConnection** 。 调用**Dispose**以释放引用并释放任何关联的资源，并从字典中删除连接。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/MainPage.xaml.cs" id="snippet_ReleaseAudioPlaybackConnectionButton":::

应处理在启用或打开连接时删除设备的情况。 为此，请为设备观察程序的[DeviceWatcher](/uwp/api/windows.devices.enumeration.devicewatcher.removed)事件实现一个处理程序。 首先，删除的设备的 ID 用于从绑定到应用的**ListView**控件的可观察集合中删除设备。 接下来，如果与此设备关联的连接在应用的字典中，则调用**Dispose**以释放关联的资源，然后从字典中删除连接。 所有这些都是在对**RunAsync**的调用中完成的，以确保在 ui 线程上执行 ui 更新。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/MainPage.xaml.cs" id="snippet_DeviceWatcher_Removed":::

## <a name="related-topics"></a>相关主题

[媒体播放](media-playback.md)


 




