---
title: GET (/users/xuid({xuid})/achievements)
assetID: 381d49d1-7a4b-4a1e-1baf-cf674f7e0d54
permalink: en-us/docs/xboxlive/rest/uri-achievementsusersxuidachievementsgetv2.html
author: KevinAsgari
description: " GET (/users/xuid({xuid})/achievements)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: ed26a509d75ea7b62705023b0e31850581adc66a
ms.sourcegitcommit: 68fcac3288d5698a13dbcbd57f51b30592f24860
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/19/2018
ms.locfileid: "4052892"
---
# <a name="get-usersxuidxuidachievements"></a>GET (/users/xuid({xuid})/achievements)
获取在标题、 解锁用户，或用户已在进行中定义的成就的列表。 这些 Uri 的域是`achievements.xboxlive.com`。
 
  * [URI 参数](#ID4EX)
  * [查询字符串参数](#ID4ECB)
  * [授权](#ID4ENF)
  * [需的请求标头](#ID4ESG)
  * [可选的请求标头](#ID4ESH)
  * [请求正文](#ID4EIBAC)
  * [响应正文](#ID4ETBAC)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 说明| 
| --- | --- | --- | 
| xuid| 64 位无符号的整数| Xbox 用户 ID (XUID) 在访问其 （资源） 的用户。 必须匹配的身份验证的用户的 XUID。| 
  
<a id="ID4ECB"></a>

 
## <a name="query-string-parameters"></a>查询字符串参数
 
| 参数| 必需| 类型| 说明| 
| --- | --- | --- | --- | --- | --- | --- | 
| <b>skipItems</b>| 否| 32 位有符号的整数| 返回在给定的项目数之后开始的项目。 例如， <b>skipItems ="3"</b>将检索项目开头的第四项检索。 | 
| <b>ContinuationToken</b>| 否| 字符串| 返回在给定的延续令牌启动的项目。 | 
| <b>maxItems</b>| 否| 32 位有符号的整数| 要从该集合，这可以与<b>skipItems</b>和<b>continuationToken</b>返回项目的范围结合使用返回的项目的最大数量。 如果<b>maxItems</b>不存在，并且可能会返回少于<b>maxItems</b>，即使尚未返回结果的最后一页服务可能会提供一个默认值。 | 
| <b>titleId</b>| 否| 字符串| 返回的结果筛选器。 接受一个或多个以逗号分隔的十进制作品标识符。| 
| <b>unlockedOnly</b>| 否| 布尔值| 返回的结果筛选器中。 如果设置为<b>true</b>，将只返回为用户解锁成就。 默认值为<b>false</b>。| 
| <b>possibleOnly</b>| 否| 布尔值| 返回的结果筛选器中。 如果设置为<b>true</b>，将返回所有可能的结果，但不是解锁元数据-只需从 XMS 的成就信息。 默认值为<b>false</b>。| 
| <b>类型</b>| 否| 字符串| 返回的结果筛选器。 可以是"持续"挑战"。 默认值为所有受支持的类型。| 
| <b>orderBy</b>| 否| 字符串| 指定的顺序返回结果。 可以是"无序"、"标题"、"UnlockTime"或"EndingSoon"。 默认值是"无序"。| 
  
<a id="ID4ENF"></a>

 
## <a name="authorization"></a>授权
 
| 声明| 是否为必需？| 说明| 如果缺少的行为| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 用户| 调用方是授权的 Xbox LIVE 用户。| 调用方必须是 Xbox LIVE 上的有效用户。| 403 已禁止| 
  
<a id="ID4ESG"></a>

 
## <a name="required-request-headers"></a>需的请求标头
 
| 标头| 类型| 说明| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 授权| 字符串| HTTP 身份验证的身份验证凭据。 示例值:"XBL3.0 x =&lt;userhash >;&lt;令牌 >"。| 
  
<a id="ID4ESH"></a>

 
## <a name="optional-request-headers"></a>可选的请求标头
 
| 标头| 类型| 说明| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| <b>X RequestedServiceVersion</b>| 字符串| 生成此请求应定向到 Xbox LIVE 的服务的名称/数。 验证在标头、 身份验证令牌等中的声明的有效性后仅为请求路由到该服务。默认值： 1。| 
| <b>x xbl 协定版本</b>| 32 位无符号的整数| 如果存在，并且设置为 2，就会使用此 API 的 V2 版本。 否则为 V1。| 
| <b>接受的语言</b>| 字符串| 所需的区域设置和回退 （例如，FR-FR、 fr、 EN-GB、 en 全球、 EN-US） 的列表。 成就服务将通过列表工作，直到找到匹配的本地化的字符串。 如果找不到，它将尝试匹配用户令牌，这是来自用户的 IP 地址中定义的位置。 如果找到仍不匹配的本地化的字符串，它使用由游戏开发人员/发布者提供的默认字符串。 | 
  
<a id="ID4EIBAC"></a>

 
## <a name="request-body"></a>请求正文
 
此请求的正文中不发送任何对象。
  
<a id="ID4ETBAC"></a>

 
## <a name="response-body"></a>响应正文
 
如果在调用成功，该服务返回[成就 (JSON)](../../json/json-achievementv2.md)对象和[PagingInfo (JSON)](../../json/json-paginginfo.md)对象的数组。
 
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
                "url":"http://www.xbox.com"
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

[Achievement (JSON)](../../json/json-achievementv2.md)

 [PagingInfo (JSON)](../../json/json-paginginfo.md)

 [分页参数](../../additional/pagingparameters.md)

   