---
author: WilliamsJason
title: "Device Portal Fiddler API 参考"
description: "了解如何以编程方式启用/禁用 Fiddler 跟踪。"
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: e7d4225e-ac2c-41dc-aca7-9b1a95ec590b
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 24e966f953928d238f9197359e0b539b8a3e5c3c
ms.lasthandoff: 02/08/2017

---

# <a name="fiddler-settings-api-reference"></a>Fiddler 设置 API 参考   
可以使用此 REST API 在开发工具包上启用和禁用 Fiddler 网络跟踪。

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


