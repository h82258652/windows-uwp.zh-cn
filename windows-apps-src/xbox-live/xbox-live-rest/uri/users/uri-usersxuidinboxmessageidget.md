---
title: GET (/users/xuid({xuid})/inbox/{messageId})
assetID: d76563d0-2c74-0308-054b-762c80392a02
permalink: en-us/docs/xboxlive/rest/uri-usersxuidinboxmessageidget.html
author: KevinAsgari
description: " GET (/users/xuid({xuid})/inbox/{messageId})"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 8e94396f86b235aafce2e8a65f93eedbdc96f46b
ms.sourcegitcommit: 1c6325aa572868b789fcdd2efc9203f67a83872a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2018
ms.locfileid: "4752961"
---
# <a name="get-usersxuidxuidinboxmessageid"></a>GET (/users/xuid({xuid})/inbox/{messageId})
检索特定用户消息，将其标记为已在服务上的读的详细的消息文本。
这些 Uri 的域是`msg.xboxlive.com`。

  * [备注](#ID4EV)
  * [URI 参数](#ID4EEB)
  * [授权](#ID4ERB)
  * [请求正文](#ID4E3B)
  * [资源的隐私设置的效果](#ID4EJC)
  * [HTTP 状态代码](#ID4EUC)
  * [JavaScript 对象表示法 (JSON) 响应](#ID4EUE)

<a id="ID4EV"></a>


## <a name="remarks"></a>备注

在 get 操作仅可以执行的用户、 系统和 FriendRequest 消息类型。

此 URI 需要在 Xbox.com 刷新。 目前，Xbox 360 中不会更新的读/未读状态，直到用户注销并重新登录。

此 API 支持仅内容类型是"application/json"，这必需的每个调用的 HTTP 标头。

<a id="ID4EEB"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 类型| 说明|
| --- | --- | --- |
| xuid | 64 位无符号的整数 | Xbox 用户 ID (XUID) 发出请求的玩家。 |
| 邮件 Id | 字符串 [50] | 要检索或删除该消息的 ID。 |

<a id="ID4ERB"></a>


## <a name="authorization"></a>授权

你必须拥有自己声明检索用户消息的用户。

<a id="ID4E3B"></a>


## <a name="request-body"></a>请求正文

此请求的正文中不发送任何对象。

<a id="ID4EJC"></a>


## <a name="effect-of-privacy-settings-on-resource"></a>资源的隐私设置的效果

仅可以检索自己用户的消息。

<a id="ID4EUC"></a>


## <a name="http-status-codes"></a>HTTP 状态代码

该服务返回的状态代码之一此部分中使用此方法对此资源所做的请求的响应。 有关使用 Xbox Live 服务的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。

| 代码| 说明|
| --- | --- | --- | --- | --- |
| 200| 成功。|
| 400| 无法正确转换的 XUID。|
| 403| 不能转换 XUID 或找不到有效的 XUID 声明。|
| 404| 有效的 XUID 是缺少，或使用消息 ID 找不到或正确地分析。|
| 500| 常规服务器端错误或消息类型获取无效。|

<a id="ID4EUE"></a>


## <a name="javascript-object-notation-json-response"></a>JavaScript 对象表示法 (JSON) 响应

如果调用成功，该服务将返回采用 JSON 格式的结果数据。 根对象是一个 UserMessageHeader 对象。

#### <a name="usermessageheader"></a>UserMessageHeader

| 属性| 类型| 最大长度| 备注|
| --- | --- | --- | --- |
| 标题| 标题|  | JSON 对象|
| messageText| 字符串| 256| UTF-8|

#### <a name="header"></a>标题

| 属性| 类型| 最大长度| 备注|
| --- | --- | --- | --- |
| 发送| DateTime|  | 日期和时间已发送消息。 （由该服务）。|
| 到期| DateTime|  | 过期日期和时间消息。 （所有消息都具有最大生存时间，以在将来确定）。|
| 可以忽略 messageType| 字符串| 13| 消息类型： 用户、 系统，FriendRequest。|
| senderXuid| ulong|  | 发件人的 XUID。|
| 发送方| 字符串| 15| 发件人的玩家代号。|
| hasAudio| Bool|  | 该消息是否音频 （语音） 附件。|
| hasPhoto| Bool|  | 是否消息包含照片附件。|
| hasText| Bool|  | 是否消息包含文本。|

#### <a name="sample-response"></a>示例响应

```cpp
{
          "header":
          {
            "expiration":"2011-10-11T23:59:59.9999999",
            "messageType":"User",
            "senderXuid":"123456789",
            "sender":"Striker",
            "sent":"2011-05-08T17:30:00Z",
            "hasAudio":false,
            "hasPhoto":false,
            "hasText":true
          },
        "messageText":"random user text up to 256 characters"
        }

```

#### <a name="error-response"></a>错误响应

发生错误，该服务可能会返回一个服务器对象，其中可能包含从该服务的环境的值。

| 属性| 类型| 说明|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 错误码| 字符串| 指示错误的来源。|
| 错误代码| int| 与 （可以为 null） 的错误相关联的数字代码。|
| errorMessage| 字符串| 如果配置为显示详细信息的错误的详细信息。|

<a id="ID4E3DAC"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4E5DAC"></a>


##### <a name="parent"></a>Parent 的子磁盘）  

[/users/xuid({xuid})/inbox](uri-usersxuidinbox.md)


<a id="ID4EMEAC"></a>


##### <a name="reference--standard-http-status-codesadditionalhttpstatuscodesmd"></a>参考[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)
