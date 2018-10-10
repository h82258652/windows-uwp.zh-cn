---
title: 迁移仲裁程序
author: KevinAsgari
description: 了解如何在 Xbox Live 多人游戏 2015 中迁移仲裁程序
ms.assetid: 9fb5d2c0-d548-4a22-b64e-6b215f78d22e
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, 仲裁程序, 多人游戏 2015
ms.localizationpriority: medium
ms.openlocfilehash: 073f189d8571a93eb0d5b6ac4ec536e29e6a80e3
ms.sourcegitcommit: 8e30651fd691378455ea1a57da10b2e4f50e66a0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/10/2018
ms.locfileid: "4504950"
---
# <a name="migrating-an-arbiter"></a>迁移仲裁程序

在完整会话期间的某个时刻，你可能需要使用仲裁程序迁移来选择新的仲裁程序。 迁移有两类：

-   **正常的仲裁程序迁移**
-   **故障转移仲裁程序迁移**

以下流程图说明如何迁移仲裁程序。

![](../../images/multiplayer/Multiplayer_2015_HostMigration.png)

## <a name="graceful-arbiter-migration"></a>正常的仲裁程序迁移

在正常的仲裁程序迁移中，传出仲裁程序可以协助迁移任务并确定新仲裁程序。 此类迁移使用[操作方法：设置 MPSD 会话的仲裁程序](multiplayer-how-tos.md)所介绍的仲裁程序设置。


## <a name="failover-arbiter-migration"></a>故障转移仲裁程序迁移

在故障转移仲裁程序迁移中，与之前仲裁程序的连接丢失，其余对等方必须确定会话的新仲裁程序。 故障转移仲裁程序迁移还设置主机设备令牌，并处理 HTTP/412 状态代码，正如正常仲裁程序迁移所做的。 但是，在故障转移仲裁程序迁移期间，有多种选择新仲裁程序的方法。
## <a name="select-arbiter-using-the-host-candidate-list"></a>使用候选主机列表选择仲裁程序

你可以将 MPSD 配置为基于某些操作期间测量的匹配 QoS 指标提供有序的候选主机列表。 客户端可以使用此列表来确定新仲裁程序。 若要在仲裁程序迁移期间利用此列表，各对等方可以：

1.  确定之前仲裁程序的列表位置。
2.  评估列表中的下一个主机。
3.  如果主机是本地主机，将它用作新仲裁程序。
4.  如果主机不再出现在多人游戏会话中，或已从对等方断开连接，移到列表中的下一个候选项，并按照前面的步骤对它进行评估。
5.  如果已到达列表末尾但未选择新仲裁程序，请使用贪婪方法进行仲裁程序选择，这可能断开连接。 请参阅“使用贪婪仲裁程序选择”。

| 注意                                                                                                                                                                                                                                                                                    |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 不建议在通过显式游戏内 QoS 探测进行匹配后在游戏内创建候选主机列表。 如果绝对有必要使用此机制，请让你的客户端使用主机设备令牌而不是用户信息（例如，Xbox 用户 ID）来确定候选仲裁程序。 |


### <a name="select-arbiter-using-peer-voting"></a>使用对等方投票选择仲裁程序

如果在所有对等方之间存在完整连接，他们可以使用对等方消息进行投票，然后选择新的仲裁程序。 新仲裁程序然后使用同步更新更新会话的主机设备令牌。 请参阅[操作方法：更新多人游戏会话](multiplayer-how-tos.md)。


### <a name="use-greedy-arbiter-selection"></a>使用贪婪仲裁程序选择

有时，不提供候选主机列表或不需要连接 QoS，例如，对于纯仲裁程序责任。 在这些情况下，你的客户端可以使用贪婪仲裁程序选择方法。 在此情况下，对等方应在按照 **MultiplayerSessionChanged** 事件报告的情况，检测到原始仲裁程序已离开游戏会话后立即设置新仲裁程序。 所有其他对等方在尝试设置主机设备令牌时都将看到 HTTP/412 状态代码，假设此时没有对会话进行其他更改。 只有一个对等方会在选择新仲裁程序时成功。


## <a name="see-also"></a>另请参阅

[MPSD 会话详细信息](mpsd-session-details.md)
