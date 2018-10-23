---
title: /users/{userId}/profile/settings/people/{userList}?settings={settings}
assetID: 0ba20eba-f0ab-28ab-61d3-b4f9e4c07bc5
permalink: en-us/docs/xboxlive/rest/uri-usersuseridprofilesettingspeopleuserlist.html
author: KevinAsgari
description: " /users/{userId}/profile/settings/people/{userList}?settings={settings}"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 44341b5fc8f831e3a500f47a51b94978f587cb8c
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/23/2018
ms.locfileid: "5407066"
---
# <a name="usersuseridprofilesettingspeopleuserlistsettingssettings"></a>/users/{userId}/profile/settings/people/{userList}?settings={settings}
访问用户或用户，人脉名字对象支持的配置文件。 这些 Uri 的域是`profile.xboxlive.com`。
 
  * [URI 参数](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 说明| 
| --- | --- | --- | 
| userId| 字符串| 可以是 xuid(12345)、 gt(myGamertag) 或 me。| 
| 用户列表| 字符串| 命名的人获取设置列表。 目前，用户是唯一支持的列表。| 
  
<a id="ID4E1B"></a>

 
## <a name="valid-methods"></a>有效的方法

[GET (/users/{userId}/profile/settings/people/{userList})](uri-usersuseridprofilesettingspeopleuserlistget.md)

&nbsp;&nbsp;获取用户的配置文件或支持用户，与人脉名字对象。
 
<a id="ID4EEC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EGC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[配置文件 URI](atoc-reference-profiles.md)

 [Profile (JSON)](../../json/json-profile.md)

   