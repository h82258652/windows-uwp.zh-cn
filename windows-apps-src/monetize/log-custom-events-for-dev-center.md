---
author: mcleanbyron
Description: You can log custom events from your UWP app and review those events in the Usage report on the Windows Dev Center dashboard.
title: 记录开发人员中心的自定义事件
ms.author: mcleans
ms.date: 06/01/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Microsoft Store Services SDK, 日志事件
ms.assetid: 4aa591e0-c22a-4c90-b316-0b5d0410af19
ms.localizationpriority: medium
ms.openlocfilehash: 2b9cd4d7c527001bb382596c9c805be4ad5e7b08
ms.sourcegitcommit: 8e30651fd691378455ea1a57da10b2e4f50e66a0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/10/2018
ms.locfileid: "4508154"
---
# <a name="log-custom-events-for-dev-center"></a>记录开发人员中心的自定义事件

Windows 开发人员中心仪表板中的[使用情况](https://msdn.microsoft.com/windows/uwp/publish/usage-report)报告可使你获取有关在通用 Windows 平台 (UWP) 应用中已定义的自定义事件的信息。 自定义事件是表示应用中的某个事件或活动的任意字符串。 例如，游戏可能定义名为 *firstLevelPassed*、*secondLevelPassed* 等的自定义事件，用户在游戏中通过每个关卡时记录这些事件。

若要记录应用中的自定义事件，请将自定义事件字符串传递到 Microsoft Store Services SDK 提供的 [Log](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log) 方法。 可在开发人员中心仪表板中的[使用情况报告](https://msdn.microsoft.com/windows/uwp/publish/usage-report)的**自定义事件**部分中查看自定义事件的总发生次数。

> [!NOTE]
> 你记录到开发人员中心的自定义事件与 [Windows 事件](https://msdn.microsoft.com/library/windows/desktop/aa964766.aspx)无关，它们不会显示在**事件查看器**中。

## <a name="prerequisites"></a>先决条件

必须先在应用商店中发布应用，然后才能在仪表板中查看应用的**使用情况报告**中的自定义日志记录事件。

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
