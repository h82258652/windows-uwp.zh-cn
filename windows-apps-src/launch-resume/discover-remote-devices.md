---
author: PatrickFarley
title: "发现远程设备"
description: Learn how to discover remote devices from your app using Project "Rome".
translationtype: Human Translation
ms.sourcegitcommit: dc5030fea8e149b3926b926b09bfec3eeb5092ee
ms.openlocfilehash: b4b4eb28ea56ab3d84e2fe0bc0c281cb53149d5d

---

# 发现远程设备
你的应用可以使用无线网络、蓝牙和云连接来发现使用发现设备的相同 Microsoft 帐户登录的 Windows 设备。 也可以发现可接受匿名连接的公用设备（如 Surface Hub 和 Xbox One）。 无需安装任何特殊软件，即可发现远程设备。

> [!NOTE]
> 本指南假设你已经按照[启动远程应用](launch-a-remote-app.md)中的步骤授予远程系统功能的访问权限。

## 筛选可发现的设备集
你可以将 [**RemoteSystemWatcher**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemWatcher) 与筛选器结合使用，缩小可发现的设备集。 筛选器可以检测发现类型（本地网络与云连接）、设备类型（桌面设备、移动设备、Xbox、Hub 和全息设备）以及可用性状态（要使用远程系统功能的设备的可用性状态）。

在初始化 **RemoteSystemWatcher** 对象之前，必须构建筛选器对象，因为它们将作为一项参数传入它的构造函数中。 以下代码可为每一种可用类型都创建一个筛选器，然后将它们添加到列表。

> [!NOTE]
> 这些示例中的代码假设你的文件中有 `using Windows.System.RemoteSystems` 语句。

[!code-cs[Main](./code/DiscoverDevices/MainPage.xaml.cs#SnippetMakeFilterList)]

创建 [**IRemoteSystemFilter**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.IRemoteSystemFilter) 对象列表后，可以将它传递到 **RemoteSystemWatcher** 的构造函数中。

[!code-cs[Main](./code/DiscoverDevices/MainPage.xaml.cs#SnippetCreateWatcher)]

当调用该观察程序的 [**Start**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemWatcher.Start) 方法时，只有检测到满足以下全部条件的设备，才会引发 [**RemoteSystemAdded**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemWatcher.RemoteSystemAdded) 事件。
* 可以通过邻近连接发现该设备
* 它是一台桌面设备或手机
* 它被归类为可用

从此处开始，处理事件、检索 [**RemoteSystem**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystem) 对象和连接到远程设备的过程与[启动远程应用](launch-a-remote-app.md)中的过程完全相同。 简而言之，**RemoteSystem** 对象作为 [**RemoteSystemAddedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemAddedEventArgs) 对象的属性存储，这些都是每个 **RemoteSystemAdded** 事件的参数。

## 通过地址输入发现设备
某些设备可能与用户没有关联，或无法通过扫描发现，但是如果发现应用使用直接地址，仍可以查找到它们。 [**HostName**](https://msdn.microsoft.com/library/windows/apps/windows.networking.hostname.aspx) 类用于表示远程设备的地址。 该地址通常以 IP 地址的形式存储，但也允许存储为其他几种格式（请参阅 [**HostName 构造函数**](https://msdn.microsoft.com/library/windows/apps/br207118.aspx)，了解详细信息）。

如果提供了有效的 **HostName** 对象，即可检索 **RemoteSystem** 对象。 如果地址数据无效，将返回 `null` 对象引用。

[!code-cs[Main](./code/DiscoverDevices/MainPage.xaml.cs#SnippetFindByHostName)]

## 相关主题
[连接的应用和设备（项目“Rome”）](connected-apps-and-devices.md)  
[启动远程应用](launch-a-remote-app.md)  
[远程系统 API 参考](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems)  
[远程系统示例](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems )演示了如何发现远程系统、在远程系统上启动应用，以及使用应用服务在两个系统上运行的应用之间发送消息。



<!--HONumber=Nov16_HO1-->


