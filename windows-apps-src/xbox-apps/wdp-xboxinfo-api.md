---
title: Device Portal Xbox 信息 API 参考
description: 了解如何访问 Xbox 设备信息。
ms.date: 11/072017
ms.topic: article
keywords: windows 10、 uwp、 xbox、 设备门户
ms.localizationpriority: medium
ms.openlocfilehash: 7aa8b11bc439266d36fbb27a7eaa7b07e924a17c
ms.sourcegitcommit: bad7ed6def79acbb4569de5a92c0717364e771d9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/08/2019
ms.locfileid: "59244033"
---
# <a name="xbox-info-api-reference"></a>Xbox 信息 API 参考   
你可以使用此 API 访问 Xbox One 设备信息。

## <a name="get-xbox-one-device-information"></a>获取 Xbox One 设备信息

## <a name="request"></a>请求

你可以获取与 Xbox One 相关的设备信息。

方法      | 请求 URI
:------     | :-----
GET | /ext/xbox/info

**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

## <a name="response"></a>响应
具有以下字段的 JSON 对象：

* OsVersion -（字符串）操作系统的版本。
* OsEdition -（字符串）操作系统的版本，如“2017 年 3 月”或“2017 年 3 月 QFE 1”。
* ConsoleId -（字符串）控制台 ID。
* DeviceId -（字符串）控制台的 Xbox Live 设备 ID。
* SerialNumber -（字符串）控制台的序列号。
* DevMode -（字符串）控制台当前的开发人员模式，如“无”或“零售”。
* ConsoleType -（字符串）控制台的类型，如“Xbox One”或“Xbox One S”。
* DevkitCertificateExpirationTime -（数值）控制台的开发工具包证书将过期的 UTC 时间（以秒为单位）。

**状态代码**

此 API 具有以下预期状态代码。

HTTP 状态代码      | 描述
:------     | :-----
200 | 请求已成功
4XX | 错误代码
5XX | 错误代码

**可用设备系列**

* Windows Xbox