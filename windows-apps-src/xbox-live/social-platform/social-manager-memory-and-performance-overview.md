---
title: 社交管理器内存和性能
author: KevinAsgari
description: 介绍使用 Xbox Live 社交管理器 API 管理器时内存和性能方面的注意事项。
ms.assetid: 2540145e-b8e2-4ab5-9390-65e2c9b19792
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Xbox live, xbox, 游戏, uwp, windows 10, xbox one, 社交管理器, 人脉
ms.localizationpriority: medium
ms.openlocfilehash: 4e32ea2c1938bc72642d8affa031dffd538d4feb
ms.sourcegitcommit: f5321b525034e2b3af202709e9b942ad5557e193
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/18/2018
ms.locfileid: "4020734"
---
# <a name="social-manager-memory-and-performance-overview"></a>社交管理器内存和性能概述

## <a name="memory"></a>内存
社交管理器现在允许被分配可以使其保留在标题空间内的永久内存。 供社交管理器使用的自定义内存分配器通过调用 `xbox_live_services_settings::set_memory_allocation_hooks()` 指定。 此内存分配器挂钩还可以用于将来 Xbox Live API (XSAPI) 所使用的大型内存分配。

社交管理器最糟糕的内存分配情况默认应该是大约 4.75 mb (1)。 社交管理器还可能基于 RTA 和 HTTP 更新另外分配一些内存。 如果从列表创建 `xbox_social_user_group`，每个增加的成员将另外占用 2.43 kb。 如果用户是通过 `create_social_social_user_group_from_list`、`update_social_user_group` 添加的，或用户在标题之外添加好友，这可能导致重新分配以寻找连续的内存空间。

(1) 4.75 mb 来自 = 每个 2.43 kb 的 1000 个 `xbox_social_user` * 2。 2 是该社交管理器保留双内存缓冲区。

## <a name="additional-user-limits"></a>其他用户限制
社交管理器目前对添加到图片中的用户数量限制为 100 个。 这是由于两个问题：

1. 用户可以订阅的 RTA 订阅的最大数是 1100。 如果本地用户有 1000 个好友，就只能为其他订阅留 100 个配额。
2. 可以发送到 PeopleHub 的用户的最大批大小目前大约是 100。

社交管理器通过不允许列表中的社交用户组包含超过 100 个用户来传达此限制。 任何时候通过 `create_social_user_group_from_list` 加入系统的其他用户有 100 个用户总数的全局限制。

## <a name="processing-time"></a>处理时间
社交管理器最糟糕的情况是 1100 个用户。 Xbox One 上社交管理器的性能特征的最糟糕情况是 `do_work` 的时间为 0.3 ms 到 0.5 ms，具体取决于所创建社交图片的数量。 典型情况是不创建组时为 0.01 ms，创建 1000 个用户的组最多 0.2 ms。
