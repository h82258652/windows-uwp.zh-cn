---
title: WNS 通知优先级
description: 可以在通知设置各种优先级的说明
ms.date: 01/10/2017
ms.topic: article
keywords: windows 10，uwp，WinRT API WNS
localizationpriority: medium
ms.openlocfilehash: 2c297a04786c6fbf1eb0600e63a04a6d88585864
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57648702"
---
# <a name="wns-notification-priorities"></a>WNS 通知优先级
通过将通知的优先级并使用简单的标头设置为 WNS 发布消息，可以控制如何在电池敏感的情况下传递通知。

## <a name="power-on-windows"></a>在 Windows 上的电源
仅在由电池供电设备上处理更多的用户，最大程度减少电源使用情况已成为一项标准要求的所有应用。 如果应用程序消耗比它们提供的值的更多能源，用户可能会卸载应用。 尽管 Windows 操作系统降低了电池上的电源使用情况，在可能的情况，负责应用的高效地工作。 

WNS 优先级是一种方法将移动非关键工作电池。 WNS 优先级告知的系统应立即传递的通知和其中可以等待，直到该设备插入到电源插座。 通过这些提示，系统可以提供的确切时间它们会对用户和应用程序最有价值的通知。 

## <a name="power-modes-on-the-device"></a>在设备上的电源模式
通过各种电源模式 （电池、 电池保护程序和费用），每台 Windows 设备进行操作，用户希望应用程序的不同行为，在不同的电源模式下。 在设备上时，应传递的所有通知。 在省电模式，应传递仅最重要的通知。 虽然在插入设备，可以完成同步或非时间关键操作。

Windows 不知道的通知非常重要的任何用户或应用程序中，因此系统完全依赖于应用程序将其通知的正确优先级设置。 

## <a name="priorities"></a>优先级
有可用于应用程序发送推送通知时要使用的四个优先级别。 将优先级设置单独的通知，让你可以选择需要立即传递的通知 （例如，IM 消息） 和哪些可以等待 （例如，可联系照片更新）。

优先级包括： 

|    Priority    |    用户替代    |    描述    |    示例    |
|----------------|---------------------|-------------------|---------------|
|    高    |    是 – 用户可以阻止从应用程序的所有通知，或者可以阻止应用在省电模式中受到限制。    |    必须传递立即在任何情况下设备可能会收到通知时最重要通知。 诸如 VoIP 呼叫或应唤醒设备的关键警报属于此类别。    |    VoIP 呼叫，时间-严重警报    |
|    中等    |    是 – 用户可以阻止从应用程序的所有通知，或者可以阻止应用在省电模式中受到限制。    |    这些是不视为重要的是，无需立即，发生的事情的内容，但用户会很苦恼，如果它们不在后台中运行。    |    动态磁贴更新辅助电子邮件帐户同步。    |
|    低    |    是 – 用户可以阻止从应用程序的所有通知，或者可以阻止应用在省电模式中受到限制。    |    仅当用户正在使用该设备或后台活动有意义时有意义的通知。 这些是缓存，直到用户登录或在其设备的插入不处理。    |    联系人的状态 （联机/脱机）    |
|    非常低     |    否-它不能阻止非常低优先级服务通知省电模式中受到限制。    |    这是几乎相同，因为除用户以外的低优先级不能重写电池保护程序策略中。 节电模式将永远不会提供这些通知。    |    对于同步服务的同步文件。    |

请注意，许多应用程序将具有在其整个生命周期的不同优先级的通知。 由于在每个通知的基础上设置优先级，这不是问题。 VoIP 应用可以发送的传入呼叫的高优先级通知，然后按照它使用低优先级一个联系人联机时。 

## <a name="setting-the-priority"></a>设置优先级

通知请求上设置的优先级是通过 POST 请求的其他标头`X-WNS-PRIORITY`。 这是一个整数值介于 0 到 3 之间映射到一个优先级： 

| 优先级名称 | X WNS 优先级值 | 默认值： |
|---------------|----------------------|------------------|
| 高 | 1 | Toast |
| Meduim | 2 | 磁贴和徽章 |
| 低 | 3 | 原始 |
| 非常低 | 4 |  |

要向后兼容，设置优先级不需要。 如果应用不会将其通知的优先级设置，系统将提供默认优先级。 在上方图表中显示和匹配的现有版本的 Windows 行为的默认值。 

## <a name="detailed-listing-of-desktop-behavior"></a>桌面的行为的详细的列表 

如果跨 Windows 许多不同的 Sku 发运你的应用，则通常最好遵循上面的部分中的图表。 

下面列出了每个优先级更具体的建议的行为。 这不是每个设备会完全根据图表的保障。 Oem 可以自由配置的行为方式不同，但大多数接近此图表。 

| 设备状态    | 优先级：高    |    优先级：中等        | 优先级：低    |    优先级：非常低    |
|-------------------------------------------------------|----------------------------------------------------|----------------------------------------------------|----------------------------------------------------|--------------------------|
|    屏幕上或接通电源    |    交付    |    交付    |    交付    |    交付    |
|    屏幕关闭和电池    |    交付    |    如果被免除的用户： 提供 Else： 批处理     |    如果被免除的用户： 提供 Else： 缓存 *    |    缓存    |
|    节电模式已启用    |    如果被免除的用户： 提供 Else： 缓存    |    如果被免除的用户： 提供 Else： 缓存    |    如果被免除的用户： 提供 Else： 缓存    |    缓存     |
|    电池 + 电池保护程序上启用 + 屏幕    |    如果被免除的用户： 提供 Else： 缓存    |    如果被免除的用户： 提供 Else： 缓存    |    如果被免除的用户： 提供 Else： 缓存    |    缓存    |

请注意，默认情况下，屏幕关闭传递低优先级服务通知和电池仅对 Windows Phone 的基于设备。 这是为了维护与预先存在的 MPNS 策略的兼容性。 另请注意，第四个和第五个行是相同，只需调用不同的方案。

若要使电池保护程序中的应用，用户必须转到"电池使用情况通过中的应用程序"设置并选择"允许应用程序运行后台任务。" 此用户选择免除对高、 中和低优先级服务通知的电池保护程序中的应用程序。 您还可以调用[BackgroundExecutionManager API](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccesskindasync#Windows_ApplicationModel_Background_BackgroundExecutionManager_RequestAccessKindAsync_Windows_ApplicationModel_Background_BackgroundAccessRequestKind_System_String_)若要以编程方式要求的用户的权限。  

## <a name="related-topics"></a>相关主题
- [Windows 推送通知服务 (WNS) 概述](windows-push-notification-services--wns--overview.md)
- [要求在后台运行的权限](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccesskindasync#Windows_ApplicationModel_Background_BackgroundExecutionManager_RequestAccessKindAsync_Windows_ApplicationModel_Background_BackgroundAccessRequestKind_System_String_)
- 
