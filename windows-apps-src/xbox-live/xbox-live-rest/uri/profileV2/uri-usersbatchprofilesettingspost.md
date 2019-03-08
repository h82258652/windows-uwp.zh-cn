---
title: POST (/users/batch/profile/settings)
assetID: 2a619148-a626-f413-bda1-a2790063075d
permalink: en-us/docs/xboxlive/rest/uri-usersbatchprofilesettingspost.html
description: " POST (/users/batch/profile/settings)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 0f859a58e32624223d59d918d46f6230a3abd6db
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662222"
---
# <a name="post-usersbatchprofilesettings"></a>POST (/users/batch/profile/settings)
获取一个用户或用户的配置文件。 这些 Uri 的域是`profile.xboxlive.com`。
 
  * [备注](#ID4EV)
  * [Authorization](#ID4EFB)
  * [所需的请求标头](#ID4EOB)
  * [请求正文](#ID4EZC)
  * [响应正文](#ID4EJD)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>备注
 
这是中允许使用仅完全限定配置文件 URL。 所有其他配置文件 Api 的客户端会被阻止。
  
<a id="ID4EFB"></a>

 
## <a name="authorization"></a>授权
 
若要访问配置文件，只有正常的身份验证令牌和声明需要。
  
<a id="ID4EOB"></a>

 
## <a name="required-request-headers"></a>所需的请求标头
 
| 标头| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| x-xbl-contract-version| 32 位无符号的整数| 协定版本必须设置为 2，以区分从 Xbox 360 API 此调用。| 
| content-type| 字符串| 值 = <code>application/json</code>| 
  
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
                  "value":"https://images-eds.xboxlive.com/image?url=z951ykn43p4FqWbbFvR2Ec.8vbDhj8G2Xe7JngaTToBrrCmIEEXHC9UNrdJ6P7KIN0gxC2r1YECCd3mf2w1FDdmFCpSokJWa2z7xtVrlzOyVSc6pPRdWEXmYtpS2xE4F"
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

[配置文件 (JSON)](../../json/json-profile.md)

   