---
title: GET (/users/xuid({xuid})/scids/{scid}/stats)
assetID: af117e87-6f1d-6448-9adf-7cf890d1380f
permalink: en-us/docs/xboxlive/rest/uri-usersxuidscidsscidstatsget.html
author: KevinAsgari
description: " GET (/users/xuid({xuid})/scids/{scid}/stats)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: ed96418141aec049a9577924597a07da4313b7e2
ms.sourcegitcommit: 232543fba1fb30bb1489b053310ed6bd4b8f15d5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/25/2018
ms.locfileid: "4175511"
---
# <a name="get-usersxuidxuidscidsscidstats"></a>GET (/users/xuid({xuid})/scids/{scid}/stats)
获取由逗号分隔的代表指定用户的用户统计信息名称列表范围的服务配置。
这些 Uri 的域是`userstats.xboxlive.com`。

  * [备注](#ID4EV)
  * [URI 参数](#ID4EEB)
  * [查询字符串参数](#ID4EPB)
  * [授权](#ID4EUC)
  * [需的请求标头](#ID4EPD)
  * [可选的请求标头](#ID4EYE)
  * [请求正文](#ID4E3F)
  * [HTTP 状态代码](#ID4EHG)
  * [响应正文](#ID4E5BAC)

<a id="ID4EV"></a>


## <a name="remarks"></a>备注

客户端需要一种方法向读取和写入标题统计信息玩家代表我们的新玩家统计信息系统。

<a id="ID4EEB"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 类型| 说明|
| --- | --- | --- |
| xuid| GUID| Xbox 用户 ID (XUID) 的用户的名义访问服务配置。|
| scid| GUID| 服务配置，其中包含正在访问的资源的标识符。|

<a id="ID4EPB"></a>

 
## <a name="query-string-parameters"></a>查询字符串参数

| 参数| 类型| 说明|
| --- | --- | --- | --- | --- | --- |
| statNames| 字符串| 唯一的查询字符串参数是用逗号分隔用户统计数据名称 URI 名词。例如，以下 URI 通知服务 URI 中指定的用户 id 代表请求的四个统计信息。 `https://userstats.xboxlive.com/users/xuid({xuid})/scids/{scid}/stats/wins,kills,kdratio,headshots`将可以在单个调用中，请求的统计信息的数量限制，该限制将仔细考虑"批处理"为开发人员方便起见，与 URI 长度适用范围。 例如，限制可能决定通过值得的统计数据名称文本 （包括逗号分隔），无论 600 个字符或最大 10 统计信息。 启用所示的简单获取启用 HTTP 缓存常用的请求统计信息，减少从聊天客户端调用卷。 |

<a id="ID4EUC"></a>


## <a name="authorization"></a>授权

没有针对内容隔离和访问控制方案实现的授权逻辑。

   * 排行榜和用户统计信息可以读取的所有平台上的客户端，前提是调用方提交与请求有效的 XSTS 令牌。 写入进行显然限于数据平台支持的客户端。
   * 游戏开发人员可以将统计数据标记为打开或 XDP 或开发人员中心使用限制。 排行榜是打开统计信息。 打开统计信息可以访问 Smartglass，以及 iOS、 Android、 Windows、 Windows Phone 和 web 应用程序，只要用户有权沙盒。 通过 XDP 或开发人员中心管理到沙盒的用户身份验证。

检查伪代码如下所示：


```cpp
If (!checkAccess(serviceConfigId, resource, CLAIM[userid, deviceid, titleid]))
{
        Reject request as Unauthorized
}

// else accept request.

```


<a id="ID4EPD"></a>


## <a name="required-request-headers"></a>需的请求标头

| 标头| 类型| 说明|
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 授权| 字符串| HTTP 身份验证的身份验证凭据。 示例值:"XBL3.0 x =&lt;userhash >;&lt;令牌 >"。|

<a id="ID4EYE"></a>


## <a name="optional-request-headers"></a>可选的请求标头

| 标头| 类型| 说明|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X RequestedServiceVersion|  | 生成此请求应定向到该服务的名称/数。 请求将仅可路由到的服务验证该标头，身份验证令牌中的声明的有效性后，依此类推。 默认值： 1。|

<a id="ID4E3F"></a>


## <a name="request-body"></a>请求正文

此请求的正文中不发送任何对象。

<a id="ID4EHG"></a>


## <a name="http-status-codes"></a>HTTP 状态代码

此部分中使用此方法对此资源所做的请求的响应，该服务返回的状态代码之一。 有关使用 Xbox Live 服务的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。

| 代码| 原因短语| 说明|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| “确定”| 已成功检索会话。|
| 304| 未修改| 资源不已修改由于最后一次请求。|
| 400| 错误请求| 服务可能不理解格式不正确的请求。 通常无效参数。|
| 401| 未授权| 请求要求用户身份验证。|
| 403| 已禁止| 为用户或服务不允许该请求。|
| 404| 找不到| 找不到指定的资源。|
| 406| 不允许| 不支持资源版本。|
| 408| 请求超时| 资源版本不受支持;应拒绝 MVC 层。|

<a id="ID4E5BAC"></a>


## <a name="response-body"></a>响应正文

<a id="ID4EECAC"></a>


### <a name="sample-response"></a>示例响应


```cpp
{
    "user": {
    "xuid": "123456789",
        "gamertag": "WarriorSaint",
        "stats": [
            {
                "statname": "Wins",
                "type": "Integer",
                "value": 40
            },
            {
                "statname": "Kills",
                "type": "Integer",
                "value": 700
            },
            {
                "statname": "KDRatio",
                "type": "Double",
                "value": 2.23
            },
            {
                "statname": "Headshots",
                "type": "Integer",
                "value": 173
            }
        ],
    }
}

```


<a id="ID4EOCAC"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4EQCAC"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[/users/xuid({xuid})/scids/{scid}/stats](uri-usersxuidscidsscidstats.md)
