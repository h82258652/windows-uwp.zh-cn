---
title: 多人游戏会话模板
author: KevinAsgari
description: 了解 Xbox Live 多人游戏会话模板。
ms.assetid: 178c9863-0fce-4e6a-9147-a928110b53a2
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, 多人游戏, 会话模板
ms.localizationpriority: medium
ms.openlocfilehash: 0296aaa21d9e3d873b7edff3026ea22f9b5b2b67
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2018
ms.locfileid: "5740892"
---
# <a name="multiplayer-session-templates"></a>多人游戏会话模板

本主题简要概述了多人游戏会话模板，并提供了可复制和修改的多人游戏会话模板的几个示例。

多人游戏会话模板是用于创建多人游戏会话的蓝图。 所有会话都必须根据预定义的模板创建。 模板可定义对通过模板创建的任何会话都相同的常量。 在通过模板创建会话后，游戏可以向会话中添加并修改其他数据，但不能修改已在模板中定义的常量。

 有关详细信息，请参阅[会话概述](../multiplayer-appendix/mpsd-session-details.md)。

适用于服务配置标识符 (SCID) 的会话模板列表以及特定会话模板的内容可从多人游戏会话目录 (MPSD) 中检索。


## <a name="about-session-templates"></a>关于会话模板

会话模板使用与 HTTP PUT 请求相同的格式创建或修改会话。 不同之处在于模板仅限于常量（无成员、服务器或属性）。 常量可以是任何会话常量，包括自定义部分和各种系统常量。

### <a name="session-template-versions"></a>会话模板版本

本主题中定义的会话模板使用模板合约版本 107 构建。 使用这些模板创建新模板时，请确保输入 107 作为合约版本。

如果你使用 XSAPI，并监视调试程序中生成的请求，则可能会注意到，这些请求使用模板合约版本 105。 在运行时，MPSD 会将这些请求有效地“升级”到版本 107。

> **注意：** 允许在请求中使用用于会话模板的不同合约版本。

如有必要，你可以将会话模板从版本 104/105 更改为版本 107。 有关说明，请参阅[调整适用于 2015 Multiplayer 的作品时遇见的常见问题](../multiplayer-appendix/common-issues-when-adapting-multiplayer.md)中的迁移说明。


## <a name="session-template-default-values"></a>会话模板默认值

通过会话模板创建的每个会话都以模板副本的形式启动。 模板中不包含的值可在创建会话时提供。 如果未设置其他值，则在某些情况下将提供默认值。 例如，对于合约版本 107，默认的超时值集为：

```json
    {
      "constants": {
        "system": {
          "reservedRemovalTimeout": 30000,
          "inactiveRemovalTimeout": 0,
          "readyRemovalTimeout": 180000,
          "sessionEmptyTimeout": 0
        }
      }
    }
```
你可以通过指定 null 使值保持未设置状态。 这将替代任何默认设置，并阻止在创建会话时设置值。 例如，若要删除会话空超时值，从而允许会话无限期继续，则即使没有任何成员，也将以下项添加到会话模板中：
```json
    {
      "constants": {
        "system": {
          "sessionEmptyTimeout": null
        }
      }
    }
```
> **重要提示：** 通过模板设置的常量不能通过写入到 MPSD 更改。 若要更改这些值，你必须创建并提交包含所需更改的新模板。


## <a name="example-session-templates"></a>示例会话模板
此部分说明适用于不同用途和网络拓扑的会话模板的几个示例。 请先检查每个模板，然后选择最适合你的客户端的模板。 此后，你可以复制模板，并将其粘贴到你的服务配置中，此操作可能在执行必要的更改后执行。

### <a name="standard-lobby-session"></a>标准大厅会话
你可以将以下模板用作初始模板，以为你的游戏创建大厅会话：

* 将 `maxMembersCount` 值更改为你希望在大厅会话中支持的最大玩家数。  
* 如果你的作品不支持来自不同平台（如 Xbox One 和电脑）的玩家一起玩游戏，则可以删除 `crossPlay` 元素。  
* 你也可以更改其他值，但是，如果你不确定自己所需的内容，则最好先从以下值入手。


```json
{
   "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 8,
            "visibility": "open",
            "capabilities": {
                "connectivity": true,
                "connectionRequiredForActiveMembers": true,
                "crossPlay": true,
                "userAuthorizationStyle": true
            },
        },
        "custom": {}
    }
}
```

### <a name="standard-game-session-without-matchmaking"></a>不含匹配的标准游戏会话
如果你的游戏不包括匿名匹配，也不需要 100 多个成员，则可以将以下模板用作起始模板，以为你的游戏创建游戏会话。

请注意，下面仅提供在标准大厅会话模板中指定的新值：
* `constants.system.inviteProtocol : "game"`
* `constants.system.capabilities.gameplay : true`

```json
{
   "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 8,
            "visibility": "open",
            "inviteProtocol": "game",
            "capabilities": {
                "connectivity": true,
                "connectionRequiredForActiveMembers": true,
                "gameplay" : true,
                "crossPlay": true,
                "userAuthorizationStyle": true
            }
        },
        "custom": {}
    }
}
```

### <a name="add-matchmaking-to-a-game-session-template-while-letting-the-multiplayer-service-handle-quality-of-service-checks"></a>将匹配添加到游戏会话模板中，同时让多人游戏服务处理服务质量检查。

你可以向游戏玩法模板中添加以下 `memberInitialization` json 元素，以添加对匹配的支持。

创建 SmartMatch hopper 时，可以将此模板用作 hopper 的目标会话模板。

```json
{
   "constants": {
        "system": {
            "memberInitialization": {
               "joinTimeout": 20000,
               "measurementTimeout": 15000,
               "membersNeededToStart": 2
            }
        }
    }
}
```

### <a name="add-matchmaking-to-a-game-session-template-where-quality-of-service-checks-are-handled-by-a-title-managed-data-center"></a>将匹配添加到游戏会话模板中，其中，服务质量检查由托管作品的数据中心处理。



```json
{
   "constants": {
        "system": {
            "peerToHostRequirements": {  
                "latencyMaximum": 250,
                "bandwidthDownMinimum": 256,
                "bandwidthUpMinimum": 256,
                "hostSelectionMetric": "latency"
            },
            "memberInitialization": {
               "joinTimeout": 15000,
               "measurementTimeout": 15000,
               "membersNeededToStart": 2
            }
        },
        "custom": {}
    }
}
```

### <a name="basic-session-template-for-client-server-game-session"></a>适用于客户端-服务器游戏会话的基本会话模板

你可以对以下作品使用此模板：不执行对等通信，也不使用 Xbox Live Compute，而让客户端连接到第三方托管的服务器。
```json
    {
      "constants": {
        "system": {
          "version": 1,
          "maxMembersCount": 12,
          "visibility": "open",
          "inviteProtocol": "game",
          "capabilities": {
            "connectionRequiredForActiveMembers": true,
            "gameplay" : true,
          },
        },
        "custom": {}
      }
    }
```

### <a name="lobbysmartmatch-ticket-session-template-for-peer-based-networking"></a>适用于对等网络的大厅/SmartMatch 票证会话模板

使用此模板可创建大厅会话或 SmartMatch 票证会话，该会话仅用于将一组玩家发送到匹配中。 该模板不能用于配置游戏会话。 该模板专供使用对等或对等-主机网络拓扑的客户端使用。
```json
    {
      "constants": {
        "system": {
          "version": 1,
          "maxMembersCount": 10,
          "visibility": "open",
          "capabilities": {
            "connectionRequiredForActiveMembers": true,
          },
          "memberInitialization": {
            "membersNeededToStart": 1
          },
        },
        "custom": {}
      }
    }
```

### <a name="quality-of-service-qos-templates"></a>服务质量 (QoS) 模板

如果你的客户端要使用匹配和评估 QoS，则必须将某些常量添加到会话模板，以通知 MPSD 与客户端进行协调，从而管理加入会话的用户。 在通知用户游戏准备启动前，这种协调可验证连接状态的质量。 如果是客户端-服务器游戏，则在一组玩家进入匹配前，该协调会验证连接质量。

#### <a name="peer-to-host-game-session-template-with-qos"></a>使用 QoS 的对等-主机游戏会话模板

以下示例提供了使用 QoS 的对等-主机游戏会话模板。
```json
    {
      "constants": {
        "system": {
          "version": 1,
          "maxMembersCount": 12,
          "visibility": "open",
          "inviteProtocol": "game",
          "capabilities": {
            "connectivity": true,
            "connectionRequiredForActiveMembers": true,
            "gameplay" : true
          },
          "memberInitialization": {
            "membersNeededToStart": 2
          },
          "peerToHostRequirements": {
            "latencyMaximum": 350,
            "bandwidthDownMinimum": 1000,
            "bandwidthUpMinimum": 1000,
            "hostSelectionMetric": "latency"
          }
        },
        "custom": { }
      }
    }
```

#### <a name="peer-to-peer-game-session-template-with-qos"></a>使用 QoS 的对等游戏会话模板

下面是使用 QoS 的对等游戏会话模板的示例。
```json
    {
    "constants": {
      "system": {
        "version": 1,
        "maxMembersCount": 12,
        "visibility": "open",
        "inviteProtocol": "game",
        "capabilities": {
          "connectivity": true,
          "connectionRequiredForActiveMembers": true,
          "gameplay" : true
        },
        "memberInitialization": {
          "membersNeededToStart": 2
        },
        "peerToPeerRequirements": {
          "latencyMaximum": 250,
          "bandwidthMinimum": 10000
        }
      },
      "custom": { }
     }
    }
```

#### <a name="client-server-xbox-live-compute-lobbymatchmaking-session-template-with-qos"></a>使用 QoS 的客户端-服务器 (Xbox Live Compute) 大厅/匹配会话模板

使用此模板可通过 QoS 创建大厅会话或匹配会话。 此模板不能用于配置游戏会话。
```json
    {
      "constants": {
        "system": {
          "version": 1,
          "maxMembersCount": 12,
          "visibility": "open",
          "memberInitialization": {
            "membersNeededToStart": 1
          }
        },
        "custom": {}
      }
    }
```

#### <a name="session-template-for-crossplay-between-xbox-one-and-windows-10"></a>适用于 Xbox One 与 Windows 10 之间的跨平台联机游戏的会话模板

使用此模板可在 Xbox One 与 Windows 10 之间实现跨平台多人游戏。 userAuthorizationStyle 功能支持访问 Windows 10。 可选的 crossPlay 功能表明，你的作品支持各种会话，例如跨平台的邀请和加入正在进行的会话。
```json
    {
      "constants": {
        "system": {
          "capabilities": {
            "crossPlay": true,
            "userAuthorizationStyle": true
          },
        },
        "custom": {}
      }
    }
```
