---
title: GET (/users/xuid({xuid})/scids/{scid}/stats?include=valuemetadata)
assetID: 890e3f93-4fdc-955f-d849-ba9579d5c1eb
permalink: en-us/docs/xboxlive/rest/uri-usersxuidscidsscidstatsgetvaluemetadata.html
description: " GET (/users/xuid({xuid})/scids/{scid}/stats?include=valuemetadata)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 55ad44d4c29a2d7a43c76c4df2a78e08462fa65f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57612712"
---
# <a name="get-usersxuidxuidscidsscidstatsincludevaluemetadata"></a>GET (/users/xuid({xuid})/scids/{scid}/stats?include=valuemetadata)
获取指定的统计信息，包括与统计信息值，指定的服务配置中的用户相关联的元数据的列表。
这些 Uri 的域是`userstats.xboxlive.com`。

  * [备注](#ID4EV)
  * [URI 参数](#ID4EAB)
  * [查询字符串参数](#ID4ELB)
  * [Authorization](#ID4EWC)
  * [所需的请求标头](#ID4ERD)
  * [可选的请求标头](#ID4EDF)
  * [请求正文](#ID4EHG)
  * [HTTP 状态代码](#ID4ESG)
  * [响应正文](#ID4EJCAC)

<a id="ID4EV"></a>


## <a name="remarks"></a>备注

？ 包括 = valuemetadata 查询参数，响应包含任何统计信息的用户名的值，如模型和用于实现正轨争用时间一辆汽车的颜色与关联的元数据。

若要在响应中包含值的元数据，请求调用还必须为 3 设置标头值 X Xbl 协定版本。

<a id="ID4EAB"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- |
| xuid| GUID| Xbox 用户 ID (XUID) 的用户的名义访问服务配置。|
| scid| GUID| 服务配置，其中包含要访问的资源的标识符。|

<a id="ID4ELB"></a>


## <a name="query-string-parameters"></a>查询字符串参数

| 参数| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- | --- | --- | --- |
| statNames| 字符串| 逗号分隔的用户统计信息名称的列表。例如，以下 URI 告知服务，请求 URI 中指定的用户 id 代表的四个统计信息。{:: nomakrdown}<br/><br/>`https://userstats.xboxlive.com/users/xuid({xuid})/scids/{scid}/stats/wins,kills,kdratio,headshots?include=valuemetadata`| 
| include=valuemetadata| 字符串| 指示响应包含与请使用 stat 值相关联的任何值元数据。|

<a id="ID4EWC"></a>


## <a name="authorization"></a>授权

没有为内容隔离和访问控制方案而实现的授权逻辑。

   * 可以从任何平台上的客户端读取排行榜和用户统计信息，前提是调用方将提交与请求的有效 XSTS 令牌。 写入被限制为客户端支持的数据平台。
   * 标题开发人员可以将统计信息标记为开放或受限 XDP 或合作伙伴中心。 排行榜是打开的统计信息。 打开统计信息可以通过 Smartglass，以及 iOS、 Android、 Windows、 Windows Phone 和 web 应用程序访问，只要用户授权到沙盒。 用户授权给沙盒通过 XDP 或合作伙伴中心管理。

用于检查的伪代码如下所示：


```cpp
If (!checkAccess(serviceConfigId, resource, CLAIM[userid, deviceid, titleid]))
{
        Reject request as Unauthorized
}

// else accept request.

```


<a id="ID4ERD"></a>


## <a name="required-request-headers"></a>所需的请求标头

| 标头| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 授权| 字符串| HTTP 身份验证的身份验证凭据。 示例值："XBL3.0 x =&lt;userhash >;&lt;令牌 >"。|
| X-Xbl-Contract-Version| 字符串| 指示要使用的 API 版本。 若要在响应中包含值的元数据，此值必须设置为"3"。|

<a id="ID4EDF"></a>


## <a name="optional-request-headers"></a>可选的请求标头

| 标头| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X-RequestedServiceVersion|  | 生成此请求应定向到服务的名称/编号。 请求将只路由到的服务验证标头身份验证令牌中声明的有效性后，依次类推。 默认值：1.|

<a id="ID4EHG"></a>


## <a name="request-body"></a>请求正文

此请求的正文中不发送的任何对象。

<a id="ID4ESG"></a>


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
