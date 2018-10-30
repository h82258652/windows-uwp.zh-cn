---
title: POST (/users/xuid({xuid})/outbox)
assetID: de991d88-efe0-04f2-f6b2-0bc3e68bfd46
permalink: en-us/docs/xboxlive/rest/uri-usersxuidoutboxpost.html
author: KevinAsgari
description: " POST (/users/xuid({xuid})/outbox)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 86853a490ab407fa96d40c4b2e3deb42c5f346a3
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2018
ms.locfileid: "5746580"
---
# <a name="post-usersxuidxuidoutbox"></a>POST (/users/xuid({xuid})/outbox)
指定的消息发送到收件人的列表。
这些 Uri 的域是`msg.xboxlive.com`。

  * [备注](#ID4EV)
  * [URI 参数](#ID4EAB)
  * [授权](#ID4ENB)
  * [资源的隐私设置的效果](#ID4EYB)
  * [请求正文](#ID4E3F)
  * [HTTP 状态代码](#ID4ETCAC)
  * [响应正文](#ID4E1EAC)

<a id="ID4EV"></a>


## <a name="remarks"></a>备注

此 API 支持仅内容类型是"application/json"，这需要在每个调用的 HTTP 标头中。

<a id="ID4EAB"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 类型| 说明|
| --- | --- | --- |
| xuid | 64 位无符号的整数 | Xbox 用户 ID (XUID) 发出请求的玩家。 |

<a id="ID4ENB"></a>


## <a name="authorization"></a>授权

你必须具有用户声明和有效金会员订阅用户发送。

<a id="ID4EYB"></a>


## <a name="effect-of-privacy-settings-on-resource"></a>资源的隐私设置的效果

成功用户将消息发送到玩家，无论该玩家好友，会导致为 200 的结果代码。 但是，如果向已阻止你的任何人发送一条消息，接收者不会收到消息，并将不会收到你的消息未成功任何指示。

在多少消息可发送每日和多少好友和非好友，如下所示还存在限制。

   * 每条消息 20 陌生人
   * 200 陌生人 24 小时
   * 250 邮件总数 24 小时
   * 24 小时的收件人 2500年总数

| 请求的用户| 目标用户的隐私设置| 行为|
| --- | --- | --- | --- | --- | --- |
| 我| -| 所述。|
| 好友| 每个人都| 200 OK|
| 好友| 仅好友| 200 OK|
| 好友| 阻止| 200 OK|
| 非好友用户| 每个人都| 200 OK|
| 非好友用户| 仅好友| 200 OK|
| 非好友用户| 阻止| 200 OK|
| 第三方网站| 每个人都| 200 OK|
| 第三方网站| 仅好友| 200 OK|
| 第三方网站| 阻止| 200 OK|

<a id="ID4E3F"></a>


## <a name="request-body"></a>请求正文

| 属性| 类型| 最大长度| 使用者| 备注|
| --- | --- | --- | --- | --- |
| 标题| 标题|  | 全部| 用户消息标头|
| messageText| 字符串| 250| 除了 Windows 8 的所有平台| 用户消息文本 (utf-8)|

#### <a name="header"></a>标题

| 属性| 类型| 最大长度| 使用者| 备注|
| --- | --- | --- | --- | --- |
| 收件人| 用户]| 20| 全部| 邮件收件人的列表|

#### <a name="user"></a>用户

| 属性| 类型| 最大长度| 使用者| 备注|
| --- | --- | --- | --- | --- |
| xuid| ulong|  | 全部| 受邀的 XUID。 如果玩家代号，将会发送不使用。|
| 玩家代号| 字符串| 15| 全部| 受邀的玩家代号。 如果发送 XUID 不使用。|

#### <a name="sample-request-body"></a>示例请求正文 

```cpp
{
          "header":
          {
            "recipients":
            [{"gamertag":"GoTeamEmily"},
            {"gamertag":"Longstreet360"}]
          },
          "messageText":"random user text"
        }

```


<a id="ID4ETCAC"></a>


## <a name="http-status-codes"></a>HTTP 状态代码

此部分中使用此方法对此资源所做的请求的响应，该服务返回其中一个状态代码。 有关使用 Xbox Live 服务的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。

| 代码| 说明|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| 成功。|
| 400| 收件人列表为空或超过最大长度。指定的玩家代号和 XUID; 或或 messageText 太长。|
| 403| 不能转换 XUID。|
| 404| 玩家代号无效或找不到用户。|
| 409| 用户已到达由系统的每日限制。|
| 500| 常规服务器端错误。|

<a id="ID4E1EAC"></a>


## <a name="response-body"></a>响应正文

该响应正文中不发送任何对象。

<a id="ID4EJFAC"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4ELFAC"></a>


##### <a name="parent"></a>Parent 的子磁盘）  

[/users/xuid({xuid})/outbox](uri-usersxuidoutbox.md)


<a id="ID4EZFAC"></a>


##### <a name="reference--standard-http-status-codesadditionalhttpstatuscodesmd"></a>参考[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)
