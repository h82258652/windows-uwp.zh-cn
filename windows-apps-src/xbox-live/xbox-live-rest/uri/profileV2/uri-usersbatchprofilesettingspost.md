---
title: POST (/users/batch/profile/settings)
assetID: 2a619148-a626-f413-bda1-a2790063075d
permalink: en-us/docs/xboxlive/rest/uri-usersbatchprofilesettingspost.html
description: " POST (/users/batch/profile/settings)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: aa029c0cffa369eeb802521b394a52b958b54557
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2018
ms.locfileid: "8879888"
---
# <a name="post-usersbatchprofilesettings"></a>POST (/users/batch/profile/settings)
获取用户或用户配置文件。 这些 Uri 的域是`profile.xboxlive.com`。
 
  * [备注](#ID4EV)
  * [授权](#ID4EFB)
  * [需的请求标头](#ID4EOB)
  * [请求正文](#ID4EZC)
  * [响应正文](#ID4EJD)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>备注
 
这是仅完全限定的个人资料 URL 中允许。 从客户端的所有其他配置文件 Api 会被阻止。
  
<a id="ID4EFB"></a>

 
## <a name="authorization"></a>授权
 
若要访问配置文件，仅正常身份验证令牌和声明需要。
  
<a id="ID4EOB"></a>

 
## <a name="required-request-headers"></a>需的请求标头
 
| 标头| 类型| 描述| 
| --- | --- | --- | 
| x xbl 协定版本| 32 位无符号的整数| 协定版本必须设置为 2，来区分 Xbox 360 API 从此调用。| 
| 内容类型| 字符串| 值 = <code>application/json</code>| 
  
<a id="ID4EZC"></a>

 
## <a name="request-body"></a>请求正文
 
<a id="ID4E6C"></a>

 
### <a name="sample-request"></a>示例请求
 

```cpp
POST /users/batch/profile/settings
   {
      "userIds":[
         "2533274791381930"
       ],
      "settings":[
         "GameDisplayName",
         "GameDisplayPicRaw",
         "Gamerscore",
         "Gamertag"
      ]
   }
      
```

   
<a id="ID4EJD"></a>

 
## <a name="response-body"></a>响应正文
 
<a id="ID4EPD"></a>

 
### <a name="sample-response"></a>示例响应
 

```cpp
{
      "profileUsers":[
         {
            "id":"2533274791381930",
            "settings":[
               {
                  "id":"GameDisplayName",
                  "value":"John Smith"
               },
               {
                  "id":"GameDisplayPicRaw",
                  "value":"http://images-eds.xboxlive.com/image?url=z951ykn43p4FqWbbFvR2Ec.8vbDhj8G2Xe7JngaTToBrrCmIEEXHC9UNrdJ6P7KIN0gxC2r1YECCd3mf2w1FDdmFCpSokJWa2z7xtVrlzOyVSc6pPRdWEXmYtpS2xE4F"
               },
               {
                  "id":"Gamerscore",
                  "value":"0"
               },
               {
                  "id":"Gamertag",
                  "value":"CracklierJewel9"
               }
            ]
         }
      ]
   }
         
```

   
<a id="ID4EZD"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4E2D"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[/users/batch/profile/settings](uri-usersbatchprofilesettings.md)

  
<a id="ID4EFE"></a>

 
##### <a name="reference"></a>参考 

[Profile (JSON)](../../json/json-profile.md)

   