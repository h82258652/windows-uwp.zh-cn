---
title: Arena API UI 元数据
author: KevinAsgari
description: 包含用于 Xbox Arena API 的 UI 元数据的完整列表。
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, arena, 锦标赛, ux
ms.localizationpriority: medium
ms.openlocfilehash: e2f785836b6d81213ca3e13d9687fd67beae1259
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2018
ms.locfileid: "6263932"
---
# <a name="arena-apis-a-comprehensive-list-of-ui-metadata"></a>Arena API：UI 元数据的完整列表

Arena API 返回用于突出显示游戏中的锦标赛、比赛和玩家详细信息的元数据。 下面是完整列表。

锦标赛详细信息  | 比赛详细信息 | 团队详细信息  | 注册详细信息
--- | --- | --- | ---
组织者名称 | 结束时间 - 此锦标赛变为“已取消”或“已结束”状态的日期/时间。 锦标赛更新时会自动设置结束时间。 | 上一场比赛（锦标赛结果状态、排名、结束时间、比赛开始时间、轮空、描述、对方团队 ID） | 注册状态（未知、挂起、已撤销、已拒绝、已注册、已完成）
锦标赛名称（最多 128 个字符） | 轮空   | 团队状态（未知、已注册、已进入等待列表、候补、已报到、正在比赛、已完成） | 注册原因（未知、已关闭、成员已注册、锦标赛已报满、团队已被淘汰、锦标赛已结束）
锦标赛描述（最多 1000 个字符） | 锦标赛结果状态（无赛事/已取消、胜、负、平局、排名、无显示） | 团队处于已完成状态的原因（已拒绝、已退出、已被淘汰、已完成） | 最小和最大已注册团队数
注册开始/结束时间 | 仲裁状态：已完成、无、已取消、无结果、部分结果 | 注册日期 - 注册团队的日期和时间。 |
报到开始/结束时间 | 比赛描述性“标签”–（“决赛，热度 1”） | 团队排名 | 正在注册
比赛开始/结束时间 | 开始时间 | 团队名称 | 正在报到
有奖项 | 对方团队 ID | 团队最终排名 | 注册开始/结束时间
最小/最大团队规模 | | 延续任务 URI - 将团队成员带回到 Arena UI | 报到开始/结束时间
游戏模式 | | 当前比赛元数据（描述、开始时间、轮空、对方团队 ID） | 已注册团队计数
锦标赛赛制（单淘汰赛、循环赛） | | 团队概要（团队状态、排名） |
正在注册 | | 玩家代号 |
正在报到 | | |
已注册团队计数 | | |
正在比赛 | | |
