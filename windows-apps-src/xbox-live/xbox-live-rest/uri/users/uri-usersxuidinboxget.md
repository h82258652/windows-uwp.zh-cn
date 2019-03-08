---
title: GET (/users/xuid({xuid})/inbox)
assetID: c603910d-b430-f157-2634-ceddea89f2bd
permalink: en-us/docs/xboxlive/rest/uri-usersxuidinboxget.html
description: " GET (/users/xuid({xuid})/inbox)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 05f75510f15f6e6c5f1b1673673428c00f7a6c16
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632242"
---
# <a name="get-usersxuidxuidinbox"></a>GET (/users/xuid({xuid})/inbox)
从服务中检索指定的数量的用户消息摘要。
这些 Uri 的域是`msg.xboxlive.com`。

  * [备注](#ID4EV)
  * [URI 参数](#ID4EEB)
  * [查询字符串参数](#ID4EIC)
  * [Authorization](#ID4EGE)
  * [资源上的隐私设置的效果](#ID4ETE)
  * [HTTP 状态代码](#ID4E5E)
  * [JavaScript 对象表示法 (JSON) 响应](#ID4EMH)

<a id="ID4EV"></a>


## <a name="remarks"></a>备注

用户消息摘要包含仅消息使用者。 对于用户生成的消息，这是当前的消息文本的前 20 个字符。 系统消息可能提供了另一个使用者，例如"LIVE System"。

发送; 的顺序相反的顺序返回消息也就是说，较新的消息首先返回。

此 API 支持只包含内容类型为"application/json"; 所需的每个调用的 HTTP 标头中。

<a id="ID4EEB"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- |
| xuid| 64 位无符号的整数| Xbox 用户 ID (XUID) 正在发出请求的播放器。|

<a id="ID4EIC"></a>


## <a name="query-string-parameters"></a>查询字符串参数

| 属性| 在任务栏的搜索框中键入| 最大长度| 备注|
| --- | --- | --- | --- | --- | --- | --- |
| maxItems| int| 100| 返回的消息的最大数目。|
| ContinuationToken| 字符串|  | 在上一枚举调用; 返回的字符串用于继续枚举。|
| skipItems| int| 100| 要跳过; 的消息数忽略 continuationToken 是否存在。|

<a id="ID4EGE"></a>


## <a name="authorization"></a>授权

您必须具有用户声明来检索用户消息摘要。

<a id="ID4ETE"></a>


## <a name="effect-of-privacy-settings-on-resource"></a>资源上的隐私设置的效果

仅可以枚举用户消息。

<a id="ID4E5E"></a>


## <a name="http-status-codes"></a>HTTP 状态代码

服务将返回其中一个状态代码在本部分中使用此方法在此资源上发出的请求的响应中。 有关与 Xbox Live 服务一起使用的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。

| 代码| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| 请求已成功。|
| 400| 服务无法理解请求格式不正确。 通常是一个无效的参数。|
| 403| 为用户或服务不允许该请求。|
| 404| 在 URI 中找不到有效 XUID。|
| 409| 根据传递的继续标记更改基础集合。|
| 416| 若要跳过的项目数大于可用的项目数。|
| 500| 常规服务器端错误。|

<a id="ID4EMH"></a>


## <a name="javascript-object-notation-json-response"></a>JavaScript 对象表示法 (JSON) 响应

如果调用成功，服务将以 JSON 格式返回结果数据。

| 属性| 在任务栏的搜索框中键入| 最大长度| 备注|
| --- | --- | --- | --- |
| 结果| Message[]| 100| 用户消息的数组|
| pagingInfo| PagingInfo|  | 当前结果集的分页信息|

#### <a name="message"></a>消息

| 属性| 在任务栏的搜索框中键入| 最大长度| 备注|
| --- | --- | --- | --- |
| 标题| 标头|  | 用户消息标头|
| messageSummary| 字符串| 20| UTF-8;通常前 20 个字符的消息|

#### <a name="header"></a>标头

| 属性| 在任务栏的搜索框中键入| 最大长度| 备注|
| --- | --- | --- | --- |
| id| 字符串| 50| 消息标识符，用于检索消息的详细信息或删除消息。|
| isRead| 布尔|  | 标志，指示用户已经读取消息的详细信息。|
| 发送| DateTime|  | 发送消息的 UTC 日期和时间。 （由服务提供。）|
| 过期| DateTime|  | 消息过期的 UTC 日期和时间。 （所有消息都必须在将来确定的最大生存期。）|
| messageType| 字符串| 50| 消息类型：用户、 系统、 FriendRequest、 视频、 QuickChat、 VideoChat、 PartyChat、 标题、 GameInvite。|
| senderXuid| ulong|  | 发件人的 XUID。|
| 发送程序| 字符串| 15| 发件人的玩家代号。|
| hasAudio| 布尔|  | 是否将消息具有音频 （语音） 附件。|
| hasPhoto| 布尔|  | 是否将消息具有照片附件。|
| hasText| 布尔|  | 是否将消息包含文本。|

#### <a name="paging-info"></a>分页信息

| 属性| 在任务栏的搜索框中键入| 最大长度| 备注|
| --- | --- | --- | --- |
| ContinuationToken| 字符串| 100| （可选） 返回的服务器。 允许更高版本调用，若要继续枚举。|
| totalItems| int|  | 收件箱中的消息的总数。|

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

如果出现错误，该服务可能返回 errorResponse 对象，其中可能包含服务的环境中的值。

| 属性| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| errorSource| 字符串| 指示错误源于何处。|
| errorCode| int| 与错误 （可以为 null） 关联的数值代码。|
| errorMessage| 字符串| 如果配置为显示详细信息的错误的详细信息。|

<a id="ID4EIKAC"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4EKKAC"></a>


##### <a name="parent"></a>Parent 的子磁盘）  

[/users/xuid({xuid})/inbox](uri-usersxuidinbox.md)


<a id="ID4EWKAC"></a>


##### <a name="reference--standard-http-status-codesadditionalhttpstatuscodesmd"></a>引用[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)
