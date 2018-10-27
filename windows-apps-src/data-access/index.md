---
author: normesta
ms.assetid: 76776b0f-3163-48c9-835b-3f4213968079
title: 数据访问
description: 本节讨论在专用数据库中存储设备上的数据，并在通用 Windows 平台 (UWP) 应用中使用对象关系映射。
ms.author: normesta
ms.date: 11/13/2017
ms.topic: article
keywords: windows 10, uwp, 数据, 数据库, 关系, 表, sqlite
ms.localizationpriority: medium
ms.openlocfilehash: beca20d358430ecd82cd1bc57459a6f6af36be77
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2018
ms.locfileid: "5693431"
---
# <a name="data-access"></a>数据访问

你可以通过使用 SQLite 数据库在用户设备上存储数据。 你可以也你的应用直接连接到 SQL Server 数据库无需使用任何一种服务层。

| 主题 | 说明|
|-------|------------|
| [在 UWP 应用中使用 SQLite 数据库](sqlite-databases.md) | 向你显示了如何使用 SQLite 来存储和检索用户设备上的轻量级数据库中的数据。 SQLite 是一种无服务器的嵌入式数据库引擎。 |
| [在 UWP 应用中使用 SQL server 数据库](sql-server-databases.md) | 显示了如何将直接连接到 SQL Server 数据库然后存储和检索数据使用[System.Data.SqlClient](https://msdn.microsoft.com/library/system.data.sqlclient.aspx)命名空间中的类。 无需服务层。 |

## <a name="related-topics"></a>相关主题

* [客户订单数据库示例](https://github.com/Microsoft/Windows-appsample-customers-orders-database)
