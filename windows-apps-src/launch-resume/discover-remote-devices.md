---
title: 发现远程设备
description: 了解如何通过使用项目“Rome”发现应用的远程设备。
ms.assetid: 5b4231c0-5060-49e2-a577-b747e20cf633
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10，uwp，连接设备、 远程系统、 rome、 项目 rome
ms.localizationpriority: medium
ms.openlocfilehash: 7788cb546eddf77292210b5b1e8268239504a843
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2018
ms.locfileid: "7706220"
---
# <a name="discover-remote-devices"></a>发现远程设备
你的应用可以使用无线网络、蓝牙和云连接来发现使用发现设备的相同 Microsoft 帐户登录的 Windows 设备。 无需安装任何特殊软件，即可发现远程设备。

> [!NOTE]
> 本指南假设你已经按照[启动远程应用](launch-a-remote-app.md)中的步骤授予远程系统功能的访问权限。

## <a name="filter-the-set-of-discoverable-devices"></a>筛选可发现的设备集
你可以将 [**RemoteSystemWatcher**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemWatcher) 与筛选器结合使用，缩小可发现的设备集。 筛选器可以检测发现类型（邻近与本地网络与云连接）、设备类型（桌面设备、移动设备、Xbox、Hub 和全息设备）以及可用性状态（要使用远程系统功能的设备的可用性状态）。

在初始化 **RemoteSystemWatcher** 对象之前或之时，必须构建筛选器对象，因为它们将作为一项参数传入它的构造函数中。 以下代码可为每一种可用类型都创建一个筛选器，然后将它们添加到列表。

> [!NOTE]
> 这些示例中的代码要求文件中具有 `using Windows.System.RemoteSystems` 语句。

[!code-cs[Main](./code/DiscoverDevices/MainPage.xaml.cs#SnippetMakeFilterList)]

> [!NOTE]
> “proximal”筛选器值不能保证位置邻近的程度。 对于需要可靠的位置邻近的情形，请在你的筛选器中使用值 [**RemoteSystemDiscoveryType.SpatiallyProximal**](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemdiscoverytype)。 当前，此筛选器只允许通过蓝牙发现的设备。 当支持新发现机制和保证位置邻近的协议时，此处也会将其包括进来。  
[**RemoteSystem**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystem) 类中还有一个属性用于指示已发现的设备是否实际处于邻近位置：[**RemoteSystem.IsAvailableBySpatialProximity**](https://docs.microsoft.com/uwp/api/Windows.System.RemoteSystems.RemoteSystem.IsAvailableByProximity)。

> [!NOTE]
> 如果打算通过本地网络发现设备（由发现类型筛选条件选择决定），则网络需要使用“私有”或“域”配置文件。 你的设备不会通过“公共”网络发现其他设备。

创建 [**IRemoteSystemFilter**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.IRemoteSystemFilter) 对象列表后，可以将其传入 **RemoteSystemWatcher** 的构造函数。

[!code-cs[Main](./code/DiscoverDevices/MainPage.xaml.cs#SnippetCreateWatcher)]

当调用该观察程序的 [**Start**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemWatcher.Start) 方法时，只有检测到满足以下全部条件的设备，才会引发 [**RemoteSystemAdded**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemWatcher.RemoteSystemAdded) 事件。
* 可以通过邻近连接发现该设备
* 它是一台桌面设备或手机
* 它被归类为可用

从此处开始，处理事件、检索 [**RemoteSystem**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystem) 对象和连接到远程设备的过程与[启动远程应用](launch-a-remote-app.md)中的过程完全相同。 简而言之，**RemoteSystem** 对象作为 [**RemoteSystemAddedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemAddedEventArgs) 对象的属性存储，这些都随每个 **RemoteSystemAdded** 事件传递。

## <a name="discover-devices-by-address-input"></a>通过地址输入发现设备
某些设备可能与用户没有关联，或无法通过扫描发现，但是如果发现应用使用直接地址，仍可以查找到它们。 [**HostName**](https://msdn.microsoft.com/library/windows/apps/windows.networking.hostname.aspx) 类用于表示远程设备的地址。 该地址通常以 IP 地址的形式存储，但也允许存储为其他几种格式（请参阅 [**HostName 构造函数**](https://msdn.microsoft.com/library/windows/apps/br207118.aspx)，了解详细信息）。

如果提供了有效的 **HostName** 对象，即可检索 **RemoteSystem** 对象。 如果地址数据无效，将返回 `null` 对象引用。

[!code-cs[Main](./code/DiscoverDevices/MainPage.xaml.cs#SnippetFindByHostName)]

## <a name="querying-a-capability-on-a-remote-system"></a>查询远程系统上的功能

尽管与发现筛选分离，但查询设备功能也是发现过程中的一个重要部分。 使用 [**RemoteSystem.GetCapabilitySupportedAsync**](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystem.GetCapabilitySupportedAsync) 方法，你可以查询已发现的远程系统是否支持特定功能，如远程会话连接或空间实体（全息）共享。 有关可查询功能的列表，请参阅 [**KnownRemoteSystemCapabilities**](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.knownremotesystemcapabilities) 类。

```csharp
// Check to see if the given remote system can accept LaunchUri requests
bool isRemoteSystemLaunchUriCapable = remoteSystem.GetCapabilitySupportedAsync(KnownRemoteSystemCapabilities.LaunchUri);
```

## <a name="cross-user-discovery"></a>跨用户发现

开发人员可以指定发现邻近客户端设备的_所有_设备，而不仅仅是已向同一用户注册的设备。 这是通过特殊的 **IRemoteSystemFilter** - [**RemoteSystemAuthorizationKindFilter**](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemauthorizationkindfilter) 来实现的。 其实现与其他筛选器类型相似：

```csharp
// Construct a user type filter that includes anonymous devices
RemoteSystemAuthorizationKindFilter authorizationKindFilter = new RemoteSystemAuthorizationKindFilter(RemoteSystemAuthorizationKind.Anonymous);
// then add this filter to the RemoteSystemWatcher
```

* [**RemoteSystemAuthorizationKind**](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemauthorizationkind) 值 **Anonymous** 将允许发现所有邻近设备，即使来自不受信任的用户也是如此。
* 值 **SameUser** 只将发现筛选到与客户端设备注册到同一用户的设备。 这是默认行为。

### <a name="checking-the-cross-user-sharing-settings"></a>检查跨用户共享设置

除了在发现应用中指定的上述筛选器外，还必须对客户端设备本身进行配置，以允许来自登录设备的与其他用户之间的共享体验。 这是一种系统设置，可用 **RemoteSystem** 类中的静态方法进行查询：

```csharp
if (!RemoteSystem.IsAuthorizationKindEnabled(RemoteSystemAuthorizationKind.Anonymous)) {
    // The system is not authorized to connect to cross-user devices. 
    // Inform the user that they can discover more devices if they
    // update the setting to "Anonymous".
}
```

若要更改此设置，用户必须打开**设置**应用。 在**系统** > **共享体验** > **跨设备共享**菜单中有一个下拉框，用户可在此指定系统可与哪些设备共享体验。

![共享体验设置页面](images/shared-experiences-settings.png)

## <a name="related-topics"></a>相关主题
* [连接的应用和设备（项目“Rome”）](connected-apps-and-devices.md)
* [启动远程应用](launch-a-remote-app.md)
* [远程系统 API 参考](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems)
* [远程系统示例](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems)
