---
title: GET (/users/xuid({xuid})/achievements/{scid}/{achievementid})
assetID: 27318886-f084-d6a8-e582-3eb070ccbc38
permalink: en-us/docs/xboxlive/rest/uri-usersxuidachievementsscidachievementidget.html
description: " GET (/users/xuid({xuid})/achievements/{scid}/{achievementid})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 9b1eb428b130798c34befc999736153fc0e89735
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2018
ms.locfileid: "8897090"
---
# <a name="get-usersxuidxuidachievementsscidachievementid"></a>GET (/users/xuid({xuid})/achievements/{scid}/{achievementid})
获取在成就的详细信息。 这些 Uri 的域是`achievements.xboxlive.com`。
 
  * [URI 参数](#ID4EV)
  * [授权](#ID4EAB)
  * [资源上的隐私设置的效果](#ID4E4C)
  * [需的请求标头](#ID4EPG)
  * [可选的请求标头](#ID4EPH)
  * [请求正文](#ID4ECBAC)
  * [HTTP 状态代码](#ID4ENBAC)
  * [响应正文](#ID4EBGAC)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 描述| 
| --- | --- | --- | 
| xuid| 64 位无符号的整数| Xbox 用户 ID (XUID) 所访问的资源的用户。 必须匹配的身份验证的用户的 XUID。| 
| scid| GUID| 其成就所访问的服务配置的唯一标识符。| 
| achievementid| 32 位无符号的整数| 正在访问的成就 （中指定的 SCID) 的唯一标识符。| 
  
<a id="ID4EAB"></a>

 
## <a name="authorization"></a>授权
 
使用授权声明 | 声明| 是否为必需？| 描述| 如果缺少的行为| 
| --- | --- | --- | --- | --- | --- | --- | 
| 用户| 是| Xbox LIVE 的身份提出请求上是有效的用户。| 403 已禁止| 
| Title| 否| 调用的标题。| 依赖于身份验证。 截至 2013 年 5 月 1 日，或者不提供声明时缺少并因此将未标记为公共任何 Scid 拒绝访问。| 
| 沙盒| 否| 应从中检索结果沙盒。| 依赖于身份验证。 截至 2013 年 5 月 1 日，或者不提供默认声明时缺少。| 
  
<a id="ID4E4C"></a>

 
## <a name="effect-of-privacy-settings-on-resource"></a>资源上的隐私设置的效果
 
资源上的隐私设置的效果 | 请求的用户| 目标用户的隐私设置| 行为| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 我| -| 所述。| 
| 好友| 每个人都| “确定”| 
| 好友| 仅好友| “确定”| 
| 好友| 阻止| 禁止访问。| 
| 非好友用户| 每个人都| “确定”| 
| 非好友用户| 仅好友| 禁止访问。| 
| 非好友用户| 阻止| 禁止访问。| 
| 第三方网站| 每个人都| “确定”| 
| 第三方网站| 仅好友| 禁止访问。| 
| 第三方网站| 阻止| 禁止访问。| 
  
<a id="ID4EPG"></a>

 
## <a name="required-request-headers"></a>需的请求标头
 
| 标头| 类型| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 授权| 字符串| HTTP 身份验证的身份验证凭据。 示例值:"XBL3.0 x =&lt;userhash >;&lt;令牌 >"。| 
  
<a id="ID4EPH"></a>

 
## <a name="optional-request-headers"></a>可选的请求标头
 
| 标头| 类型| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X RequestedServiceVersion| 字符串| 名称/的内部版本号应指向此请求的 Xbox LIVE 的服务。 请求将仅可路由到的服务验证该标头，身份验证令牌中的声明的有效性后，依此类推。 默认值： 1。| 
| x xbl 协定版本| 字符串| 默认值为 V1。| 
| 接受的语言| 字符串| 所需的区域设置和回退 （例如，FR-FR、 fr、 EN-GB、 en 全球、 EN-US） 的列表。 成就服务将通过列表工作，直到找到匹配的本地化的字符串。 如果找不到，它将尝试以匹配用户令牌，这是来自用户的 IP 地址中定义的位置。 如果找到仍不匹配的本地化的字符串，它使用由游戏开发人员/发布者提供的默认字符串。 | 
  
<a id="ID4ECBAC"></a>

 
## <a name="request-body"></a>请求正文
 
此请求的正文中不发送任何对象。
  
<a id="ID4ENBAC"></a>

 
## <a name="http-status-codes"></a>HTTP 状态代码
 
此部分中使用此方法对此资源所做的请求的响应，该服务返回的状态代码之一。 有关使用 Xbox Live 服务的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。
 
| 代码| 原因短语| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| “确定”| 已成功检索会话。| 
| 301| 已永久移动| 该服务已移动到不同的 URI。| 
| 307| 临时重定向| 此资源的 URI 暂时已发生更改。| 
| 400| 错误请求| 服务可能不理解格式不正确的请求。 通常无效参数。| 
| 401| 未授权| 请求要求用户身份验证。| 
| 403| 已禁止| 为用户或服务不允许该请求。| 
| 404| 找不到| 找不到指定的资源。| 
| 406| 不允许| 资源版本不受支持;应拒绝 MVC 层。| 
| 408| 请求超时| 请求所花的时间太长，才能完成。| 
| 410| 走| 所请求的资源不再可用。| 
  
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
    }
}
         
```

   
<a id="ID4ERGAC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4ETGAC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[/users/xuid({xuid})/achievements/{scid}/{achievementid}](uri-usersxuidachievementsscidachievementid.md)

   