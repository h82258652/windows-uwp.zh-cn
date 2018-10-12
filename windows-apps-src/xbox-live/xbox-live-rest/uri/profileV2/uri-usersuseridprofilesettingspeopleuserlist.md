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
ms.sourcegitcommit: d10fb9eb5f75f2d10e1c543a177402b50fe4019e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/12/2018
ms.locfileid: "4562857"
---
# <a name="usersuseridprofilesettingspeopleuserlistsettingssettings"></a>/users/{userId}/profile/settings/people/{userList}?settings={settings}
访问用户或用户，人脉名字对象支持的配置文件。 这些 Uri 的域是`profile.xboxlive.com`。
 
  * [URI 参数](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 描述| 
| --- | --- | --- | 
| userId| 字符串| 可以是 xuid(12345)、 gt(myGamertag) 或 me。| 
| userList| 字符串| 命名的人员获取设置的列表。 目前，用户是唯一受支持的列表。| 
  
<a id="ID4E1B"></a>

 
## <a name="valid-methods"></a>有效的方法

[GET (/users/{userId}/profile/settings/people/{userList})](uri-usersuseridprofilesettingspeopleuserlistget.md)

&nbsp;&nbsp;获取用户的个人资料或支持用户，与人脉名字对象。
 
<a id="ID4EEC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EGC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[配置文件 URI](atoc-reference-profiles.md)

 [Profile (JSON)](../../json/json-profile.md)

   