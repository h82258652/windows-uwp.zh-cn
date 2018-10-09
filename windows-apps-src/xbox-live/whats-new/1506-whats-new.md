---
title: Xbox Live SDK 的新增功能 - 2015 年 6 月
author: KevinAsgari
description: Xbox Live SDK 的新增功能 - 2015 年 6 月
ms.assetid: 354bcd47-2564-4dd5-89e3-242bca462b35
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 29e31363a1afb0d01d24112a4fa8dd69f4f87bff
ms.sourcegitcommit: 49aab071aa2bd88f1c165438ee7e5c854b3e4f61
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2018
ms.locfileid: "4464231"
---
# <a name="whats-new-for-the-xbox-live-sdk---june-2015"></a>Xbox Live SDK 的新增功能 - 2015 年 6 月

Xbox Live SDK 的 6 月版本包括下列更新：

## <a name="os-and-tool-support"></a>操作系统和工具支持 ##
Xbox Live SDK 现在支持最新的 Windows 10 和 Visual Studio 2015 RC 预览体验成员版本。

## <a name="title-callable-ui-apis"></a>游戏可调用 UI API

| 注意 |
|------|
| 此部分仅适用于 UWP 游戏，XDK 游戏应该参考 TCUI 的 Windows.Xbox.UI.SystemUI 类  |

Xbox Live SDK 现在包含包装器 API，它支持游戏可调用 UI (TCUI) 在 Windows 10 电脑/移动设备上显示股价图 UI。

这些 API 仅面向 UWP 应用提供。

你可以从 TitleCallableUI (WinRT) 和 title_callable_ui (C++) 类调用 TCUI 包装器。

股票图 UI 包括：
* 玩家选取器 UI
* 游戏邀请选取器 UI
* 玩家个人资料卡 UI
* 添加/删除好友 UI
* 显示游戏成就 UI

请参见新 *TCUI* 示例，查看新 API 的使用示例。 你可以在 {*SDK source root*}\Samples\TCUI 下找到此示例。

## <a name="new-authentication-model-for-uwp-apps"></a>UWP 应用的新身份验证模型
Xbox Live SDK 现在支持使用 Microsoft 帐户 (MSA) 来识别 Windows 10 电脑/移动版设备上的 Xbox Live 玩家。

你现在可以通过使用 XboxLiveUser (WinRT) 和 xbox_live_user (C++) 类来登录用户。

## <a name="new-api-for-writing-events-in-uwp-apps"></a>用于在 UWP 应用中编写事件的新 API

| 注意 |
|------|
| 本部分仅适用于 UWP 游戏。  XDK 开发人员应参阅[游戏事件](https://developer.microsoft.com/en-us/games/xbox/docs/xboxlive/xbox-live-partners/event-driven-data-platform/game-events)一文了解 XDK 特定信息  |

新 EventsService (WinRT) 和 events_service (C++) 类可让你编写可以更新用户统计信息、成就、排行榜等游戏内事件。这些新类仅适用于 UWP 应用。

## <a name="breaking-change-to-event-handlers"></a>事件处理程序的重大更改 ##
C++ SDK 中的所有事件处理程序已从单个 `void set_*_handler()` 方法更改为一对 `function_context add_*_handler()` 和 `void remove_*_handler(function_context context)` 方法。

每个 `add_*_handler()` 方法现在返回一个 `function_context` 对象。 在删除事件处理程序时，必须传入 `function_context` 对象。

例如：
```
function_context subscriptionLostContext = xbox_live_context()->multiplayer_service().add_multiplayer_subscription_lost_handler(...);

localUser->xbox_live_context()->multiplayer_service().remove_multiplayer_subscription_lost_handler(subscriptionLostContext);
```

## <a name="new-apis-for-managing-multiplayer-scenarios"></a>用于管理多人游戏场景的新 API
Xbox Live SDK 现在包含一组 C++ API，用于管理常见的多人游戏场景。 API 包含在 xbox.services.experimental.multiplayer 命名空间内。

这些 API 位于实验命名空间内，这意味着它们可能会根据反馈更改。  我们鼓励开发人员使用这些 API 并提供反馈。

请参见新 *MultiplayerManager* 示例，查看这些新 API 的使用示例。 你可以在 {*SDK source root*}\Samples\MultiplayerManager 下找到此示例。
