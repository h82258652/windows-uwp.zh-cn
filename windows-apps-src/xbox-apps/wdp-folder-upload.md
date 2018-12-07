---
title: Device Portal 文件夹上载 API 参考
description: 了解如何以编程方式访问文件夹上载 API。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: e1a2c7f0-0040-4ce7-94de-17224736e20b
ms.localizationpriority: medium
ms.openlocfilehash: 0805dbeedcf66bc3596f3d284f51e8f177608396
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/07/2018
ms.locfileid: "8795084"
---
# <a name="upload-a-folder-to-the-development-directory"></a>将文件夹上载到开发目录

**请求**

你可以一次将整个文件夹上载到 DevelopmentFiles 的“已知文件夹 ID”（或上载到该文件中的子文件夹）。

方法      | 请求 URI
:------     | :------
POST | /api/app/packagemanager/upload 
<br />
**URI 参数**

你可以在请求 URI 上指定以下附加参数：

URI 参数      | 描述
:------     | :-----
destinationFolder（必需） | 要上载的文件夹的目标文件夹名称。 此文件夹将放置在主机的 d:\developmentfiles\LooseApps 之下。 如果此文件夹是 LooseApps 下面的一个子文件夹，那么该文件夹的名称应进行 base64 编码，因为它可能包含路径分隔符。
<br />

**请求标头**

- 无

**请求正文**

- 目录内容的多部分一致性 http 正文。

**响应**

**状态代码**

此 API 具有以下预期状态代码。

HTTP 状态代码      | 说明
:------     | :-----
200 | 成功
4XX | 错误代码
5XX | 错误代码
<br />
**可用设备系列**

* Windows Xbox

