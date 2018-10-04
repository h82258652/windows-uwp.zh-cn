---
title: 完整状态
author: KevinAsgari
description: 了解 Xbox Live“完整状态”如何帮助提升你的游戏。
ms.assetid: 00042359-f877-4b26-9067-58834590b1dd
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, 完整状态
ms.localizationpriority: medium
ms.openlocfilehash: 57ee9c43b4a78fc68f6da29721d38ac11c440624
ms.sourcegitcommit: 5c9a47b135c5f587214675e39c1ac058c0380f4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/04/2018
ms.locfileid: "4355380"
---
# <a name="rich-presence"></a>完整状态

通过使用“完整状态”，你的游戏可以播发玩家正在做什么。 例如，你的游戏可以使用“完整状态”字符串来向所有玩家显示你游戏的玩家的状态，如*离开*。 “完整状态”信息向连接到 Xbox Live 的玩家显示。 理想情况下，“完整状态”字符串告诉其他 Xbox Live 玩家某个玩家正在做什么，以及他在你游戏中的位置。 Xbox One 上的“完整状态”字符串概念与 Xbox 360 相同，但新实现遵照“娱乐即服务”计划。 此部分中的主题介绍如何配置“完整状态”字符串，以及如何为玩你游戏的用户设置此字符串。


## <a name="definitions"></a>定义

**枚举**  
枚举是一些游戏内维度的列表。 这些游戏内维度的示例包括武器、角色分类、地图等。 我们希望看到你的游戏中可能提供的武器的列表，所有可能提供的角色分类或地图的列表，等等。

**区域设置字符串对**  
每个可能的“完整状态”字符串都必须有与其关联的区域设置，以指定该字符串可以/应该在其中使用的区域。 每个枚举还将有一组区域设置字符串对。

**字符串集**  
字符串集由一组区域设置字符串对组成。 此集为所有可能的区域设置定义可能的“完整状态”值，或为所有可能的区域设置定义可能的枚举值。

**友好名称**  
友好名称有两种类型：

**“完整状态”字符串**  
字符串集的友好名称是以用来引用字符串集的字符串形式显示的唯一标识符。

**枚举**  
这些友好名称用于唯一识别特定枚举，如武器枚举或角色分类枚举。


## <a name="in-this-section"></a>本部分内容

[“完整状态”配置](rich-presence-strings-configuration.md)  
如何配置游戏中使用的“完整状态”。

[“完整状态”更新字符串](rich-presence-strings-updating-strings.md)  
如何从游戏中更新“完整状态”字符串。

[“完整状态”最佳实践](rich-presence-strings-best-practices.md)  
有关在游戏中使用“完整状态”的最佳实践。

[“完整状态”策略和限制](rich-presence-strings-policies-and-limitations.md)  
有关在游戏中使用“完整状态”的策略。

[“完整状态”附录](rich-presence-strings-appendix.md)  
数据平台与“完整状态”有关的其他示例和详细信息。

[对 Xbox Live“完整状态”进行编程](programming-rich-presence.md)  
演示如何通过 Xbox Live 使用“完整状态”。
