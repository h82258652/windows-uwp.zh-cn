---
author: WilliamsJason
title: Device Portal Fiddler API 参考
description: 了解如何以编程方式启用/禁用 Fiddler 跟踪。
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: e7d4225e-ac2c-41dc-aca7-9b1a95ec590b
ms.localizationpriority: medium
ms.openlocfilehash: 8e0faf3a0b6a4f13c0fce24aa093cf94a1e7ee7e
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/19/2018
ms.locfileid: "7284641"
---
# <a name="fiddler-settings-api-reference"></a>Fiddler 设置 API 参考   
可以使用此 REST API 在开发工具包上启用和禁用 Fiddler 网络跟踪。

## <a name="determine-if-fiddler-tracing-is-enabled"></a>确定是否已启用 Fiddler 跟踪

**请求**

你可以检查以查看是否已使用以下请求在设备上启用 Fiddler 跟踪。

方法      | 请求 URI
:------     | :-----
GET | /ext/fiddler
<br />
**URI 参数**

- 无

**请求标头**

- 无

**请求正文**   

- 无

**响应**   

- JSON bool 属性 IsProxyEnabled 哪些说明符代理是否已启用。

**状态代码**

此 API 具有以下预期状态代码。

HTTP 状态代码      | 说明
:------     | :-----
200 | 成功
4XX | 错误代码
5XX | 错误代码

## <a name="enable-fiddler-tracing"></a>启用 Fiddler 跟踪

**请求**

可以使用以下请求为开发工具包启用 Fiddler 跟踪。  请注意，必须重启设备才能使此操作生效。

方法      | 请求 URI
:------     | :-----
POST | /ext/fiddler
<br />
**URI 参数**

可以在请求 URI 上指定以下附加参数：

| URI 参数      | 说明     | 
| ------------------ |-----------------|
| proxyAddress       | 运行 Fiddler 的设备的 IP 地址或主机名 |
| proxyPort          | Fiddler 用于监视流量的端口。 默认为 8888 |
| updateCert（可选）| 指示是否提供根 Fiddler 证书的布尔值。 如果 Fiddler 从未在此开发工具包上配置或曾针对其他主机进行配置，则此值必须为 true。  |
<br>

**请求头**

- 无

**请求正文**

- 如果 updateCert 为 false 或未提供，则为 None。 否则为包含 FiddlerRoot.cer 文件的多方一致 http 正文。

**响应**   

- 无  

**状态代码**

此 API 具有以下预期状态代码。

HTTP 状态代码      | 说明
:------     | :-----
204 | 已接受启用 Fiddler 的请求。 将在设备下次重启时启用 Fiddler。
4XX | 错误代码
5XX | 错误代码

## <a name="disable-fiddler-tracing-on-the-devkit"></a>在开发工具包上禁用 Fiddler 跟踪

**请求**

可以使用以下请求在设备上禁用 Fiddler 跟踪。 请注意，必须重启设备才能使此操作生效。

方法      | 请求 URI
:------     | :-----
DELETE | /ext/fiddler
<br />
**URI 参数**

- 无

**请求标头**

- 无

**请求正文**   

- 无

**响应**   

- 无 

**状态代码**

此 API 具有以下预期状态代码。

HTTP 状态代码      | 说明
:------     | :-----
204 | 禁用 Fiddler 跟踪的请求已成功。 将在设备下次重启时禁用跟踪。
4XX | 错误代码
5XX | 错误代码

<br />
**可用设备系列**

* Windows Xbox

## <a name="see-also"></a>另请参阅
- [为 Xbox 上的 UWP 配置 Fiddler](uwp-fiddler.md)

