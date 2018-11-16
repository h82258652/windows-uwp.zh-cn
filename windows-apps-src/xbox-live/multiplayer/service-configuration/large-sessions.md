---
title: 大型会话
author: KevinAsgari
description: 了解如何将大型会话用于 Xbox Live 多人游戏平台。
ms.author: kevinasg
ms.date: 07/11/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, 多人游戏, 大型会话, 最近的玩家
ms.localizationpriority: medium
ms.openlocfilehash: 52b65f0e6c4ee8642b49ff71533961f50761be98
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2018
ms.locfileid: "6843183"
---
# <a name="large-sessions"></a>大型会话

如果你需要可处理 100 多个成员的多人游戏会话，则将需要使用所谓的大型会话。 此方案对于大规模多人在线 (MMO) 游戏和广播（大多数成员是旁观者）最为常见，但是还有可能适用于其他风格的游戏。

在某些情况下，你可能也希望使用大型会话，即使涉及一小群玩家。 如果你希望多个玩家加入同一个会话，而他们不一定相互认识（如果他们在游戏中不会相遇），则可以使用大型会话的“encounters”属性。

大型会话目前不受 [Xbox 集成多人游戏 (XIM)](../xbox-integrated-multiplayer.md) 或[多人游戏管理器 (MPM)](../multiplayer-manager.md) 的支持，因此，你必须使用 Multiplayer 2015 API 直接调用多人游戏服务目录 (MPSD)。

大型会话与常规会话略有不同：

* 与常规会话相比，包含的信息较少，但效率较高。
* 最多支持 10,000 名成员。
* 你无法订阅大型会话。
* 大型会话的成员不会自动包含到最近的玩家列表中。

## <a name="recent-players"></a>最近的玩家

Xbox Live 的一项功能是，如果 Xbox Live 玩家与新人玩多人游戏，则在游戏启动后，他们可在其仪表板上的**最近的玩家**列表中看到这些玩家。 如果某玩家在游戏中与新玩家一起玩游戏时获得出色体验，则可能希望再与他们一起玩游戏，或将他们添加为好友。 如果他们与某玩家玩游戏时获得糟糕的体验，则他们可能希望以后不要再遇到该玩家，并且/或者在游戏结束后报告其不好的行为。

对于常规会话，Xbox Live 会自动将游戏会话中的玩家添加到最近的玩家列表中。 但是，如果你使用大型会话，则必须采取一些额外的措施，以确保正确填充最近的玩家列表。

## <a name="set-up-a-large-session"></a>设置一个大型会话

若要将会话设置为大型会话，请将 `“large”: true` 添加到会话模板中的功能部分。 这样，你就可以将 `maxMembersCount` 设置为最大值 10,000。 如下会话模板的工作原理应为：

```json
{
    "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 2000,
            "visibility": "open",
            "capabilities": {
                "gameplay": true,
                "large": true
            },
            "timeouts": {
                "inactive": 0,
                "ready": 180000,
                "sessionEmpty": 0
            }
        },
        "custom": { }
    }
}
```

## <a name="working-with-large-sessions"></a>使用大型会话

在将大型会话写入到 MPSD 时，我们建议你每秒最多写入 10 次。 通常，这是包含 1000 玩家的会话，每个玩家平均每 2 分钟写入一次（例如加入/离开）。

其他属性不能保留在大型会话中。

### <a name="associating-players-from-the-same-large-session"></a>关联同一个大型会话中的玩家

如果从 MPSD 中检索大型会话，则响应时不返回成员列表。事实上，无法获取完整列表。 相反，如果调用方处于会话中，则其成员记录只能位于“成员”集合中，并且标记为“我”（就像在请求中一样）。

这意味着，客户端成员只能在会话中更新其自己的条目，并将依赖于服务器为他们提供 Xbox Live 可用于关联一起玩游戏的玩家的常见标识符。

你可以采用两种方法，指示会话中的玩家是一起玩游戏的（用于更新信誉和最近的玩家状态）。

#### <a name="1-persistent-groups"></a>1. 永久组

如果一组玩家一直在一起（可能包括有来有往的玩家），则可以命名该组（例如，guid – 采用与常规会话相同的命名规则）。每个成员在加入和离开该组时，应在其自己的“groups”属性中添加或删除该组名，这是一组字符串：

```json
{
    "members": {
        "me": {
            "properties": {
                "system": {
                    "groups": [ "boffins-posse" ]
                }
            }
        }
    }
}
```

#### <a name="2-brief-encounters"></a>2. 短暂遇到

如果两人短暂遇到过一次，则游戏可以改用“encounters”数组。 为每次遇到提供名称，在遇到后，两个（或所有）参与者都要将该名称写入其自己的“encounters”属性：

```json
{
    "members": {
        "me": {
            "properties": {
                "system": {
                    "encounters": [ "trade.0c7bbbbf-1e49-40a1-a354-0a9a9e23d26a" ]
                }
            }
        }
    }
}
```

你可以对“groups”和“encounters”使用相同的名称。例如，如果一个玩家与某个组“进行交易”，则该组中的成员不需要执行任何操作（假定他们之前将该组名添加到其“groups”中），并且所遇到的人会将该组名上传到其“encounters”列表中。 这样，各用户便可像最近的玩家一样查看该组的所有成员，反之亦然。

成为该组成员 30 秒钟时的遇到计数。 由于遇到被视为一次性活动，因此，系统总会立即处理“encounters”数组，然后从会话中将其清除。该数字将不会出现在响应中。（“groups”数组始终存在，直到被更改或删除，或者成员离开会话。）
