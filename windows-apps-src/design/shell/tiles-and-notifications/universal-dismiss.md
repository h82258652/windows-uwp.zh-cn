---
author: andrewleader
Description: Learn how to use Universal Dismiss on your toast notifications.
title: 全局消除
label: Universal Dismiss
template: detail.hbs
ms.author: mijacobs
ms.date: 12/15/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, toast, 云中的操作中心, 全局消除, 通知, 跨设备, 一次消除即全部消除
ms.localizationpriority: medium
ms.openlocfilehash: 90ad60949504d4478341ff9455fe0f7da90d78a9
ms.sourcegitcommit: 106aec1e59ba41aae2ac00f909b81bf7121a6ef1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/15/2018
ms.locfileid: "4621632"
---
# <a name="universal-dismiss"></a>全局消除

由云中的操作中心提供支持的全局消除操作意味着，从某个设备消除通知时，也将消除其他设备上的相同通知。

> [!IMPORTANT]
> **需要周年更新**：必须面向 SDK 14393 且运行版本 14393 或更高版本才能使用全局消除。

此方案的常见示例是日历提醒...所拥有的两种设备上均安装有日历应用...在手机和桌面设备上收到提醒...在桌面设备上单击“消除”...由于启用了全局消除，手机上的提醒也一并消除！ **仅需一行代码即可启用全局消除！**

<img alt="Diagram of Universal Dismiss" src="images/universal-dismiss.gif" width="406"/>

在此方案中，关键一点是**多台设备上安装了同一应用**，这意味着**每台设备都会收到通知**。 日历应用是典型示例，因为通常会在 Windows 电脑和手机中安装相同的日历应用，并且该应用的每个实例已在每台设备上发送通知。 通过添加对全局消除的支持，可以跨设备链接这些相同提醒的实例。


## <a name="how-to-enable-universal-dismiss"></a>如何启用全局消除

对于开发人员，启用全局消除非常容易。 只需提供一个可实现跨设备链接每个通知的 ID，用户从一台设备中消除通知时，便可同时消除其他设备中链接的相应通知。

![全局消除 RemoteId 图](images/universal-dismiss-remoteid.jpg)

> **RemoteId**：用于*跨设备*唯一标识通知的标识符。

只需一行代码即可添加 RemoteId，从而启用对全局消除的支持！ 由你来决定如何生成 RemoteId，但需要确保它能够跨设备唯一标识通知，并且可从同一应用在不同设备上运行的不同实例生成相同的标识符。

例如，在我的家庭作业计划器应用中，我通过将类型设为“提醒”来生成 RemoteId，然后含入联机帐户 ID 和家庭作业项目的联机标识符。 无论哪个设备发送通知，总是能够生成完全相同的 RemoteId，因为这些在线 ID 已在设备之间共享。

```csharp
var toast = new ScheduledToastNotification(content.GetXml(), startTime);
 
// If the RemoteId property is present
if (ApiInformation.IsPropertyPresent(typeof(ScheduledToastNotification).FullName, nameof(ScheduledToastNotification.RemoteId)))
{
    // Assign the RemoteId to add support for Universal Dismiss
    toast.RemoteId = $"reminder_{account.AccountId}_{homework.Identifier}"
}
  
ToastNotificationManager.CreateToastNotifier().AddToSchedule(toast);
```

以下代码同时在手机和桌面应用上运行，这意味着两台设备上的通知将具有相同的 RemoteId。

以上就是要执行的全部操作！ 当用户消除（或单击）某个通知时，系统会检查它是否具有 RemoteId，如果有，会在该用户的所有设备上消除该 RemoteId。

**已知问题**：通过 `ToastNotificationHistory.GetHistory()` API 检索 **RemoteId** 始终返回空字符串，而不是所指定的 **RemoteId**。 不必担心，一切都正常运行 - 只是在检索已损坏的值。

> [!NOTE]
> 如果用户或企业对应用禁用[通知镜像](notification-mirroring.md)（或完全禁用通知镜像），则全局消除将无法运行，因为云中没有相应通知。


## <a name="supported-devices"></a>支持的设备

自周年更新起，Windows 移动版和 Windows 桌面版均支持全局消除。 全局消除可在电脑与电脑、电脑与手机及手机与手机之间双向运作。