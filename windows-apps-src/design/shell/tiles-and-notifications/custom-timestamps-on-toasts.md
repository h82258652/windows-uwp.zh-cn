---
author: andrewleader
Description: Learn how to use custom timestamps on your toast notifications.
title: Toast 上的自定义时间戳
label: Custom timestamps on toasts
template: detail.hbs
ms.author: mijacobs
ms.date: 12/15/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, toast, 自定义时间戳, 时间戳, 通知, 操作中心
ms.localizationpriority: medium
ms.openlocfilehash: 7ef01feaf422674977dc4549d4cc68a2ca0052c7
ms.sourcegitcommit: a160b91a554f8352de963d9fa37f7df89f8a0e23
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/21/2018
ms.locfileid: "4122918"
---
# <a name="custom-timestamps-on-toasts"></a>Toast 上的自定义时间戳

默认情况下，Toast 通知（操作中心内可见）上的时间戳设置为发送通知的时间。

<img alt="Toast with custom timestamp" src="images/toast-customtimestamp.jpg" width="396"/>

可以选择用自己的自定义日期和时间替代时间戳，使时间戳表示消息/信息/内容实际创建的时间，而不是发送通知的时间。 这还可以确保通知以正确的顺序在操作中心显示（按时间排序）。 我们建议大多数应用指定自定义时间戳。

> [!IMPORTANT]
> **需要创意者更新和通知库 1.4.0**：必须运行内部版本 15063 或更高版本，以查看自定义时间戳。 必须使用版本 1.4.0 或更高版本的 [UWP 社区工具包通知 NuGet 库](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)来分配 Toast 的内容上的时间戳。

若要使用自定义时间戳，只需分配 **ToastContent** 上的 **DisplayTimestamp** 属性。

```csharp
ToastContent toastContent = new ToastContent()
{
    DisplayTimestamp = new DateTime(2017, 04, 15, 19, 45, 00, DateTimeKind.Utc),
    ...
};
```

```xml
<toast displayTimestamp="2017-04-15T19:45:00Z">
  ...
</toast>
```

如果使用 XML，则必须将日期格式设置为 [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601)。

> [!NOTE]
> 针对秒，最多只能使用 3 个小数位（虽然实际上提供任何具体内容都没有值）。 如果使用更多小数位，有效负载将无效，你将收到“新通知”通知。


## <a name="usage-guidance"></a>用法指南

一般情况下，我们建议大多数应用指定自定义时间戳。 这可以确保通知的时间戳准确地表示消息/信息/内容的生成时间，不受网络延迟、飞行模式或定期后台任务的固定间隔的影响。

例如，新闻应用可能每 15 分钟运行一次后台任务，检查新文章并显示通知。 在使用自定义时间戳之前，时间戳对应 Toast 通知生成的时间（因此始终间隔 15 分钟）。 但是，现在应用可以将时间戳设置为文章实际发布的时间。 同样，如果电子邮件应用和社交网络应用的通知使用类似的定期请求模式，则这些应用也可以利用这一功能。

此外，提供自定义时间戳可以确保时间戳是正确的，即使用户断开与 Internet 的连接。 例如，当用户打开计算机，后台任务运行时，最终可以确保通知上的时间戳表示发送邮件的时间，而不是用户打开计算机的时间。


## <a name="default-timestamp"></a>默认时间戳

如果不提供自定义时间戳，我们会使用发送通知的时间。

如果通过 WNS 发送推送通知，我们会使用 WNS 服务器收到通知的时间（因此将通知传送到设备过程中的任何延迟都不会影响时间戳）。

如果已发送本地通知，我们会使用通知平台收到通知的时间（应该是即时）。


## <a name="related-topics"></a>相关主题

- [发送本地 toast](send-local-toast.md)
- [toast 内容文档](adaptive-interactive-toasts.md)