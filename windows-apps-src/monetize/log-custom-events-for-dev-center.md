---
author: mcleanbyron
Description: "可记录 UWP 应用中的自定义事件，并查看 Windows 开发人员中心仪表板上的使用情况报告中的事件。"
title: "记录开发人员中心的自定义事件"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, Microsoft Store Services SDK, 日志事件"
ms.assetid: 4aa591e0-c22a-4c90-b316-0b5d0410af19
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 80cc3ec6aab90549c55ff8c8f78b54f5827f61ff
ms.lasthandoff: 02/08/2017

---

# <a name="log-custom-events-for-dev-center"></a>记录开发人员中心的自定义事件

Windows 开发人员中心仪表板中的[使用情况](https://msdn.microsoft.com/windows/uwp/publish/usage-report)报告可使你获取有关在通用 Windows 平台 (UWP) 应用中已定义的自定义事件的信息。 自定义事件是表示应用中的某个事件或活动的任意字符串。 例如，游戏可能定义名为 *firstLevelPassed*、*secondLevelPassed* 等的自定义事件，用户在游戏中通过每个关卡时记录这些事件。

若要记录应用中的自定义事件，请将自定义事件字符串传递到 Microsoft Store Services SDK 提供的 [Log](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.log.aspx) 方法。 可在开发人员中心仪表板中的[使用情况报告](https://msdn.microsoft.com/windows/uwp/publish/usage-report)的**自定义事件**部分中查看自定义事件的总发生次数。

>**注意**&nbsp;&nbsp;你记录到开发人员中心的自定义事件与 [Windows 事件](https://msdn.microsoft.com/library/windows/desktop/aa964766.aspx)无关，它们不会显示在**事件查看器**中。

## <a name="prerequisites"></a>先决条件

必须先在应用商店中发布应用，然后才能在仪表板中查看应用的**使用情况报告**中的自定义日志记录事件。

## <a name="how-to-log-custom-events"></a>如何记录自定义事件

1. 如果先前尚未这样做，请在开发计算机上[安装 Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk)。 除了用于记录自定义事件的 API 外，此 SDK 还提供用于其他功能的 API，例如通过 A/B 测试运行实验和显示广告。
2. 在 Visual Studio 中打开你的项目。
3. 在“解决方案资源管理器”中，右键单击你的项目的**引用**节点，然后单击**添加引用**。
4. 在**引用管理器**中，展开**通用 Windows** 并单击**扩展**。
5. 在 SDK 列表中，单击 **Microsoft 协议框架**旁边的复选框，然后单击 **确定**。
7. 将以下语句添加到要记录自定义事件的每个代码文件的顶部。

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[EventLogger](./code/StoreSDKSamples/cs/LogEvents.cs#EngagementNamespace)]

8. 在要记录自定义事件的每个代码部分中，获取 [StoreServicesCustomEventLogger](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.log.aspx) 对象，然后调用 [Log](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.log.aspx) 方法。 将自定义事件字符串传递到该方法。

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[EventLogger](./code/StoreSDKSamples/cs/LogEvents.cs#Log)]

## <a name="related-topics"></a>相关主题

* [使用情况报告](https://msdn.microsoft.com/windows/uwp/publish/usage-report)
* [记录方法](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.log.aspx)
* [Microsoft Store Services SDK](https://msdn.microsoft.com/windows/uwp/monetize/microsoft-store-services-sdk)

