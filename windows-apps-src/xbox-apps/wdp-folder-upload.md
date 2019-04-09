---
title: Device Portal 文件夹上载 API 参考
description: 了解如何以编程方式访问文件夹上载 API。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: e1a2c7f0-0040-4ce7-94de-17224736e20b
ms.localizationpriority: medium
ms.openlocfilehash: 870d203271cb75ecf5531106bb2c10b3736db9b9
ms.sourcegitcommit: bad7ed6def79acbb4569de5a92c0717364e771d9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/08/2019
ms.locfileid: "59244043"
---
# <a name="upload-a-folder-to-the-development-directory"></a>将文件夹上载到开发目录

**请求**

你可以一次将整个文件夹上载到 DevelopmentFiles 的“已知文件夹 ID”（或上载到该文件中的子文件夹）。

方法      | 请求 URI
:------     | :------
发布 | /api/app/packagemanager/upload 

**URI 参数**

可以在请求 URI 上指定以下附加参数：

URI 参数      | 描述
:------     | :-----
destinationFolder（必需） | 要上载的文件夹的目标文件夹名称。 此文件夹将放置在主机的 d:\developmentfiles\LooseApps 之下。 如果此文件夹是 LooseApps 下面的一个子文件夹，那么该文件夹的名称应进行 base64 编码，因为它可能包含路径分隔符。


**请求标头**

- 无

**请求正文**

- 目录内容的多部分一致性 http 正文。

**响应**

**状态代码**

此 API 具有以下预期状态代码。

HTTP 状态代码      | 描述
:------     | :-----
200 | 成功
4XX | 错误代码
5XX | 错误代码

**可用设备系列**

* Windows Xbox

