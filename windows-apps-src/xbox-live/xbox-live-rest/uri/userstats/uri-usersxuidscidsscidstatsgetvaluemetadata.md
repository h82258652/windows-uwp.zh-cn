---
title: GET (/users/xuid({xuid})/scids/{scid}/stats?include=valuemetadata)
assetID: 890e3f93-4fdc-955f-d849-ba9579d5c1eb
permalink: en-us/docs/xboxlive/rest/uri-usersxuidscidsscidstatsgetvaluemetadata.html
author: KevinAsgari
description: " GET (/users/xuid({xuid})/scids/{scid}/stats?include=valuemetadata)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 08cbb5710a2bcafc1edfdf78a8f96561961ef8cd
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/01/2018
ms.locfileid: "5872550"
---
# <a name="get-usersxuidxuidscidsscidstatsincludevaluemetadata"></a>GET (/users/xuid({xuid})/scids/{scid}/stats?include=valuemetadata)
获取指定的统计数据，包括与统计信息值，指定的服务配置中的用户相关联的元数据的列表。
这些 Uri 的域是`userstats.xboxlive.com`。

  * [备注](#ID4EV)
  * [URI 参数](#ID4EAB)
  * [查询字符串参数](#ID4ELB)
  * [授权](#ID4EWC)
  * [需的请求标头](#ID4ERD)
  * [可选的请求标头](#ID4EDF)
  * [请求正文](#ID4EHG)
  * [HTTP 状态代码](#ID4ESG)
  * [响应正文](#ID4EJCAC)

<a id="ID4EV"></a>


## <a name="remarks"></a>备注

？ 包括 = valuemetadata 查询参数允许该响应包含任何关联的用户统计数据值，如的模型和颜色的汽车用于实现跑道上的某个时间使用元数据。

若要在响应中包含值元数据，请求调用还必须将标头值 X Xbl 协定版本为 3。

<a id="ID4EAB"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 类型| 说明|
| --- | --- | --- |
| xuid| GUID| Xbox 用户 ID (XUID) 的用户的名义访问的服务配置。|
| scid| GUID| 服务配置，其中包含所访问的资源的标识符。|

<a id="ID4ELB"></a>


## <a name="query-string-parameters"></a>查询字符串参数

| 参数| 类型| 说明|
| --- | --- | --- | --- | --- | --- |
| statNames| 字符串| 以逗号分隔用户统计数据名称的列表。例如，以下 URI 通知服务 URI 中指定的用户 id 代表请求的四个统计信息。{:: nomakrdown}<br/><br/>`https://userstats.xboxlive.com/users/xuid({xuid})/scids/{scid}/stats/wins,kills,kdratio,headshots?include=valuemetadata`| 
| 包括 = valuemetadata| 字符串| 指示该响应包括与 uset 统计信息值关联的任何值元数据。|

<a id="ID4EWC"></a>


## <a name="authorization"></a>授权

没有针对内容隔离和访问控制方案实现的授权逻辑。

   * 假设调用方提交请求有效的 XSTS 令牌，可以从任何平台上的客户端读取排行榜和用户统计信息。 写入仅限于受数据平台的客户端。
   * 游戏开发人员可以将统计数据标记为打开或 XDP 或开发人员中心使用限制。 排行榜是开放的统计信息。 打开统计信息可访问 Smartglass，以及 iOS、 Android、 Windows、 Windows Phone 和 web 应用程序，只要用户有权访问沙盒。 通过 XDP 或开发人员中心管理到沙盒的用户身份验证。

检查伪代码如下所示：


```cpp
If (!checkAccess(serviceConfigId, resource, CLAIM[userid, deviceid, titleid]))
{
        Reject request as Unauthorized
}

// else accept request.

```


<a id="ID4ERD"></a>


## <a name="required-request-headers"></a>需的请求标头

| 标头| 类型| 说明|
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 授权| 字符串| HTTP 身份验证的身份验证凭据。 示例值:"XBL3.0 x =&lt;userhash >;&lt;令牌 >"。|
| X Xbl 协定版本| 字符串| 指示哪个版本的要使用的 API。 若要在响应中包含值元数据，此值必须设置为"3"。|

<a id="ID4EDF"></a>


## <a name="optional-request-headers"></a>可选的请求标头

| 标头| 类型| 说明|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X RequestedServiceVersion|  | 名称/的内部版本号应指向此请求的服务。 请求将仅可路由到的服务验证该标头，身份验证令牌中的声明的有效性后，依此类推。 默认值： 1。|

<a id="ID4EHG"></a>


## <a name="request-body"></a>请求正文

此请求的正文中不发送任何对象。

<a id="ID4ESG"></a>


## <a name="http-status-codes"></a>HTTP 状态代码

此部分中使用此方法对此资源所做的请求的响应，该服务返回其中一个状态代码。 有关使用 Xbox Live 服务的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。

| 代码| 原因短语| 说明|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| “确定”| 已成功检索会话。|
| 304| 未修改| 资源未修改由于最后一次请求。|
| 400| 错误请求| 服务可能不理解格式不正确的请求。 通常无效参数。|
| 401| 未授权| 请求要求用户身份验证。|
| 403| 已禁止| 为用户或服务不允许该请求。|
| 404| 找不到| 找不到指定的资源。|
| 406| 不允许| 不支持资源版本。|
| 408| 请求超时| 资源版本不受支持;应拒绝 MVC 层。|

<a id="ID4EJCAC"></a>


## <a name="response-body"></a>响应正文

<a id="ID4EPCAC"></a>


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
        "value": 40,
        "valuemetadata" : "{\"region\" : \"EU\", \"isRanked\" : true}"
      },
      {
        "statname": "Kills",
        "type": "Integer",
        "value": 700,
        "valuemetadata" : "{\"longestKillStreak" : 15, \"favoriteTarget\" : \"CrazyPigeon\"}"
      },
      {
        "statname": "KDRatio",
        "type": "Double",
        "value": 2.23,
        "valuemetadata" : "{\"totalKills\" : 700, \"totalDeaths\" : 314}"
      },
      {
        "statname": "Headshots",
        "type": "Integer",
        "value": 173,
        "valuemetadata" : ""
      }
    ],
  }
}

```


<a id="ID4EZCAC"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4E2CAC"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[/users/xuid({xuid})/scids/{scid}/stats](uri-usersxuidscidsscidstats.md)
