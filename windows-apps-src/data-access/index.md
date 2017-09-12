---
author: normesta
ms.assetid: 76776b0f-3163-48c9-835b-3f4213968079
title: "数据访问"
description: "本节讨论在专用数据库中存储设备上的数据，并在通用 Windows 平台 (UWP) 应用中使用对象关系映射。"
ms.author: normesta
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, 数据, 数据库, 关系, 表, sqlite"
ms.openlocfilehash: 19690b6877fb4304b7740e6098711ca0b097d567
ms.sourcegitcommit: 378382419f1fda4e4df76ffa9c8cea753d271e6a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/08/2017
---
# <a name="data-access"></a>数据访问

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

本部分讨论在专用数据库中存储设备上的数据，并在通用 Windows 平台 (UWP) 应用中使用对象关系映射。

SQLite 包括在 UWP SDK 中。 Entity Framework Core 在 UWP 应用中可与 SQLite 一起协作。 若要针对离线/间歇性连接方案进行开发，并在应用会话上保留数据，请使用这些技术。

| 主题 | 说明|
|-------|------------|
| [面向 C# 应用的带有 SQLite 的 Entity Framework Core](entity-framework-7-with-sqlite-for-csharp-apps.md) | Entity Framework (EF) 是一个对象关系映射程序，支持使用特定于域的对象处理关系数据。 本文介绍如何在通用 Windows 应用中使用带有 SQLite 数据库的 Entity Framework Core。 |
| [SQLite 数据库](sqlite-databases.md) | SQLite 是一种无服务器的嵌入式数据库引擎。 本文介绍了如何使用包含在 SDK 中的 SQLite 库、如何将自己的 SQLite 库打包在通用 Windows 应用中或从源生成该库。 |
