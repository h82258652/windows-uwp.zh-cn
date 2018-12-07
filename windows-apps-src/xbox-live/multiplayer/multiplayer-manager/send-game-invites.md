---
title: 发送游戏邀请
description: 了解如何使用 Xbox Live 多人游戏管理器让玩家发送游戏邀请。
ms.assetid: 8b9a98af-fb78-431b-9a2a-876168e2fd76
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, 多人游戏, 多人游戏管理器, 流程图, 游戏邀请
ms.localizationpriority: medium
ms.openlocfilehash: 85aa45558d1443638ba7dd50dbea8923125ef664
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/06/2018
ms.locfileid: "8747308"
---
# <a name="send-game-invites"></a>发送游戏邀请

更为简单的多人游戏方案之一是允许玩家在线与好友玩游戏。 该方案介绍了向其他玩家发送邀请，使其加入你的游戏所需的步骤。

[初始化多人游戏管理器](play-multiplayer-with-friends.md)，并[通过添加本地用户创建大厅会话](play-multiplayer-with-friends.md)后，你必须在收到 `user_added` 事件后才可以开始发送邀请。

有两种发送邀请的方法：

1. [Xbox 平台邀请 TCUI](#xbox-platform-invite-tcui)
2. [游戏实现的自定义 UI](#title-implemented-custom-ui)

你可以在此处查看过程流程图：[流程图 - 向其他玩家发送邀请](mpm-flowcharts/mpm-send-invites.md)。

### <a name="1-xbox-platform-invite-tcui-a-namexbox-platform-invite-tcui"></a>1) Xbox 平台邀请 TCUI <a name="xbox-platform-invite-tcui">

| 方法 | 触发的事件 |
| -----|----------------|
| `multiplayer_lobby_session::invite_friends()` | `invite_sent` |

调用 `invite_friends()` 将引入用于邀请好友的标准 Xbox UI。 这会显示允许玩家选择好友或最近的玩家以进行游戏邀请的 UI。 玩家点击确认后，多人游戏管理器会向选择的玩家发送邀请。

**示例：**

```cpp
auto result = mpInstance->lobby_session()->invite_friends(xboxLiveContext);
if (result.err())
{
  // handle error
}
```

**多人游戏管理器执行的功能**

* 引入 Xbox 库存游戏可调用 UI (TCUI)
* 直接向选择的玩家发送邀请

### <a name="2-title-implemented-custom-uia-nametitle-implemented-custom-ui"></a>2) 游戏实现的自定义 UI<a name="title-implemented-custom-ui">

| 方法 | 触发的事件 |
|-----|----------------|
| `multiplayer_lobby_session::invite_users()` | `invite_sent` |

你的游戏可实现查看在线好友并邀请他们的自定义 TCUI。 游戏可使用 `invite_users()` 方法向一组用户发送邀请，这组用户由其 Xbox Live 用户 Id 定义。 如果你更想要使用自己的游戏内 UI，而不是库存 Xbox UI，该方法十分有用。

**示例：**

```cpp
std::vector<string_t>& xboxUserIds;
xboxUserIds.push_back();  // Add xbox_user_ids from your in-game roster list

auto result = mpInstance->lobby_session()->invite_users(xboxUserIds);
if (result.err())
{
  // handle error
}
```

**多人游戏管理器执行的功能**

* 直接向选择的玩家发送邀请
