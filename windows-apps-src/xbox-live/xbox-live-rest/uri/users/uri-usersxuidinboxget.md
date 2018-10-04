---
title: GET (/users/xuid({xuid})/inbox)
assetID: c603910d-b430-f157-2634-ceddea89f2bd
permalink: en-us/docs/xboxlive/rest/uri-usersxuidinboxget.html
author: KevinAsgari
description: " GET (/users/xuid({xuid})/inbox)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 3d27ed6fa81bfd8618f19938c97a56361c16c009
ms.sourcegitcommit: 5c9a47b135c5f587214675e39c1ac058c0380f4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/04/2018
ms.locfileid: "4355335"
---
# <a name="get-usersxuidxuidinbox"></a>GET (/users/xuid({xuid})/inbox)
从服务检索指定的数量的用户消息摘要。
这些 Uri 的域是`msg.xboxlive.com`。

  * [备注](#ID4EV)
  * [URI 参数](#ID4EEB)
  * [查询字符串参数](#ID4EIC)
  * [授权](#ID4EGE)
  * [在资源的隐私设置的效果](#ID4ETE)
  * [HTTP 状态代码](#ID4E5E)
  * [JavaScript 对象表示法 (JSON) 响应](#ID4EMH)

<a id="ID4EV"></a>


## <a name="remarks"></a>备注

用户的消息摘要包含仅邮件主题。 对于用户生成的消息，这是当前的消息文本的前 20 个字符。 系统消息可能会提供其他使用者，如"LIVE System"。

发送; 顺序的相反中返回的消息也就是说，首先返回较新消息。

此 API 支持仅内容类型是"application/json"，这必需的每个调用的 HTTP 标头。

<a id="ID4EEB"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 类型| 描述|
| --- | --- | --- |
| xuid| 64 位无符号的整数| Xbox 用户 ID (XUID) 发出请求的玩家。|

<a id="ID4EIC"></a>


## <a name="query-string-parameters"></a>查询字符串参数

| 属性| 类型| 最大长度| 备注|
| --- | --- | --- | --- | --- | --- | --- |
| maxItems| int| 100| 返回的邮件的最大数量。|
| ContinuationToken| 字符串|  | 以前的枚举调用; 中返回的字符串使用继续枚举。|
| skipItems| int| 100| 消息以跳过; 数忽略 continuationToken 是否存在。|

<a id="ID4EGE"></a>


## <a name="authorization"></a>授权

你必须具有用户声明检索用户消息摘要。

<a id="ID4ETE"></a>


## <a name="effect-of-privacy-settings-on-resource"></a>在资源的隐私设置的效果

仅可以枚举自己用户的消息。

<a id="ID4E5E"></a>


## <a name="http-status-codes"></a>HTTP 状态代码

该服务返回的状态代码之一此部分中使用此方法对此资源所做的请求的响应。 有关使用 Xbox Live 服务的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。

| 代码| 说明|
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| 请求已成功。|
| 400| 服务可能不理解格式不正确的请求。 通常无效参数。|
| 403| 为用户或服务不允许该请求。|
| 404| URI 中找不到有效的 XUID。|
| 409| 基础集合更改具体取决于传递延续令牌。|
| 416| 跳过的项数大于可用的项目数。|
| 500| 常规服务器端错误。|

<a id="ID4EMH"></a>


## <a name="javascript-object-notation-json-response"></a>JavaScript 对象表示法 (JSON) 响应

如果调用成功，该服务将返回采用 JSON 格式的结果数据。

| 属性| 类型| 最大长度| 备注|
| --- | --- | --- | --- |
| 结果| 消息]| 100| 用户消息的数组|
| pagingInfo| PagingInfo|  | 当前的结果集的页面信息|

#### <a name="message"></a>消息

| 属性| 类型| 最大长度| 备注|
| --- | --- | --- | --- |
| 标题| 标题|  | 用户消息标头|
| messageSummary| 字符串| 20| UTF-8;通常首先 20 个字符的消息|

#### <a name="header"></a>标题

| 属性| 类型| 最大长度| 备注|
| --- | --- | --- | --- |
| id| 字符串| 50| 消息标识符，用于检索消息详细信息或删除邮件。|
| isRead| Bool|  | 标志，指示用户具有已阅读消息详细信息。|
| 发送| DateTime|  | 发送消息 UTC 日期和时间。 （由该服务）。|
| 到期| DateTime|  | UTC 过期日期和时间消息。 （所有消息都具有最大生存时间，以在将来确定）。|
| 可以忽略 messageType| 字符串| 50| 消息类型： 用户、 系统、 FriendRequest、 视频、 QuickChat、 VideoChat、 PartyChat、 标题、 GameInvite。|
| senderXuid| ulong|  | 发件人的 XUID。|
| 发送方| 字符串| 15| 发件人的玩家代号。|
| hasAudio| Bool|  | 该消息是否音频 （语音） 附件。|
| hasPhoto| Bool|  | 是否消息包含照片附件。|
| hasText| Bool|  | 是否消息包含文本。|

#### <a name="paging-info"></a>分页信息

| 属性| 类型| 最大长度| 备注|
| --- | --- | --- | --- |
| ContinuationToken| 字符串| 100| （可选） 返回的服务器。 允许更高版本的调用，以继续枚举。|
| totalItems| int|  | 收件箱中的消息的总数。|

#### <a name="sample-response"></a>示例响应

```cpp
{
          "results":
          [
            {
              "header":
              {
                "expiration":"2011-10-11T23:59:59.9999999",
                "id":"opaqueBlobOfText",
                "messageType":"User",
                "isRead":false,
                "senderXuid":"123456789",
                "sender":"Striker",
                "sent":"2011-05-08T17:30:00Z",
                "hasAudio":false,
                "hasPhoto":false,
                "hasText":true
              },
            "messageSummary":"first 20 chars"
          },
          ...
        ],
        "pagingInfo":
          {
          "continuationToken":"opaqueBlobOfText"
          "totalItems":5,
          }
        }

```

#### <a name="error-response"></a>错误响应

如果错误，该服务可能会返回服务器对象，其中可能包含从该服务环境的值。

| 属性| 类型| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 错误码| 字符串| 指示错误的来源。|
| 错误代码| int| 与 （可以为 null） 的错误相关联的数字代码。|
| errorMessage| 字符串| 如果配置为显示详细信息的错误的详细信息。|

<a id="ID4EIKAC"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4EKKAC"></a>


##### <a name="parent"></a>Parent 的子磁盘）  

[/users/xuid({xuid})/inbox](uri-usersxuidinbox.md)


<a id="ID4EWKAC"></a>


##### <a name="reference--standard-http-status-codesadditionalhttpstatuscodesmd"></a>参考[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)
