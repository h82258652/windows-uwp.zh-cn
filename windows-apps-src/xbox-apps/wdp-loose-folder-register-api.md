---
title: Device Portal Loose 文件夹注册 API 参考
description: 了解如何以编程方式访问 Loose 文件夹注册 API。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: efdf4214-9738-4df6-bf1f-ed7141696ef6
ms.localizationpriority: medium
ms.openlocfilehash: 6e1a6d2e0c408d37195f5a4764f71c2acc932ab5
ms.sourcegitcommit: 681c1e3836d2a51cd3b31d824ece344281932bcd
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/08/2019
ms.locfileid: "59240005"
---
# <a name="register-an-app-in-a-loose-folder"></a>注册 Loose 文件夹中的应用  

**请求**

你可以利用以下请求格式来注册 Loose 文件夹中的应用。

方法      | 请求 URI
:------     | :------
发布 | /api/app/packagemanager/register

**URI 参数**

可以在请求 URI 上指定以下附加参数：

URI 参数      | 描述
:------     | :-----
文件夹（必需） | 要注册的程序包的目标文件夹名称。 此文件夹必须位于主机的 d:\developmentfiles\LooseApps 之下。 如果此文件夹位于 LooseApps 下面的一个子文件夹中，那么该文件夹的名称应进行 base64 编码，因为它可能包含路径分隔符。

**请求标头**

- 无

**请求正文**

- 无

**响应**

**状态代码**

此 API 具有以下预期状态代码。

HTTP 状态代码      | 描述
:------     | :-----
200 | 已接受和正在处理的部署请求
4XX | 错误代码
5XX | 错误代码

**可用设备系列**

* Windows Xbox

**说明**

至少有三种不同的方式可以将主机上的 Loose 应用放在目标文件夹中。 最简单的方法是直接复制到 SMB 通过文件\\< IP_Address > \DevelopmentFiles\LooseApps。 这需要使用 UWA 工具包的用户名和密码，用户名和密码可以通过 [/ext/smb/developerfolder](wdp-smb-api.md) 获取。 

第二种方法是通过对 /api/filesystem/apps/file 执行 POST，将各文件复制到正确的位置，其中 knownfolderid 为 DevelopmentFiles、packagefullname 为空，并正确地提供了文件名和路径（路径应该以 LooseApps 开头）。

第三种方法是通过 [/api/app/packagemanager/upload](wdp-folder-upload.md) 一次性复制整个文件夹，其中 destinationFolder 为要放在 d:\developmentfiles\looseapps 下面的文件夹的名称，有效负载是目录内容的多部分一致性 http 正文。

