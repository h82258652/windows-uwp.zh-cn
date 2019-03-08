---
title: GET (/users/xuid({xuid})/achievements/{scid}/{achievementid})
assetID: 27318886-f084-d6a8-e582-3eb070ccbc38
permalink: en-us/docs/xboxlive/rest/uri-usersxuidachievementsscidachievementidget.html
description: " GET (/users/xuid({xuid})/achievements/{scid}/{achievementid})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 19083937d48d8c312215f1734513d83c59f52f0d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627502"
---
# <a name="get-usersxuidxuidachievementsscidachievementid"></a>GET (/users/xuid({xuid})/achievements/{scid}/{achievementid})
获取该成就的详细信息。 这些 Uri 的域是`achievements.xboxlive.com`。
 
  * [URI 参数](#ID4EV)
  * [Authorization](#ID4EAB)
  * [资源上的隐私设置的效果](#ID4E4C)
  * [所需的请求标头](#ID4EPG)
  * [可选的请求标头](#ID4EPH)
  * [请求正文](#ID4ECBAC)
  * [HTTP 状态代码](#ID4ENBAC)
  * [响应正文](#ID4EBGAC)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| xuid| 64 位无符号的整数| Xbox 用户 ID (XUID) 其资源的访问的用户。 必须与匹配身份验证的用户的 XUID。| 
| scid| GUID| 正在访问其成就的服务配置的唯一标识符。| 
| achievementid| 32 位无符号的整数| 正在访问该成就的 （在指定 SCID) 唯一标识符。| 
  
<a id="ID4EAB"></a>

 
## <a name="authorization"></a>授权
 
使用授权声明 | 声明| 是否为必需？| 描述| 如果缺少的行为| 
| --- | --- | --- | --- | --- | --- | --- | 
| 用户| 是| 在 Xbox LIVE 为其发出请求的有效用户。| 403 禁止访问| 
| Title| 否| 调用的标题。| 取决于授权。 自 2013 年 5 月 1 日起 AuthZ 未提供的声明时缺少并因此将拒绝任何 SCIDs 未标记为公共的访问。| 
| 沙盒| 否| 沙盒应从中检索结果。| 取决于授权。 自 2013 年 5 月 1 日起 AuthZ 时不提供默认声明缺少。| 
  
<a id="ID4E4C"></a>

 
## <a name="effect-of-privacy-settings-on-resource"></a>资源上的隐私设置的效果
 
资源上的隐私设置的效果 | 发出请求的用户| 面向用户的隐私设置| 行为| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 我| -| 如所述。| 
| 友元| 每个人| 确定| 
| 友元| 仅朋友| 确定| 
| 友元| 已阻止| 禁止访问。| 
| 非友元用户| 每个人| 确定| 
| 非友元用户| 仅朋友| 禁止访问。| 
| 非友元用户| 已阻止| 禁止访问。| 
| 第三方站点| 每个人| 确定| 
| 第三方站点| 仅朋友| 禁止访问。| 
| 第三方站点| 已阻止| 禁止访问。| 
  
<a id="ID4EPG"></a>

 
## <a name="required-request-headers"></a>所需的请求标头
 
| 标头| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 授权| 字符串| HTTP 身份验证的身份验证凭据。 示例值："XBL3.0 x =&lt;userhash >;&lt;令牌 >"。| 
  
<a id="ID4EPH"></a>

 
## <a name="optional-request-headers"></a>可选的请求标头
 
| 标头| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion| 字符串| 生成此请求应定向到 Xbox LIVE 的服务的名称/编号。 请求将只路由到的服务验证标头身份验证令牌中声明的有效性后，依次类推。 默认值：1.| 
| x-xbl-contract-version| 字符串| 默认值为 V1。| 
| Accept-Language| 字符串| 所需的区域设置和回退 （例如，-FR、 fr、 EN-GB、 en WW、 EN-US） 的列表。 获得的成就服务将共同完成列表，直到找到匹配的已本地化的字符串。 如果找不到，它尝试匹配来自用户的 IP 地址的用户标记中定义的位置。 如果找到仍没有匹配的本地化的字符串，它使用标题开发人员/发布者提供的默认字符串。 | 
  
<a id="ID4ECBAC"></a>

 
## <a name="request-body"></a>请求正文
 
此请求的正文中不发送的任何对象。
  
<a id="ID4ENBAC"></a>

 
## <a name="http-status-codes"></a>HTTP 状态代码
 
服务将返回其中一个状态代码在本部分中使用此方法在此资源上发出的请求的响应中。 有关与 Xbox Live 服务一起使用的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。
 
| 代码| 原因短语| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| 确定| 已成功检索该会话。| 
| 301| 已永久移动| 服务已移动到另一个 URI。| 
| 307| 临时重定向| 已暂时更改此资源的 URI。| 
| 400| 无效的请求| 服务无法理解请求格式不正确。 通常是一个无效的参数。| 
| 401| 未经授权| 请求需要用户身份验证。| 
| 403| 已禁止| 为用户或服务不允许该请求。| 
| 404| 未找到| 找不到指定的资源。| 
| 406| 不可接受| 不支持资源版本;应拒绝的 MVC 层。| 
| 408| 请求超时| 该请求花费了太长时间才能完成。| 
| 410| 消失| 请求的资源不再可用。| 
  
<a id="ID4EBGAC"></a>

 
## <a name="response-body"></a>响应正文
 
<a id="ID4EHGAC"></a>

 
### <a name="sample-response"></a>示例响应
 

```cpp
{
    "achievement":
    {
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
    }
}
         
```

   
<a id="ID4ERGAC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4ETGAC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[/users/xuid({xuid})/achievements/{scid}/{achievementid}](uri-usersxuidachievementsscidachievementid.md)

   