---
author: mijacobs
Description: Raw notifications are short, general purpose push notifications.
title: 原始通知概述
ms.assetid: A867C75D-D16E-4AB5-8B44-614EEB9179C7
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3e1a015d5d51ad0c15f20755afcb0d324acd1f36
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/02/2018
ms.locfileid: "5979915"
---
# <a name="raw-notification-overview"></a>原始通知概述


原始通知是简短的一般用途的推送通知。 原始通知有严格的指令，不包含 UI 组件。 与其他推送通知一样，Windows 推送通知服务 (WNS) 功能提供从云服务到应用的原始通知。

你可以将原始通知用于各种用途，其中包括触发应用以运行后台任务（如果用户已给予如此操作的应用权限）。 通过使用 WNS 与应用通信，可以避免创建持久的套接字连接、发送 HTTP GET 消息以及其他服务到应用连接的处理开销。

> [!IMPORTANT]
> 若要了解原始通知，最好熟悉一下 [Windows 推送通知服务 (WNS) 概述](windows-push-notification-services--wns--overview.md)中讨论的概念。

 

与 Toast、磁贴和锁屏提醒推送通知一样，原始通知从应用的云服务通过分配的通道统一资源标识符 (URI) 推送到 WNS。 WNS 反过来将通知传送到与该通道关联的设备和用户帐户。 与其他推送通知不同，原始通知没有指定的格式。 负载的内容完全由应用定义。

以一个从原始通知获益的应用为例，让我们看一个理论上的文档协作应用。 请考虑两名用户正在同时编辑同一个文档。 托管共享文档的云服务可以使用原始通知在其他用户进行更改时通知每个用户。 原始通知没必要包含对文档的更改，但用信号通知每个用户的应用副本联系中心位置并同步可用的更改。 通过使用原始通知，应用及其云服务可以不必在文档打开期间一直保持持久连接。

## <a name="how-raw-notifications-work"></a>原始通知的工作原理


所有原始通知都是推送通知。 因此，发送和接收推送通知所需的设置也适用于原始通知：

-   必须具有有效的 WNS 通道才能发送原始通知。 有关获取推送通知通道的详细信息，请参阅[如何请求、创建和保存通知通道](https://msdn.microsoft.com/library/windows/apps/hh465412)。
-   必须在应用的清单中包含 **Internet** 功能。 在 Microsoft Visual Studio 清单编辑器中，你可以在**功能**选项卡下看到此选项，即 **Internet (客户端)**。 有关详细信息，请参阅[**功能**](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-capabilities)。

通知的正文采用应用定义的格式。 客户端收到只需应用理解的以 null 终止的字符串 (**HSTRING**) 形式的数据。

如果客户端处于离线状态，则仅当通知中包含 [X-WNS-Cache-Policy](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#pncodes_x_wns_cache) 标头时 WNS 才会缓存原始通知。 但是，将只缓存一个原始通知，并且在设备变回联机后传递。

要使原始通知呈现在客户端上只有三种可能的途径：通过通知传递事件将通知传递给正在运行的应用、发送给后台任务或丢弃。 因此，如果客户端脱机并且 WNS 尝试传递原始通知，则会丢弃通知。

## <a name="creating-a-raw-notification"></a>创建原始通知


发送原始通知与发送磁贴、Toast 或锁屏提醒推送通知类似，但有以下不同：

-   HTTP Content-Type 标头必须设置为“application/octet-stream”。
-   HTTP [X-WNS-Type](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#pncodes_x_wns_type) 标头必须设置为“wns/raw”。
-   通知正文可以包含大小小于 5 KB 的任何字符串负载。

原始通知主要用作触发应用采取操作（如直接联系服务以同步大量数据或根据通知内容进行本地状态修改）的短消息。 请注意，WNS 推送通知不能保证传递，因此应用和云服务必须说明原始通知有可能无法达到客户端，如当客户端脱机时。

有关发送推送通知的详细信息，请参阅[快速入门：发送推送通知](https://msdn.microsoft.com/library/windows/apps/xaml/hh868252)。

## <a name="receiving-a-raw-notification"></a>接收原始通知


有两种方法可通过应用接收原始通知：

-   当应用程序正在运行时通过[通知传递事件](#notification-delivery-events)。
-   当应用支持允许后台任务时，通过[原始通知触发的后台任务](#background-tasks-triggered-by-raw-notifications)。

应用可以使用这两种机制来接收原始通知。 如果应用既实现通知传递事件处理程序，又实现原始通知触发的后台任务，则当应用正在运行时将优先采用通知传递事件。

-   如果应用正在运行，则通知传递事件将优先于后台任务，并且应用将获得最先处理通知的机会。
-   通知传递事件处理程序可以通过将事件的 [**PushNotificationReceivedEventArgs.Cancel**](https://docs.microsoft.com/uwp/api/Windows.Networking.PushNotifications.PushNotificationReceivedEventArgs.Cancel) 属性设置为 **true** 来指定处理程序退出后不应将原始通知传递给它的后台任务。 如果 **Cancel** 属性设置为 **false** 或者未设置（默认值为 **false**），则原始通知将在通知传递事件处理程序完成其工作后触发后台任务。

### <a name="notification-delivery-events"></a>通知传递事件

你的应用可以使用通知传递事件 ([**PushNotificationReceived**](https://docs.microsoft.com/uwp/api/Windows.Networking.PushNotifications.PushNotificationChannel.PushNotificationReceived)) 以在应用使用时接收原始通知。 当云服务发送原始通知时，正在运行的应用可以通过在通道 URI 上处理通知传递事件来接收该通知。

如果应用未运行并且未使用[后台任务](#background-tasks-triggered-by-raw-notifications)，则 WNS 会在接收到发送给该应用的任何原始通知时将其丢弃。 若要避免浪费云服务的资源，则应考虑在服务上实现逻辑以跟踪应用是否处于活动状态。 存在此信息的两种源：应用可以显式告知服务它已准备好开始接收通知，并且 WNS 可以告知服务何时停止。

-   **应用通知云服务**：应用可以联系其服务来告知服务应用正在前台运行。 这种方法的缺点是应用可以非常频繁地终止联系服务。 但优点是服务将始终知道应用何时准备好接收传入的原始通知。 另一个优点是，当应用联系其服务时，服务随即知道将原始通知发送到该应用的特定实例，而不进行广播。
-   **云服务响应 WNS 响应消息**：应用服务可以使用 WNS 返回的 [X-WNS-NotificationStatus](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#pncodes_x_wns_notification) 和 [X-WNS-DeviceConnectionStatus](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#pncodes_x_wns_dcs) 信息来确定何时停止向应用发送原始通知。 当服务向一个通道发送 HTTP POST 形式的通知时，它会在响应中收到以下消息之一：

    -   **X-WNS-NotificationStatus: dropped**：这指示通知未被客户端接收。 可以可靠地假设应用导致的 **dropped** 响应不再位于用户设备的前台。
    -   **X-WNS-DeviceConnectionStatus: disconnected** 或 **X-WNS-DeviceConnectionStatus: tempconnected**：这指示 Windows 客户端不再具有到 WNS 的连接。 请注意，若要从 WNS 接收此消息，你必须通过在通知的 HTTP POST 中设置 [X-WNS-RequestForStatus](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#pncodes_x_wns_request) 标头来询问它。

    应用的云服务可以在这些状态消息中使用该信息来停止通过原始通知的通信尝试。 当应用切换回前台时，服务可以在应用联系它时恢复发送原始通知。

    请注意，你不应依赖 [X-WNS-NotificationStatus](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#pncodes_x_wns_notification) 来确定通知是否已成功传递到客户端。

    有关详细信息，请参阅 [推送通知服务请求和响应头](https://msdn.microsoft.com/library/windows/apps/hh465435)

### <a name="background-tasks-triggered-by-raw-notifications"></a>原始通知触发的后台任务

> [!IMPORTANT]
> 使用原始通知后台任务之前，必须通过 [**BackgroundExecutionManager.RequestAccessAsync**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundExecutionManager#Windows_ApplicationModel_Background_BackgroundExecutionManager_RequestAccessAsync_System_String_) 向应用授予后台访问权限。

 

必须使用 [**PushNotificationTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.PushNotificationTrigger) 注册后台任务。 如果未注册，则当收到原始通知时该任务不会运行。

通过由原始通知触发后台任务，你的应用的云服务可联系你的应用，即使在应用未运行（尽管可触发该应用运行）时也可以如此。 这在应用无需保持连续的连接时发生。 原始通知是可以触发后台任务的唯一的通知类型。 尽管 Toast、磁贴和锁屏提醒推送通知不能触发后台任务，但是原始通知触发的后台任务可以通过本地 API 调用来更新磁贴和调用 Toast 通知。

为了说明原始通知触发的后台任务的工作原理，让我们考虑一个用于阅读电子书籍的应用。 首先，用户可能在另一个设备上购买一本在线书籍。 作为响应，应用的云服务可以将原始通知发送到用户的每个设备，并带有声明书籍已购买且应用应下载书籍的负载。 然后，应用直接联系应用的云服务以便开始在后台下载新的书籍，这之后，当用户启动应用时，该书籍便已经在那了并且可以阅读。

若要使用原始通知触发后台任务，你的应用必须：

1.  通过使用 [**BackgroundExecutionManager.RequestAccessAsync**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundExecutionManager#Windows_ApplicationModel_Background_BackgroundExecutionManager_RequestAccessAsync_System_String_) 请求在后台运行任务的权限（用户可以随时吊销这些任务）。
2.  实现后台任务。 有关详细信息，请参阅[通过使用后台任务支持应用](../../../launch-resume/support-your-app-with-background-tasks.md)

此后，在每次接收到应用的原始通知时，都会调用后台任务以响应 [**PushNotificationTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.PushNotificationTrigger)。 后台任务解释原始通知的应用特定的负载并对其进行操作。

对于每个应用，一次只能运行一个后台任务。 如果为已在运行后台任务的应用触发后台任务，则必须先完成第一个后台任务，然后才能运行新的后台任务。

## <a name="other-resources"></a>其他资源


你可以了解详细信息通过 Windows8.1，[推送和定期通知示例](http://go.microsoft.com/fwlink/p/?LinkId=231476)的[原始通知示例](http://go.microsoft.com/fwlink/p/?linkid=241553)下载 Windows8.1，用于在 windows 10 应用中重新使用其源代码。

## <a name="related-topics"></a>相关主题

* [原始通知指南](https://msdn.microsoft.com/library/windows/apps/hh761463)
* [快速入门：创建并注册原始通知后台任务](https://msdn.microsoft.com/library/windows/apps/jj676800)
* [快速入门：为正在运行的应用截获推送通知](https://msdn.microsoft.com/library/windows/apps/jj709908)
* [**RawNotification**](https://docs.microsoft.com/uwp/api/Windows.Networking.PushNotifications.RawNotification)
* [**BackgroundExecutionManager.RequestAccessAsync**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundExecutionManager#Windows_ApplicationModel_Background_BackgroundExecutionManager_RequestAccessAsync_System_String_)
 

 




