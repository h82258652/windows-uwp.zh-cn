---
title: Xbox Live 存储平台
author: KevinAsgari
description: 了解 Xbox Live 存储平台，其中包括连接存储和标题存储。
ms.assetid: 3c92549c-65fd-4d26-a693-3aded8bae498
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: ecdf1bb3da9f2cd92e95976e07bbc2fd58a51800
ms.sourcegitcommit: 1938851dc132c60348f9722daf994b86f2ead09e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/03/2018
ms.locfileid: "4265627"
---
# <a name="xbox-live-storage-platform---connected-storage-title-storage"></a>Xbox Live 存储平台 - 连接存储、标题存储

借助 Xbox Live，发布者能够在云中存储全局游戏数据和特定于玩家的数据。

## <a name="in-this-section"></a>本部分内容

[连接存储](connected-storage/connected-storage-overview.md)采用每用户连接存储 API 存储的数据会自动漫游，可供用户跨电脑和多个 Xbox One 主机使用，并且还能脱机使用。 使用该服务允许在设备之间切换后重新启动标题时游戏顺利继续。 应使用该连接存储服务以经常保存游戏进度数据，如清单、游戏状态及游戏中的当前位置。 连接存储服务是容错能力更强的云存储服务，相对不易受到网络和电源故障的影响。

[Xbox Live 标题存储](xbox-live-title-storage/xbox-live-title-storage.md)Xbox Live 标题存储服务提供了一种在云中存储并共享游戏数据和标题资源的方法。 所有平台上运行的游戏均可在线使用此服务。 此服务为消费者对数据可见性提供了更多的控制，除每个用户数据外，还包括每个标题的全局数据。 标题存储非常适合存储玩家统计数据、玩家排名以及标题资产（如可解锁的插图和新的地图）。