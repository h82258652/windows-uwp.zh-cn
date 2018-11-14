---
title: Xbox Live SDK 的新增功能 - 2015 年 10 月
author: KevinAsgari
description: Xbox Live SDK 的新增功能 - 2015 年 10 月
ms.assetid: 052be4aa-5f18-4eb7-ba5f-80c5f5cab6f2
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 09f8fe60cf30ee14b9b0e0c8f1c1c98feef1fa59
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2018
ms.locfileid: "6279399"
---
# <a name="whats-new-for-the-xbox-live-sdk---october-2015"></a>Xbox Live SDK 的新增功能 - 2015 年 10 月

请参阅[新增功能 - 2015 年 9 月](1509-whats-new.md)文章了解 2015 年 9 月版本中增加了哪些功能。


## <a name="os-and-tool-support"></a>操作系统和工具支持
Xbox Live SDK 支持 Windows 10 RTM [版本 10.0.10240] 和 Visual Studio 2015 RTM [版本 14.0.23107.0]。

## <a name="social-manager"></a>社交管理器
* 社交管理器主要是为了让使用 Xbox Live 社交 API 更轻松。  它将智能地缓存用户社交图片、提供更简单的 API 等。  有关详细信息，请参阅文档。

## <a name="samples"></a>示例
* 我们提供了演示 API 的新社交管理器示例。

## <a name="tools"></a>工具
* Xbox Live 跟踪分析器现在包含在 Xbox Live SDK 中。  按照[分析对 Xbox Live 服务的调用](../tools/analyze-service-calls.md)中所述收集跟踪信息，然后通过查看重复调用、调用频率等统计信息运行 Live 跟踪分析器，以确保你的游戏以最佳方式使用 Xbox Live。

## <a name="bug-fixes"></a>Bug 修复
* 将 HTTP 套接字操作的默认超时从 5 分钟更改为 30 秒。  对于长时间运行的 HTTP 套接字操作（如标题存储上传和下载调用），仍使用 5 分钟超时。

## <a name="documentation"></a>文档
* 增加了社交管理器简介
* 更新了多人游戏管理器简介
