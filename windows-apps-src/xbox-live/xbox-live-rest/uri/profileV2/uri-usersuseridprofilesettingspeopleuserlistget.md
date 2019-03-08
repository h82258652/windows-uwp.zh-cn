---
title: GET (/users/{userId}/profile/settings/people/{userList})
assetID: f6553499-89e2-f21b-a00f-7e5437c045ff
permalink: en-us/docs/xboxlive/rest/uri-usersuseridprofilesettingspeopleuserlistget.html
description: " GET (/users/{userId}/profile/settings/people/{userList})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: f868fdf4f3d5cd36000784d9c5a3437fa5d67ffa
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593852"
---
# <a name="get-usersuseridprofilesettingspeopleuserlist"></a>GET (/users/{userId}/profile/settings/people/{userList})
获取用户的配置文件或支持用户与用户的名字对象。 这些 Uri 的域是`profile.xboxlive.com`。
 
  * [备注](#ID4EV)
  * [URI 参数](#ID4EKB)
  * [查询字符串参数](#ID4EVB)
  * [所需的请求标头](#ID4EQC)
  * [请求正文](#ID4E2D)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>备注
 
**userList**并**Userid**是互斥的参数。 如果指定了或其中一个，则将获得**BadRequest**返回。 **userList**是数组，以便前瞻性中多个命名的列表适用于请求的方案。 **Userid**是由组成的十进制字符串 XUIDs-JSON 是在 64 位无符号的整数进行序列化错误。 最后，在 Xbox One 的设置将被命名为设置，使用普通用户可读名称，而不是 64 位无符号的整数，或者模糊常量喜欢**XONLINE_PROFILE_ASDF**。
  
<a id="ID4EKB"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| userId| 字符串| 可以是 xuid(12345)、 gt(myGamertag) 或 me。| 
| userList| 字符串| 若要获取设置的联系人的命名的列表。 目前，人是受支持的唯一列表。| 
  
<a id="ID4EVB"></a>

 
## <a name="query-string-parameters"></a>查询字符串参数
 
| 参数| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | --- | --- | 
| 设置| 字符串| 设置名称的逗号分隔列表。| 
  
<a id="ID4EQC"></a>

 
## <a name="required-request-headers"></a>所需的请求标头
 
| 标头| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| x-xbl-contract-version| 32 位有符号的整数| 值 = 2| 
| content-type| 字符串| 值 = <code>application/json</code>| 
  
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
响应是**ReadMultiSettingsResponseV2**对象。 假定调用用户具有只有一个朋友：
  

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

[/users/{userId}/profile/settings/people/{userList}?settings={settings}](uri-usersuseridprofilesettingspeopleuserlist.md)

 [配置文件 (JSON)](../../json/json-profile.md)

   