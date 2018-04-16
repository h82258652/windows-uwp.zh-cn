---
title: 清除本地存储
author: KevinAsgari
description: 了解如何清除连接存储数据的本地存储。
ms.assetid: 0701b03e-88e4-4720-9744-ca174f3c947d
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, 连接存储
ms.localizationpriority: low
ms.openlocfilehash: aeb42afbed4be764134b0a5b1a7581c109a80aa0
ms.sourcegitcommit: 79f09d32ee89b32fac12fb1849df991b2d6c3c0a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/07/2018
---
# <a name="clearing-local-storage"></a>清除本地存储

使用连接存储 API 写入的所有数据都将保存到开发工具包上的存储卷中。 可以使用 *xbstorage* 工具删除本地存储的数据，以便执行连接存储的恢复出厂设置。

### <a name="to-clear-local-storage"></a>要清除本地存储

1.  终止与要清除数据关联的应用。
2.  运行指定** reset **命令和** /force **开关的 *xbstorage* 工具。

        xbstorage reset /force

3.  重新启动控制台。
