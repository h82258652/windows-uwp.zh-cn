---
title: GET (/users/xuid({xuid})/inbox/{messageId})
assetID: d76563d0-2c74-0308-054b-762c80392a02
permalink: en-us/docs/xboxlive/rest/uri-usersxuidinboxmessageidget.html
description: " GET (/users/xuid({xuid})/inbox/{messageId})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 29b4c57468148a431a10e0d74f85d360ff0992b3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618052"
---
# <a name="get-usersxuidxuidinboxmessageid"></a>GET (/users/xuid({xuid})/inbox/{messageId})
检索特定用户消息，将其标记为已读对服务的详细的消息文本。
这些 Uri 的域是`msg.xboxlive.com`。

  * [备注](#ID4EV)
  * [URI 参数](#ID4EEB)
  * [Authorization](#ID4ERB)
  * [请求正文](#ID4E3B)
  * [资源上的隐私设置的效果](#ID4EJC)
  * [HTTP 状态代码](#ID4EUC)
  * [JavaScript 对象表示法 (JSON) 响应](#ID4EUE)

<a id="ID4EV"></a>


## <a name="remarks"></a>备注

只能对用户、 系统和 FriendRequest 消息类型执行 get 操作。

此 URI 需要 Xbox.com 上的刷新。 目前，Xbox 360 不会更新读/未读状态，直到用户注销后再重新登录。

此 API 支持只包含内容类型为"application/json"; 所需的每个调用的 HTTP 标头中。

<a id="ID4EEB"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- |
| xuid | 64 位无符号的整数 | Xbox 用户 ID (XUID) 正在发出请求的播放器。 |
| messageId | string[50] | 正在检索或删除的消息的 ID。 |

<a id="ID4ERB"></a>


## <a name="authorization"></a>授权

您必须具有用户声明以检索用户的消息。

<a id="ID4E3B"></a>


## <a name="request-body"></a>请求正文

此请求的正文中不发送的任何对象。

<a id="ID4EJC"></a>


## <a name="effect-of-privacy-settings-on-resource"></a>资源上的隐私设置的效果

仅可以检索用户消息。

<a id="ID4EUC"></a>


## <a name="http-status-codes"></a>HTTP 状态代码

服务将返回其中一个状态代码在本部分中使用此方法在此资源上发出的请求的响应中。 有关与 Xbox Live 服务一起使用的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。

| 代码| 描述|
| --- | --- | --- | --- | --- |
| 200| 成功。|
| 400| 不能正确转换 XUID。|
| 403| 不能转换 XUID 或找不到有效的 XUID 声明。|
| 404| 有效 XUID 缺失，或消息 ID 无法找到或解析不正确。|
| 500| 常规服务器端错误或消息类型不能用于 GET。|

<a id="ID4EUE"></a>


## <a name="javascript-object-notation-json-response"></a>JavaScript 对象表示法 (JSON) 响应

如果调用成功，服务将以 JSON 格式返回结果数据。 根对象是 UserMessageHeader 对象。

#### <a name="usermessageheader"></a>UserMessageHeader

| 属性| 在任务栏的搜索框中键入| 最大长度| 备注|
| --- | --- | --- | --- |
| 标题| 标头|  | JSON 对象|
| messageText| 字符串| 256| UTF-8|

#### <a name="header"></a>标头

| 属性| 在任务栏的搜索框中键入| 最大长度| 备注|
| --- | --- | --- | --- |
| 发送| DateTime|  | 日期和时间已发送消息。 （由服务提供。）|
| 过期| DateTime|  | 过期日期和时间的消息。 （所有消息都必须在将来确定的最大生存期。）|
| messageType| 字符串| 13| 消息类型：用户、 系统，FriendRequest。|
| senderXuid| ulong|  | 发件人的 XUID。|
| 发送程序| 字符串| 15| 发件人的玩家代号。|
| hasAudio| 布尔|  | 是否将消息具有音频 （语音） 附件。|
| hasPhoto| 布尔|  | 是否将消息具有照片附件。|
| hasText| 布尔|  | 是否将消息包含文本。|

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

如果出现错误，该服务可能返回 errorResponse 对象，其中可能包含服务的环境中的值。

| 属性| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| errorSource| 字符串| 指示错误源于何处。|
| errorCode| int| 与错误 （可以为 null） 关联的数值代码。|
| errorMessage| 字符串| 如果配置为显示详细信息的错误的详细信息。|

<a id="ID4E3DAC"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4E5DAC"></a>


##### <a name="parent"></a>Parent 的子磁盘）  

[/users/xuid({xuid})/inbox](uri-usersxuidinbox.md)


<a id="ID4EMEAC"></a>


##### <a name="reference--standard-http-status-codesadditionalhttpstatuscodesmd"></a>引用[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)
