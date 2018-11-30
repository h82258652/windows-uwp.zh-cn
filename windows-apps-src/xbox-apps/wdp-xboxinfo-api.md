---
title: Device Portal Xbox 信息 API 参考
description: 了解如何访问 Xbox 设备信息。
ms.date: 11/7/2017
ms.topic: article
keywords: windows 10，uwp，xbox，设备门户
ms.localizationpriority: medium
ms.openlocfilehash: d7901890e1cc8fab24742e8785562d13d2fe182a
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "8206709"
---
# <a name="xbox-info-api-reference"></a>Xbox 信息 API 参考   
你可以访问 Xbox One 使用此 API 的设备信息。

## <a name="get-xbox-one-device-information"></a>获取 Xbox One 设备信息

**请求**

你可以获取有关在 Xbox One 设备信息。

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
* OsEdition-（字符串） 版本的操作系统，如"2017 年 3 月"或"2017 年 3 月 QFE 1"。
* ConsoleId-（字符串） 主机的 id。
* DeviceId-（字符串） 控制台的 Xbox Live 设备 id。
* 序列号-（字符串） 控制台的序列号。
* DevMode-（字符串） 控制台的当前开发人员模式，例如"None"或"零售"。
* ConsoleType-（字符串） 主机的类型，如"Xbox One"或"Xbox One S"。
* DevkitCertificateExpirationTime-(Number) 以秒为单位控制台的开发人员工具包证书将到期时的 UTC 时间。

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