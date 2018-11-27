---
title: 设备门户 HTTP 监视器 API 参考
description: 了解如何从 Xbox 上的侧重型应用访问 HTTP 流量。
ms.localizationpriority: medium
ms.openlocfilehash: 81de2a2a3194384e9c5de1c5c45a827e4d965c91
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/27/2018
ms.locfileid: "7826114"
---
# <a name="http-monitor-api-reference"></a>HTTP 监视器 API 参考   
如果已通过在开发人员主页选中相应的框在 Xbox 主机上启用 HTTP 监视器，则可使用此 API 访问侧重型应用的实时 HTTP 流量。

## <a name="get-if-the-http-monitor-is-enabled"></a>获取是否已启用 HTTP 监视器

**请求**

可在开发人员主页中获取是否已启用 HTTP 监视器。

方法      | 请求 URI
:------     | :-----
GET | /ext/httpmonitor/sessions
<br />
**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**   
具有以下字段的 JSON 对象：

* 已启用 -（布尔型）是否已通过在开发人员主页中选中相应的框来在 Xbox 主机上启用 HTTP 监视器。

**状态代码**

此 API 具有以下预期状态代码。

HTTP 状态代码      | 说明
:------     | :-----
200 | 请求已成功
4XX | 错误代码
5XX | 错误代码

## <a name="get-http-traffic-from-the-focused-app"></a>获取来自侧重型应用的 HTTP 流量
**请求**

如果已从开发人员主页启用 HTTP 监视器，则以实时方式获取来自 Xbox 上的侧重型应用的 HTTP 流量，前提是它不是系统应用。

方法      | 请求 URI
:------     | :-----
Websocket | /ext/httpmonitor/sessions
<br />
**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**   
具有以下字段的 JSON 对象：

* 会话
    * RequestHeaders -（JSON 对象）HTTP 请求中的请求标头。
    * RequestContentHeaders -（JSON 对象）HTTP 请求中的请求内容标头。
    * RequestURL -（字符串）请求 URL。
    * RequestMethod -（字符串）请求方法。
    * RequestMessage -（字符串）请求消息，当前仅支持 JSON 和文本内容。
    * ResponseHeaders -（JSON 对象）HTTP 响应中的响应标头。
    * ResponseContentHeaders -（JSON 对象）HTTP 响应中的响应内容标头。
    * StatusCode -（数字）响应状态代码。
    * ReasponsePhrase-（字符串）响应原因短语。
    * ResponseMessage -（字符串）响应消息，当前仅支持 JSON 和文本内容。

**状态代码**

此 API 具有以下预期状态代码。

HTTP 状态代码      | 说明
:------     | :-----
200 | 请求已成功
4XX | 错误代码
403 | HTTP 监视器已禁用，必须在开发人员主页中启用
5XX | 错误代码

<br />
**可用设备系列**

* Windows Xbox