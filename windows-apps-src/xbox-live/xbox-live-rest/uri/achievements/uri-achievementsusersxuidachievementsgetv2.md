---
title: GET (/users/xuid({xuid})/achievements)
assetID: 381d49d1-7a4b-4a1e-1baf-cf674f7e0d54
permalink: en-us/docs/xboxlive/rest/uri-achievementsusersxuidachievementsgetv2.html
description: " GET (/users/xuid({xuid})/achievements)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 720170e88808db7ef95b88896fbca4f1cda4a091
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655262"
---
# <a name="get-usersxuidxuidachievements"></a>GET (/users/xuid({xuid})/achievements)
获取成就上标题、 取消锁定的用户，或有正在进行中的用户定义的列表。 这些 Uri 的域是`achievements.xboxlive.com`。
 
  * [URI 参数](#ID4EX)
  * [查询字符串参数](#ID4ECB)
  * [Authorization](#ID4ENF)
  * [所需的请求标头](#ID4ESG)
  * [可选的请求标头](#ID4ESH)
  * [请求正文](#ID4EIBAC)
  * [响应正文](#ID4ETBAC)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| xuid| 64 位无符号的整数| Xbox 用户 ID (XUID) 的用户正在访问的 （资源）。 必须与匹配身份验证的用户的 XUID。| 
  
<a id="ID4ECB"></a>

 
## <a name="query-string-parameters"></a>查询字符串参数
 
| 参数| 必需| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | --- | --- | --- | 
| <b>skipItems</b>| 否| 32 位有符号的整数| 返回从给定数目的项后开始的项。 例如， <b>skipItems ="3"</b>将检索项的第四项开头检索。 | 
| <b>continuationToken</b>| 否| 字符串| 返回从给定的继续标记处开始的项。 | 
| <b>maxItems</b>| 否| 32 位有符号的整数| 要从集合，可以结合返回的项数的最大数目<b>skipItems</b>并<b>continuationToken</b>要返回的项的范围。 该服务可能会提供默认值，如果<b>maxItems</b>不存在，并且可能返回少于<b>maxItems</b>，即使尚未返回结果的最后一页。 | 
| <b>titleId</b>| 否| 字符串| 用于返回的结果的筛选器。 接受一个或多个以逗号分隔的十进制标题标识符。| 
| <b>unlockedOnly</b>| 否| 布尔值| 返回的结果的筛选器。 如果设置为<b>，则返回 true</b>，将只返回为用户解锁的成就。 默认情况下<b>false</b>。| 
| <b>possibleOnly</b>| 否| 布尔值| 返回的结果的筛选器。 如果设置为<b>，则返回 true</b>，将返回所有可能的结果，但不是只是解除锁定元数据-从 XMS 的成就信息。 默认情况下<b>false</b>。| 
| <b>types</b>| 否| 字符串| 用于返回的结果的筛选器。 可以是"Persistent"质询"。 默认值为所有支持的类型。| 
| <b>orderBy</b>| 否| 字符串| 指定用来返回结果的顺序。 可以是"无序"、"Title"、"UnlockTime"或"EndingSoon"。 默认值为"无序"。| 
  
<a id="ID4ENF"></a>

 
## <a name="authorization"></a>授权
 
| 声明| 是否为必需？| 描述| 如果缺少的行为| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 用户| 调用方是 Xbox LIVE 的授权的用户。| 调用方必须是 Xbox LIVE 上的有效用户。| 403 禁止访问| 
  
<a id="ID4ESG"></a>

 
## <a name="required-request-headers"></a>所需的请求标头
 
| 标头| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 授权| 字符串| HTTP 身份验证的身份验证凭据。 示例值："XBL3.0 x =&lt;userhash >;&lt;令牌 >"。| 
  
<a id="ID4ESH"></a>

 
## <a name="optional-request-headers"></a>可选的请求标头
 
| 标头| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| <b>X-RequestedServiceVersion</b>| 字符串| 生成此请求应定向到 Xbox LIVE 的服务的名称/编号。 验证标头中的身份验证令牌等的声明的有效性后仅为将请求路由到该服务。默认值：1.| 
| <b>x-xbl-contract-version</b>| 32 位无符号的整数| 如果存在，并且设置为 2，就将使用此 API 的 V2 版本。 否则为 V1。| 
| <b>Accept-Language</b>| 字符串| 所需的区域设置和回退 （例如，-FR、 fr、 EN-GB、 en WW、 EN-US） 的列表。 获得的成就服务将共同完成列表，直到找到匹配的已本地化的字符串。 如果找不到，它尝试匹配来自用户的 IP 地址的用户标记中定义的位置。 如果找到仍没有匹配的本地化的字符串，它使用标题开发人员/发布者提供的默认字符串。 | 
  
<a id="ID4EIBAC"></a>

 
## <a name="request-body"></a>请求正文
 
此请求的正文中不发送的任何对象。
  
<a id="ID4ETBAC"></a>

 
## <a name="response-body"></a>响应正文
 
如果调用成功，该服务返回的数组[成就 (JSON)](../../json/json-achievementv2.md)对象和一个[PagingInfo (JSON)](../../json/json-paginginfo.md)对象。
 
<a id="ID4ECCAC"></a>

 
### <a name="sample-response"></a>示例响应
 

```cpp
{
    "achievements":
    [{
        "id":"3",
        "serviceConfigId":"b5dd9daf-0000-0000-0000-000000000000",
        "name":"Default NameString for Microsoft Achievements Sample Achievement 3",
        "titleAssociations":
        [{
                "name":"Microsoft Achievements Sample",
                "id":3051199919,
                "version":"abc"
        }],
        "progressState":"Achieved",
        "progression":
        {
                "achievementState":"Achieved",
                "requirements":null,
                "timeUnlocked":"2013-01-17T03:19:00.3087016Z",
        },
        "mediaAssets":
        [{
                "name":"Icon Name",
                "type":"Icon",
                "url":"https://www.xbox.com"
        }],
        "platform":"D",
        "isSecret":true,
        "description":"Default DescriptionString for Microsoft Achievements Sample Achievement 3",
        "lockedDescription":"Default UnachievedString for Microsoft Achievements Sample Achievement 3",
        "productId":"12345678-1234-1234-1234-123456789012",
        "achievementType":"Challenge",
        "participationType":"Individual",
        "timeWindow":
        {
                "startDate":"2013-02-01T00:00:00Z",
                "endDate":"2100-07-01T00:00:00Z"
        },
        "rewards":
        [{
                "name":null,
                "description":null,
                "value":"10",
                "type":"Gamerscore",
                "valueType":"Int"
        },
        {
                "name":"Default Name for InAppReward for Microsoft Achievements Sample Achievement 3",
                "description":"Default Description for InAppReward for Microsoft Achievements Sample Achievement 3",
                "value":"1",
                "type":"InApp",
                "valueType":"String"
        }],
        "estimatedTime":"06:12:14",
        "deeplink":"aWFtYWRlZXBsaW5r",
        "isRevoked":false
        }],
        "pagingInfo":
        {
                "continuationToken":null,
                "totalRecords":1
        }
}
         
```

   
<a id="ID4EPCAC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4ERCAC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[/users/xuid({xuid})/achievements](uri-achievementsusersxuidachievementsv2.md)

  
<a id="ID4E2CAC"></a>

 
##### <a name="reference"></a>参考 

[成就 (JSON)](../../json/json-achievementv2.md)

 [PagingInfo (JSON)](../../json/json-paginginfo.md)

 [分页参数](../../additional/pagingparameters.md)

   