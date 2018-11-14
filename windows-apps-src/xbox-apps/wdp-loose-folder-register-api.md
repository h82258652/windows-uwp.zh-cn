---
author: WilliamsJason
title: Device Portal Loose 文件夹注册 API 参考
description: 了解如何以编程方式访问 Loose 文件夹注册 API。
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: efdf4214-9738-4df6-bf1f-ed7141696ef6
ms.localizationpriority: medium
ms.openlocfilehash: cb80e2dbd7ebdfbb05bd642b9875a9cd7cc356f3
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2018
ms.locfileid: "6647885"
---
# <a name="register-an-app-in-a-loose-folder"></a>注册 Loose 文件夹中的应用  

**请求**

你可以利用以下请求格式来注册 Loose 文件夹中的应用。

方法      | 请求 URI
:------     | :------
POST | /api/app/packagemanager/register
<br />
**URI 参数**

你可以在请求 URI 上指定以下附加参数：

URI 参数      | 描述
:------     | :-----
文件夹（必需） | 要注册的程序包的目标文件夹名称。 此文件夹必须位于主机的 d:\developmentfiles\LooseApps 之下。 如果此文件夹位于 LooseApps 下面的一个子文件夹中，那么该文件夹的名称应进行 base64 编码，因为它可能包含路径分隔符。
<br />

**请求标头**

- 无

**请求正文**

- 无

**响应**

**状态代码**

此 API 具有以下预期状态代码。

HTTP 状态代码      | 说明
:------     | :-----
200 | 已接受和正在处理的部署请求
4XX | 错误代码
5XX | 错误代码
<br />
**可用设备系列**

* Windows Xbox

**注意**

至少有三种不同的方式可以将主机上的 Loose 应用放在目标文件夹中。 最简单的方式是通过 SMB 将这些文件复制到 \\&lt;IP_Address&gt;\DevelopmentFiles\LooseApps。 这需要使用 UWA 工具包的用户名和密码，用户名和密码可以通过 [/ext/smb/developerfolder](wdp-smb-api.md) 获取。 

第二种方法是通过对 /api/filesystem/apps/file 执行 POST，将各文件复制到正确的位置，其中 knownfolderid 为 DevelopmentFiles、packagefullname 为空，并正确地提供了文件名和路径（路径应该以 LooseApps 开头）。

第三种方法是通过 [/api/app/packagemanager/upload](wdp-folder-upload.md) 一次性复制整个文件夹，其中 destinationFolder 为要放在 d:\developmentfiles\looseapps 下面的文件夹的名称，有效负载是目录内容的多部分一致性 http 正文。

