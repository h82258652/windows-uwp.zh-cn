---
author: mcleblanc
ms.assetid: BC7E8130-A28A-443C-8D7E-353E7DA33AE3
description: "Entity Framework (EF) 是一个对象关系映射程序，支持使用特定于域的对象处理关系数据。"
title: "面向 C# 应用的带有 SQLite 的 Entity Framework 7"
ms.author: markl
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, SQLite, C#, EF, 实体框架"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 2ab2a12f6c2bc2f0f8853b404afaf13bf80635b7
ms.lasthandoff: 02/07/2017

---

# <a name="entity-framework-core-1-with-sqlite-for-c-apps"></a>面向 C# 应用的带有 SQLite 的 Entity Framework Core 1

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Entity Framework (EF) 是一个对象关系映射程序，支持使用特定于域的对象处理关系数据。 本文介绍如何在通用 Windows 应用中使用带有 SQLite 数据库的 Entity Framework Core 1。

最初对于 .NET 开发人员而言，Entity Framework Core 1 可以在通用 Windows 平台 (UWP) 上与 SQLite 一起使用，从而可以使用特定于域的对象存储和处理关系数据。 你可以将 EF 代码从 .NET 应用迁移到 UWP 应用，并希望它能够处理连接字符串的相应更改。

当前，EF 仅在 UWP 上支持 SQLite。 [通用 Windows 平台入门](http://go.microsoft.com/fwlink/p/?LinkId=735013)页上提供了有关安装 Entity Framework Core 1 和创建模型的详细操作实例。 它包含以下主题：

-   先决条件
-   创建新项目
-   安装实体框架
-   创建模型
-   创建数据库
-   使用模型

