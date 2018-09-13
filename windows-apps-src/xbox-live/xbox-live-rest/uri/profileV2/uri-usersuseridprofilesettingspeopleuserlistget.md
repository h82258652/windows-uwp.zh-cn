---
title: 获取 （/users/ {userId} / 配置文件/设置/人 / {用户列表}）
assetID: f6553499-89e2-f21b-a00f-7e5437c045ff
permalink: en-us/docs/xboxlive/rest/uri-usersuseridprofilesettingspeopleuserlistget.html
author: KevinAsgari
description: " 获取 （/users/ {userId} / 配置文件/设置/人 / {用户列表}）"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: d57a6620115d5f009c054210a50548c3da7e47d5
ms.sourcegitcommit: c8f6866100a4b38fdda8394ea185b02d7af66411
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/13/2018
ms.locfileid: "3958822"
---
# <a name="get-usersuseridprofilesettingspeopleuserlist"></a>获取 （/users/ {userId} / 配置文件/设置/人 / {用户列表}）
获取用户的个人资料或支持用户，与用户的名字对象。 这些 Uri 的域是`profile.xboxlive.com`。
 
  * [备注](#ID4EV)
  * [URI 参数](#ID4EKB)
  * [查询字符串参数](#ID4EVB)
  * [需的请求标头](#ID4EQC)
  * [请求正文](#ID4E2D)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>备注
 
**用户列表**和**Userid**是互斥的参数。 如果指定了两或任何一个，你将得到**BadRequest** 。 **用户列表**是未来校验多个命名的列表对请求有用的方案中的数组。 **Userid**构成 Xuid 十进制字符串-JSON 已损坏时序列化 64 位无符号的整数。 最后，将设置、 使用正常的用户可读的名称，而不是 64 位无符号的整数或模糊的常量，如**XONLINE_PROFILE_ASDF**命名 Xbox One 中的设置。
  
<a id="ID4EKB"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 说明| 
| --- | --- | --- | 
| 用户 Id| 字符串| 可以是 xuid(12345)、 gt(myGamertag) 或 me。| 
| 用户列表| 字符串| 用户获取设置已命名的列表。 目前，用户是唯一受支持的列表。| 
  
<a id="ID4EVB"></a>

 
## <a name="query-string-parameters"></a>查询字符串参数
 
| 参数| 类型| 说明| 
| --- | --- | --- | --- | --- | --- | 
| settings| 字符串| 设置名称的逗号分隔的列表。| 
  
<a id="ID4EQC"></a>

 
## <a name="required-request-headers"></a>需的请求标头
 
| 标头| 类型| 说明| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| x xbl 协定版本| 32 位有符号的整数| 值 = 2| 
| 内容类型| 字符串| 值 = <code>application/json</code>| 
  
<a id="ID4E2D"></a>

 
## <a name="request-body"></a>请求正文
 
<a id="ID4EBE"></a>

 
### <a name="sample-request"></a>示例请求
 

```cpp
GET /users/me/profile/settings/people/people?settings=GameDisplayName,GameDisplayPicRaw,Gamerscore,Gamertag
      
```

  
<a id="ID4EKE"></a>

  
 
<a id="ID4EME"></a>

 
##### <a name="response-body"></a>响应正文 
该响应是一个**ReadMultiSettingsResponseV2**对象。 假设调用用户有只有一个好友：
  

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
                  "value":"http://images-eds.xboxlive.com/image?url=z951ykn43p4FqWbbFvR2Ec.8vbDhj8G2Xe7JngaTToBrrCmIEEXHC9UNrdJ6P7KIN0gxC2r1YECCd3mf2w1FDdmFCpSokJWa2z7xtVrlzOyVSc6pPRdWEXmYtpS2xE4F&format=png&w=64&h=64"
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

   
<a id="ID4E3E"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4E5E"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[/users/ {userId} / 配置文件/设置/人 / {用户列表}？ 设置 = {设置}](uri-usersuseridprofilesettingspeopleuserlist.md)

 [配置文件 (JSON)](../../json/json-profile.md)

   