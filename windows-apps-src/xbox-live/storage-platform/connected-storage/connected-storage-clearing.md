---
title: "清除本地存储"
author: KevinAsgari
description: "了解如何清除连接存储数据的本地存储。"
ms.assetid: 0701b03e-88e4-4720-9744-ca174f3c947d
ms.author: kevinasg
ms.date: 04-04-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "xbox live, xbox, 游戏, uwp, windows 10, xbox one, 连接存储"
ms.openlocfilehash: f6f0b45751148c12306ea225c56a578aea11be83
ms.sourcegitcommit: 90fbdc0e25e0dff40c571d6687143dd7e16ab8a8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/06/2017
---
# <a name="clearing-local-storage"></a>清除本地存储

使用连接存储 API 写入的所有数据都将保存到开发工具包上的存储卷中。 可以使用 *xbstorage* 工具删除本地存储的数据，以便执行连接存储的恢复出厂设置。

### <a name="to-clear-local-storage"></a>要清除本地存储

1.  终止与要清除数据关联的应用。
2.  运行指定** reset **命令和** /force **开关的 *xbstorage* 工具。

        xbstorage reset /force

3.  重新启动控制台。
