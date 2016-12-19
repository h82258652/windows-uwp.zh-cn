---
author: TylerMSFT
title: "启动远程设备上的应用"
description: Learn how to launch an app on a remote device using Project "Rome".
translationtype: Human Translation
ms.sourcegitcommit: 4e94ee5b3c56f3ec20e3592b97348c291297a670
ms.openlocfilehash: d429ea4a6f8d52445c99cb170bb41c3fc1515dde

---

# <a name="launch-an-app-on-a-remote-device"></a>启动远程设备上的应用

本文介绍了如何启动远程设备上的 Windows 应用。

从 Windows 10 版本 1607 开始，UWP 应用可以远程启动另一台同样也运行 Windows 10 版本 1607 或更高版本的设备上的 UWP 应用或 Windows 桌面应用程序，只要这两台设备都使用相同的 Microsoft 帐户 (MSA) 登录。

远程启动功能支持面向任务的用户体验，使用户可以在一台设备上启动任务并在另一台设备上完成。 例如，如果用户在汽车里用手机听音乐，然后可以在到家后在其 Xbox One 上接着播放。 远程启动允许你将上下文数据传递给启动的远程应用，以便从任务结束的位置继续执行。

## <a name="add-the-remotesystem-capability"></a>添加 RemoteSystem 功能

为了让你的应用启动远程设备上的应用，必须将 `remoteSystem` 功能添加到应用包清单。 可以通过选择“功能”选项卡上的“远程系统”来使用程序包清单设计器添加它，也可以手动将以下行添加到项目的 Package.appxmanifest 文件。

``` xml
<Capabilities>
   <uap3:Capability Name="remoteSystem"/>
 </Capabilities>
```
## <a name="find-a-remote-device"></a>查找远程设备

必须先查找要连接的设备。 [发现远程设备](discover-remote-devices.md)详细讨论了如何执行此操作。 此处我们将使用简单的方法，将不按设备或连接类型进行筛选。 我们将创建用于查找远程设备的远程系统观察程序，并编写处理程序，用于在发现或删除设备时引发的事件。 这将向我们提供远程设备集合。

这些示例中的代码假设你的文件中有 `using Windows.System.RemoteSystems` 语句。

[!code-cs[Main](./code/RemoteLaunchScenario/MainPage.xaml.cs#SnippetBuildDeviceList)]

在进行远程启动前，必须先调用 `RemoteSystem.RequestAccessAsync()`。 检查返回值以确保你的应用可以访问远程设备。 此检查可能失败的原因之一是未将 `remoteSystem` 功能添加到应用。

当发现可以连接的设备或此类设备不再可用时，调用系统观察程序事件处理程序。 我们将使用这些事件处理程序来保持可连接的设备更新列表。

[!code-cs[Main](./code/RemoteLaunchScenario/MainPage.xaml.cs#SnippetEventHandlers)]

我们将使用**字典**按远程系统 ID 跟踪设备。 **ObservableCollection** 用于保存我们可以枚举的设备列表。 借助 **ObservableCollection**，还可以轻松将设备列表绑定到 UI，不过在该示例中我们不会执行此操作。

[!code-cs[Main](./code/RemoteLaunchScenario/MainPage.xaml.cs#SnippetMembers)]

在尝试启动远程应用前，在你的应用启动代码中添加对 `BuildDeviceList()` 的调用。

## <a name="launch-an-app-on-a-remote-device"></a>启动远程设备上的应用

通过将希望连接的设备传递给 [**RemoteLauncher.LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/windows.system.remotelauncher.launchuriasync.aspx) API，远程启动应用。 此方法有三个重载。 最简单的重载（即该示例中所演示的重载）用于指定将激活远程设备上的应用的 URI。 在该示例中，URI 将打开远程计算机上具有三维太空针塔视图的“地图”应用。

其他 **RemoteLauncher.LaunchUriAsync** 重载允许你指定网站 URI 之类的选项，以便查看是否没有合适的应用在远程设备上启动，以及可用于启动远程设备上 URI 的程序包系列名称的可选列表。 也可以采用键/值对的形式提供数据。 可以将数据传递给正在激活的应用，以便为远程应用提供上下文，例如将播放的歌曲名称，以及将播放从一台设备转移到另一台设备时的当前播放位置。

在实际情况下，可能会提供 UI 以选择你要面向的设备。 但为了简化该示例，我们将只使用列表中的第一个远程设备。

[!code-cs[Main](./code/RemoteLaunchScenario/MainPage.xaml.cs#SnippetRemoteUriLaunch)]

从 **RemoteLauncher.LaunchUriAsync()** 返回的 [**RemoteLaunchUriStatus**](https://msdn.microsoft.com/library/windows/apps/windows.system.remotelaunchuristatus.aspx) 对象提供有关远程启动是否成功以及失败原因（如果失败）的信息。

## <a name="related-topics"></a>相关主题

[远程系统 API 参考](https://msdn.microsoft.com/en-us/library/windows/apps/Windows.System.RemoteSystems)  
[连接的应用和设备（项目“Rome”）概述](connected-apps-and-devices.md)  
[发现远程设备](discover-remote-devices.md)  
[远程系统示例](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems)演示了如何发现远程系统、在远程系统上启动应用，以及使用应用服务在运行在两个系统上的应用之间发送消息。



<!--HONumber=Dec16_HO1-->


