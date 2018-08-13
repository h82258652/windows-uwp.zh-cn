---
author: M-Stahl
title: 设备门户 Xbox info API 参考 （英文）
description: 了解如何访问 Xbox 设备信息。
ms.author: mstahl
ms.date: 11/7/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10 uwp、 xbox、 设备门户
ms.localizationpriority: medium
ms.openlocfilehash: db1df2418a2bb60de4a72f51ad01a0bfd547ec20
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/13/2018
ms.locfileid: "406247"
---
# <a name="xbox-info-api-reference"></a>Xbox Info API 参考 （英文）   
您可以访问使用此 API Xbox 一个设备信息。

## <a name="get-xbox-one-device-information"></a>获取一个 Xbox 设备信息

**请求**

您可以获取有关您 Xbox 一个设备信息。

方法      | 请求 URI
:------     | :-----
GET | /ext/xbox/info
<br />
**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**   
具有以下字段的 JSON 对象：

* OsVersion-（字符串） 版本的操作系统。
* OsEdition-（字符串） 的版本的操作系统，如"年 3 月 2017"或"年 3 月 2017 QFE 1"。
* ConsoleId-（字符串） 控制台的 id。
* DeviceId-（字符串） 控制台的 Xbox Live 设备 id。
* SerialNumber-（字符串） 控制台的序列号。
* DevMode-（字符串） 控制台的当前开发人员模式，如"None"或"发布"。
* ConsoleType-（字符串） 控制台的类型，如"Xbox 一"或"Xbox 一个 S"。
* DevkitCertificateExpirationTime-（数目） 以秒为单位控制台的开发人员工具包证书过期时的 UTC 时间。

**状态代码**

此 API 具有以下预期状态代码。

HTTP 状态代码      | 说明
:------     | :-----
200 | 请求已成功
4XX | 错误代码
5XX | 错误代码

<br />
**可用设备系列**

* Windows Xbox