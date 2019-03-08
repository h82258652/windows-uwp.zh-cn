---
title: GET (/users/xuid({xuid})/scids/{scid}/stats)
assetID: af117e87-6f1d-6448-9adf-7cf890d1380f
permalink: en-us/docs/xboxlive/rest/uri-usersxuidscidsscidstatsget.html
description: " GET (/users/xuid({xuid})/scids/{scid}/stats)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: baf965dcbd23bf00d7d0953726f9f20852324e5e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662382"
---
# <a name="get-usersxuidxuidscidsscidstats"></a>GET (/users/xuid({xuid})/scids/{scid}/stats)
获取代表指定用户的用户统计信息名称的逗号分隔列表的范围扩展的服务配置。
这些 Uri 的域是`userstats.xboxlive.com`。

  * [备注](#ID4EV)
  * [URI 参数](#ID4EEB)
  * [查询字符串参数](#ID4EPB)
  * [Authorization](#ID4EUC)
  * [所需的请求标头](#ID4EPD)
  * [可选的请求标头](#ID4EYE)
  * [请求正文](#ID4E3F)
  * [HTTP 状态代码](#ID4EHG)
  * [响应正文](#ID4E5BAC)

<a id="ID4EV"></a>


## <a name="remarks"></a>备注

客户端需要读取和播放机代表标题的统计信息写入新的播放机统计信息系统的方法。

<a id="ID4EEB"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- |
| xuid| GUID| Xbox 用户 ID (XUID) 的用户的名义访问服务配置。|
| scid| GUID| 服务配置，其中包含要访问的资源的标识符。|

<a id="ID4EPB"></a>

 
## <a name="query-string-parameters"></a>查询字符串参数

| 参数| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- | --- | --- | --- |
| statNames| 字符串| 唯一查询字符串参数是逗号分隔用户统计信息名称 URI 名词。例如，以下 URI 告知服务，请求 URI 中指定的用户 id 代表的四个统计信息。 `https://userstats.xboxlive.com/users/xuid({xuid})/scids/{scid}/stats/wins,kills,kdratio,headshots`将可以在单个调用中，请求的统计信息数限制，该限制将仔细考虑"优势"开发人员方便 vs。URI 长度实用性。 例如，限制可能会通过必要的统计信息名称文本 （包括逗号），任一 600 个字符或最大 10 个统计信息来确定。 启用此类简单的 GET 启用 HTTP 缓存对于常见的请求统计信息，可减小到常用的客户端呼叫量。 |

<a id="ID4EUC"></a>


## <a name="authorization"></a>授权

没有为内容隔离和访问控制方案而实现的授权逻辑。

   * 可以从任何平台上的客户端读取排行榜和用户统计信息，前提是调用方将提交与请求的有效 XSTS 令牌。 写入是很明显限制为客户端支持的数据平台。
   * 标题开发人员可以将统计信息标记为开放或受限 XDP 或合作伙伴中心。 排行榜是打开的统计信息。 打开统计信息可以通过 Smartglass，以及 iOS、 Android、 Windows、 Windows Phone 和 web 应用程序访问，只要用户授权到沙盒。 用户授权给沙盒通过 XDP 或合作伙伴中心管理。

用于检查的伪代码如下所示：


```cpp
If (!checkAccess(serviceConfigId, resource, CLAIM[userid, deviceid, titleid]))
{
        Reject request as Unauthorized
}

// else accept request.

```


<a id="ID4EPD"></a>


## <a name="required-request-headers"></a>所需的请求标头

| 标头| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 授权| 字符串| HTTP 身份验证的身份验证凭据。 示例值："XBL3.0 x =&lt;userhash >;&lt;令牌 >"。|

<a id="ID4EYE"></a>


## <a name="optional-request-headers"></a>可选的请求标头

| 标头| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X-RequestedServiceVersion|  | 生成此请求应定向到服务的名称/编号。 请求将只路由到的服务验证标头身份验证令牌中声明的有效性后，依次类推。 默认值：1.|

<a id="ID4E3F"></a>


## <a name="request-body"></a>请求正文

此请求的正文中不发送的任何对象。

<a id="ID4EHG"></a>


## <a name="http-status-codes"></a>HTTP 状态代码

服务将返回其中一个状态代码在本部分中使用此方法在此资源上发出的请求的响应中。 有关与 Xbox Live 服务一起使用的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。

| 代码| 原因短语| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| 确定| 已成功检索该会话。|
| 304| 未修改| 资源不被修改自最后一次请求。|
| 400| 无效的请求| 服务无法理解请求格式不正确。 通常是一个无效的参数。|
| 401| 未经授权| 请求需要用户身份验证。|
| 403| 已禁止| 为用户或服务不允许该请求。|
| 404| 未找到| 找不到指定的资源。|
| 406| 不可接受| 不支持资源版本。|
| 408| 请求超时| 不支持资源版本;应拒绝的 MVC 层。|

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
