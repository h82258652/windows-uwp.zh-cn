---
title: "管理本地连接存储"
author: KevinAsgari
description: "了解如何在开发环境中管理本地连接存储数据。"
ms.assetid: 630cb5fc-5d48-4026-8d6c-3aa617d75b2e
ms.author: kevinasg
ms.date: 04-04-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "xbox live, xbox, 游戏, uwp, windows 10, xbox one, 连接存储"
ms.openlocfilehash: 9acab570049922f3dbe83538d473e0e9523da4d2
ms.sourcegitcommit: 90fbdc0e25e0dff40c571d6687143dd7e16ab8a8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/06/2017
---
# <a name="managing-local-connected-storage"></a>管理本地连接存储

*xbStorage* 是一种开发工具，可在开发电脑的 Xbox One 开发工具包中管理连接存储的本地数据。

它允许从硬盘上清除连接存储空间中的本地数据，以及通过使用 XML 文件将用户或计算机的数据导入/导出连接存储空间。

在连接存储空间的本地数据上执行操作时，系统会像应用本身执行了操作那样运行，因此，将连接存储空间中的数据读取到本地文件的操作会导致在复制之前与云同步。

同样，将开发电脑上 XML 文件中的数据复制到 Xbox One 开发工具包上的连接存储容器，也会导致控制台开始将此数据上传到云。 但是，在有些情况中不会发生此现象：如果开发工具包无法获取锁定，或者，如果控制台上的容器和云中的容器之间存在出具冲突，则控制台会像用户已经决定不解决冲突（通过选取要保存的一个容器版本）那样运行，并且在下次启动游戏之前，控制台会像用户正在脱机运行。

这些命令的一个例外是 **reset** **/f**，它会为所有 SCID 和用户清除已保存数据的本地存储，但不会改变在云中存储的数据。 如果用户漫游到控制台，并在运行一个游戏后从云中下载数据，那么，这对将控制台置于它应所处的状态非常有用。

有关 xbStorage 的详细信息，请参阅 XDK 文档中的*管理连接存储 (xbstorage.exe)*。
