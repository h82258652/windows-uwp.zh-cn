---
Description: Windows 推送通知服务 (WNS) 使第三方开发人员可从自己的云服务发送 Toast、磁贴、锁屏提醒和原始更新。 有许多种发送通知的方法，具体取决于应用程序的需要
title: 选择正确的推送通知通道类型
ms.date: 07/07/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 502395d1daa698e1b05e40f355e65f074219e9a5
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970852"
---
# <a name="choosing-the-right-push-notification-channel-type"></a>选择正确的推送通知通道类型

本文介绍三种类型的 Windows 推送通知通道（主要、次要和备用），可帮助你将内容传递到应用。 

（有关如何创建推送通知的详细信息，请参阅[Windows 推送 Notification Services （WNS）概述](../tiles-and-notifications/windows-push-notification-services--wns--overview.md)。） 

## <a name="types-of-push-channels"></a>推送通道的类型 

有三种类型的推送通道可用于将通知发送到 Windows 应用。 它们是： 

[主要通道](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanagerforuser#Methods_) -“传统”推送通道。 应用商店中的任何应用均可使用来发送 toast、磁贴、原始或徽章通知。 [在此处了解详细信息](windows-push-notification-services--wns--overview.md)。

[辅助磁贴通道](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanagerforuser#Methods_)-用于将磁贴更新推送到辅助磁贴。 仅用于将磁贴或锁屏提醒通知发送到固定在用户“开始”屏幕上的辅助磁贴

[备用通道](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanagerforuser#Methods_) - 在创意者更新中添加的一种新型通道。 它允许将原始通知发送到任何 Windows 应用，包括未在存储中注册的应用。 

> [!NOTE]
> 无论你使用哪种推送通道，在设备上运行你的应用后，应用将始终能够发送本地 Toast、磁贴或锁屏提醒通知。 它可以利用前台应用进程或后台任务发送本地通知。 


## <a name="primary-channels"></a>主要通道

目前，此类通道是 Windows 上最常用的通道，而且几乎适用于各种将通过 Microsoft Store 分发应用的情况。 可使用这些通道向应用发送各种类型的通知。 

### <a name="what-do-primary-channels-enable"></a>主要通道允许执行哪些操作？

-   **将磁贴或锁屏提醒更新发送给主要磁贴。** 如果用户已选择将磁贴固定到“开始”屏幕，那么这是你进行展示的大好时机。 在应用中发送更新以及有用的信息或经验提醒。 
-   **发送 Toast 通知。** 利用 Toast 通知，可以将一些信息立即呈现在用户面前。 这些通知通过 shell 显示在大多数应用的顶部，并且位于操作中心，以便用户以后可以返回去与之进行交互。 
-   **发送原始通知以触发后台任务。** 有时你需要根据通知代表用户执行一些工作。 原始通知允许运行应用的后台任务 
-   **在传输过程中让 Windows 使用 TLS 提供邮件加密。** 系统会对进入 WNS 以及转至用户设备的邮件进行在线加密。  

### <a name="limitations-of-primary-channels"></a>主要通道的限制

-   要求使用 WNS REST API 来推送通知，这种推送方法不是设备供应商的标准方法。 
-   只能为每个应用创建一个通道 
-   需要在 Microsoft Store 中注册应用

### <a name="creating-a-primary-channel"></a>创建主要通道 

```csharp
PushNotificationChannel channel = 
    await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
```

## <a name="secondary-tile-channels"></a>辅助磁贴通道

这些通道可用于将磁贴和锁屏提醒更新推送到辅助磁贴。 应用使用这些通道告知用户他们感兴趣的操作，或他们可在应用中与之进行交互的信息，例如群聊中的新消息或更新的体育比赛分数。 

### <a name="what-do-secondary-tile-channels-enable"></a>辅助磁贴通道允许执行哪些操作？

-   将磁贴或锁屏提醒通知发送到辅助磁贴。 辅助磁贴是吸引用户返回到应用中的一种很好的方法。 它们是到用户所关注的信息的深层链接，并且将相关信息放在磁贴上有助于使用户再三返回来查看。
-   在各种磁贴之间分隔通道（和到期）。 通过执行此操作，你可以在用户可能固定到其“开始”屏幕的各种辅助磁贴之间分隔后端中的逻辑。 
-   在传输过程中让 Windows 使用 TLS 提供邮件加密。 系统会对进入 WNS 以及转至用户设备的邮件进行在线加密。  

### <a name="limitations-of-secondary-tile-channels"></a>辅助磁贴通道的限制

-   不允许发送 Toast 或原始通知。 系统会忽略发送到辅助磁贴的 toast 或原始通知。
-   需要在 Microsoft Store 中注册应用


### <a name="creating-a-secondary-tile-channel"></a>创建辅助磁贴通道 

```csharp
PushNotificationChannel channel = 
    await PushNotificationChannelManager.CreatePushNotificationChannelForSecondaryTileAsync(tileId);
```

## <a name="alternate-channels"></a>备用通道

利用备用通道，应用能够发送推送通知，而无需向 Microsoft Store 注册或在用于应用的主要通道之外创建推送通道。 
 
### <a name="what-do-alternate-channels-enable"></a>备用通道允许执行哪些操作？
-   将原始推送通知发送到在任何 Windows 设备上运行的 Windows。 备用通道仅允许原始通知（但你仍可将后台任务唤醒以在本地显示 toast 或磁贴通知）。
-   允许应用为应用内的不同功能创建多个原始推送通道。 应用最多可以创建 1000 个备用通道，每个通道的有效期为 30 天。 应用可以单独管理或吊销每个通道。
-   无需在 Microsoft Store 中注册应用即可创建备用推送通道。 如果计划在设备上安装应用但不在 Microsoft Store 中注册，该应用仍可接收推送通知。
-   服务器可以使用 W3C 标准 REST API 和 VAPID 协议推送通知。 备用通道使用 W3C 标准协议，这样，你可以简化需要维护的服务器逻辑。
-   完全端到端消息加密。 虽然主要通道会在传输过程中提供加密，但是如果想要更加安全，则可以使用备用通道，这样，你的应用可以传递加密标头来保护邮件。 

### <a name="limitations-of-alternate-channels"></a>备用通道的限制
-   应用的服务器无法发送推送 toast、磁贴或徽章类型通知。 只能发送推送原始通知。 你的应用仍然可以利用后台任务发送本地通知。 
-   需要的 REST API 不同于主要或辅助磁贴通道所需的 REST API。 使用标准 W3C REST API 意味着你的应用将需要使用不同的逻辑来发送推送 Toast 或磁贴更新

### <a name="creating-an-alternate-channel"></a>创建备用通道 

```csharp
PushNotificationChannel webChannel = 
    await PushNotificationChannelManager.GetDefault().CreateRawPushNotificationChannelWithAlternateKeyForApplicationAsync(applicationServerKey, appChannelId);
```

## <a name="channel-type-comparison"></a>通道类型比较
下面对不同类型的通道进行了快速比较：

<table>

<tr class="header">
<th align="left">类型 </th>
<th align="left"><b>推送 Toast？</b></th>
<th align="left"><b>推送磁贴/锁屏提醒？</b></th>
<th align="left"><b>推送原始通知？</b></th>
<th align="left"><b>身份验证</b></th>
<th align="left"><b>API</b></th>
<th align="left"><b>需要在应用商店中注册？</b></th>
<th align="left"><b>通道</b></th>
<th align="left"><b>加密</b></th>
</tr>


<tr class="odd">
<td align="left">基本</td>
<td align="left">是</td>
<td align="left">是 - 仅限主要磁贴</td>
<td align="left">是</td>
<td align="left">OAuth</td>
<td align="left">WNS REST API</td>
<td align="left">是</td>
<td align="left">每个应用一个</td>
<td align="left">在传输过程中</td>
</tr>
<tr class="even">
<td align="left">辅助磁贴</td>
<td align="left">否</td>
<td align="left">是 - 仅限辅助磁贴</td>
<td align="left">否</td>
<td align="left">OAuth</td>
<td align="left">WNS REST API</td>
<td align="left">是</td>
<td align="left">每个辅助磁贴一个</td>
<td align="left">在传输过程中</td>
</tr>
<tr class="odd">
<td align="left">备用</td>
<td align="left">否</td>
<td align="left">否</td>
<td align="left">是</td>
<td align="left">VAPID</td>
<td align="left">WebPush W3C 标准</td>
<td align="left">否</td>
<td align="left">每个应用 1000 个</td>
<td align="left">在传输过程中，并且可以通过标头传输进行端到端加密（需要应用代码）</td>
</tr>
</table>

## <a name="choosing-the-right-channel"></a>选择正确的通道

通常，我们建议在应用中使用主要通道，但有一些例外： 

1. 如果要将磁贴更新推送到辅助磁贴，请使用辅助磁贴推送通道。
2. 如果要将通道传出到其他服务（例如，在使用浏览器时），请使用备用通道。
3. 如果要创建的应用（例如 LOB 应用）将不会在 Windows 应用商店中列出，请使用备用通道。
4. 如果你的服务器上已有希望重复使用的 Web 推送代码，并且后台服务中需要多个通道，请使用备用通道。

## <a name="related-articles"></a>相关文章

* [发送本地磁贴通知](../tiles-and-notifications/sending-a-local-tile-notification.md)
* [自适应和交互式 Toast 通知](../tiles-and-notifications/adaptive-interactive-toasts.md)
* [快速入门：发送推送通知](https://docs.microsoft.com/previous-versions/windows/apps/hh868252(v=win.10))
* [如何通过推送通知更新锁屏提醒](https://docs.microsoft.com/previous-versions/windows/apps/hh465450(v=win.10))
* [如何请求、创建和保存通知通道](https://docs.microsoft.com/previous-versions/windows/apps/hh465412(v=win.10))
* [如何为正在运行的应用程序截获通知](https://docs.microsoft.com/previous-versions/windows/apps/hh465450(v=win.10))
* [如何使用 Windows 推送通知服务 (WNS) 进行验证](https://docs.microsoft.com/previous-versions/windows/apps/hh465407(v=win.10))
* [推送通知服务请求和响应头](https://docs.microsoft.com/previous-versions/windows/apps/hh465435(v=win.10))
* [推送通知指南和清单](https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview)
* [原始通知](raw-notification-overview.md)
