---
Description: You can log custom events from your UWP app and review those events in the Usage report in Partner Center.
title: 记录合作伙伴中心的自定义事件
ms.date: 06/01/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store Services SDK, 日志事件
ms.assetid: 4aa591e0-c22a-4c90-b316-0b5d0410af19
ms.localizationpriority: medium
ms.openlocfilehash: d7b338fd3b34d530ad365b0377d6b6c6c65398b7
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2018
ms.locfileid: "8476210"
---
# <a name="log-custom-events-for-partner-center"></a>记录合作伙伴中心的自定义事件

在合作伙伴中心中的[使用情况报告](https://msdn.microsoft.com/windows/uwp/publish/usage-report)可供你获取有关你的通用 Windows 平台 (UWP) 应用中所定义的自定义事件的信息。 自定义事件是表示应用中的某个事件或活动的任意字符串。 例如，游戏可能定义名为 *firstLevelPassed*、*secondLevelPassed* 等的自定义事件，用户在游戏中通过每个关卡时记录这些事件。

若要记录应用中的自定义事件，请将自定义事件字符串传递到 Microsoft Store Services SDK 提供的 [Log](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log) 方法。 你可以自定义事件[使用情况报告](https://msdn.microsoft.com/windows/uwp/publish/usage-report)合作伙伴中心中的**自定义事件**部分中查看的总发生次数。

> [!NOTE]
> 与[Windows 事件](https://msdn.microsoft.com/library/windows/desktop/aa964766.aspx)，无关登录到合作伙伴中心的自定义事件，并它们不会显示在**事件查看器**。

## <a name="prerequisites"></a>先决条件

你可以为你的应用在合作伙伴中心中查看的**使用情况报告**中的自定义日志记录事件之前，必须在应用商店中发布你的应用。

## <a name="how-to-log-custom-events"></a>如何记录自定义事件

1. 如果先前尚未这样做，请在开发计算机上[安装 Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk)。

2. 在 Visual Studio 中打开你的项目。

3. 在“解决方案资源管理器”中，右键单击你的项目的**引用**节点，然后单击**添加引用**。

4. 在**引用管理器**中，展开**通用 Windows** 并单击**扩展**。

5. 在 SDK 列表中，单击 **Microsoft 协议框架**旁边的复选框，然后单击**确定**。

6. 将以下语句添加到要记录自定义事件的每个代码文件的顶部。
    [!code-cs[EventLogger](./code/StoreSDKSamples/cs/LogEvents.cs#EngagementNamespace)]

7. 在要记录自定义事件的每个代码部分中，获取 [StoreServicesCustomEventLogger](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log) 对象，然后调用 [Log](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log) 方法。 将自定义事件字符串传递到该方法。
    [!code-cs[EventLogger](./code/StoreSDKSamples/cs/LogEvents.cs#Log)]

    > [!NOTE]
    > 如果应用记录了很多名称很长的自定义事件，则[使用情况报告](https://msdn.microsoft.com/windows/uwp/publish/usage-report)的加载时间可能很长。 建议为自定义事件使用简短的名称。 

## <a name="related-topics"></a>相关主题

* [使用情况报告](https://msdn.microsoft.com/windows/uwp/publish/usage-report)
* [记录方法](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log)
* [Microsoft Store Services SDK](https://msdn.microsoft.com/windows/uwp/monetize/microsoft-store-services-sdk)
