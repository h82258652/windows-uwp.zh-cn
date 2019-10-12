---
title: WNS 通知优先级
description: 针对通知可以设置的各种优先级的说明
ms.date: 01/10/2017
ms.topic: article
keywords: windows 10，uwp，WinRT API，WNS
localizationpriority: medium
ms.openlocfilehash: 3310b34b2748bd684e46e04775c973680f8e03a9
ms.sourcegitcommit: 445320ff0ee7323d823194d4ec9cfa6e710ed85d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/11/2019
ms.locfileid: "72282242"
---
# <a name="wns-notification-priorities"></a>WNS 通知优先级
通过使用简单的标头将通知的优先级设置为 WNS POST 消息，你可以控制在使用电池的情况下通知的传送方式。

## <a name="power-on-windows"></a>Windows 开机
当更多用户仅在备有电池的设备上工作时，将电源使用降至最低会成为所有应用的标准要求。 如果应用消耗的能源超过其提供的值，用户可能会卸载这些应用。 虽然 Windows 操作系统尽可能降低电池的电源使用情况，但应用程序的工作职责是有效的。 

WNS 优先级是将非关键工作移出电池的一种方法。 WNS 优先级告知系统应立即传递哪些通知，哪些通知可以等待，直到设备插入电源。 对于这些提示，系统可以将通知准确地传递给用户和应用程序的最有价值的时间。 

## <a name="power-modes-on-the-device"></a>设备上的电源模式
每台 Windows 设备都在各种电源模式（电池、电池保护程序和充电）上运行，并且用户需要不同电源模式下的应用程序中的不同行为。 当设备打开时，应传递所有通知。 在节电模式下，只应传递最重要的通知。 设备接通电源时，可以完成同步或非时间关键操作。

Windows 不知道哪些通知对于任何用户或应用是重要的，因此系统完全依赖于应用来为其通知设置正确的优先级。 

## <a name="priorities"></a>因素
发送推送通知时，应用可使用四个优先级。 此优先级设置为单独的通知，允许你选择需要立即传递的通知（例如，IM 消息）以及哪些通知可以等待（例如，联系照片更新）。

优先级如下： 

|    Priority    |    用户替代    |    描述    |    示例    |
|----------------|---------------------|-------------------|---------------|
|    高    |    是–用户可以阻止来自应用的所有通知，也可以阻止应用在电池保护模式下受到限制。    |    当设备可以接收通知时，必须立即交付的最重要通知。 诸如 VoIP 调用或应唤醒设备的关键警报之类的问题都属于此类别。    |    VoIP 调用，时间严重警报    |
|    中等    |    是–用户可以阻止来自应用的所有通知，也可以阻止应用在电池保护模式下受到限制。    |    这些是不太重要的事情，不需要立即发生的事情，但如果用户未在后台运行，则会厌恶。    |    辅助电子邮件帐户同步，动态磁贴更新。    |
|    低    |    是–用户可以阻止来自应用的所有通知，也可以阻止应用在电池保护模式下受到限制。    |    仅当用户使用设备或后台活动有效时才有意义的通知。 在用户登录或插入到设备中之前，将缓存和不处理它们。    |    联系人状态（联机/脱机）    |
|    非常低     |    否–不能防止在电池保护模式下限制极低优先级的通知。    |    这与低优先级几乎相同，但用户不能重写电池保护策略。 这些通知决不会以节电形式提供。    |    正在同步同步服务的文件。    |

请注意，许多应用程序在其整个生命周期中都有不同的优先级通知。 由于优先级是根据每个通知进行设置的，因此不会出现问题。 VoIP 应用可为传入呼叫发送高优先级通知，并在联系人联机时使用低优先级的通知。 

## <a name="setting-the-priority"></a>设置优先级

设置通知请求的优先级是通过 POST 请求上的附加标头（`X-WNS-PRIORITY`）完成的。 这是一个介于1和4之间的整数值，映射到优先级： 

| 优先级名称 | X-WNS 优先级值 | 默认值： |
|---------------|----------------------|------------------|
| 高 | 1 | Toast |
| 中等 | 2 | 磁贴和徽章 |
| 低 | 3 | 原始 |
| 非常低 | 4 |  |

为了向后兼容，不需要设置优先级。 如果应用不设置其通知的优先级，系统将提供默认优先级。 默认值显示在上图中，并与现有 Windows 版本的行为匹配。 

## <a name="detailed-listing-of-desktop-behavior"></a>桌面行为的详细列表 

如果要在多个不同的 Windows Sku 之间交付您的应用程序，通常最好按照上一部分中的图表操作。 

下面列出了针对每个优先级的更具体的推荐行为。 这并不能保证每个设备都能准确地根据图表工作。 Oem 可以随意配置行为，但大多数情况下都接近此图表。 

| 设备状态    | 大事高    |    大事中等        | 大事低    |    大事非常低    |
|-------------------------------------------------------|----------------------------------------------------|----------------------------------------------------|----------------------------------------------------|--------------------------|
|    屏幕打开或接通电源    |    提供    |    提供    |    提供    |    提供    |
|    屏幕关闭和使用电池    |    提供    |    如果用户被免除：发送 Else： batch     |    如果用户被免除：发送 Else： cache *    |    缓存    |
|    已启用节电    |    如果用户免除：发送 Else： cache    |    如果用户免除：发送 Else： cache    |    如果用户免除：发送 Else： cache    |    缓存     |
|    启用电池 + 启用节电功能 + 屏幕关闭    |    如果用户免除：发送 Else： cache    |    如果用户免除：发送 Else： cache    |    如果用户免除：发送 Else： cache    |    缓存    |

请注意，默认情况下，对于基于 Windows Phone 的设备，将为 "仅屏幕关闭" 和 "仅限电池" 提供低优先级通知。 这是为了与预先存在的 MPNS 策略 maintian 兼容。 另请注意，第四行和第五行相同，只是调用不同的方案。

若要在电池保护中免除应用，用户必须在 "设置" 中按 "应用的电池使用情况"，并选择 "允许应用运行后台任务"。 此用户选择将豁免中的应用程序，以获取高、中和低优先级通知。 你还可以调用[BACKGROUNDEXECUTIONMANAGER API](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccesskindasync#Windows_ApplicationModel_Background_BackgroundExecutionManager_RequestAccessKindAsync_Windows_ApplicationModel_Background_BackgroundAccessRequestKind_System_String_)以编程方式请求用户的权限。  

## <a name="related-topics"></a>相关主题
- [Windows 推送通知服务 (WNS) 概述](windows-push-notification-services--wns--overview.md)
- [正在请求权限在后台运行](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccesskindasync#Windows_ApplicationModel_Background_BackgroundExecutionManager_RequestAccessKindAsync_Windows_ApplicationModel_Background_BackgroundAccessRequestKind_System_String_)
