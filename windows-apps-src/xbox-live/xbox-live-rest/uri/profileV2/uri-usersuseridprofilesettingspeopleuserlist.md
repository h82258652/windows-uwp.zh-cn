---
title: /users/{userId}/profile/settings/people/{userList}?settings={settings}
assetID: 0ba20eba-f0ab-28ab-61d3-b4f9e4c07bc5
permalink: en-us/docs/xboxlive/rest/uri-usersuseridprofilesettingspeopleuserlist.html
description: " /users/{userId}/profile/settings/people/{userList}?settings={settings}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 24b58c817156a7c372a8e6acfab895e6b7c51207
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57636962"
---
# <a name="usersuseridprofilesettingspeopleuserlistsettingssettings"></a>/users/{userId}/profile/settings/people/{userList}?settings={settings}
访问一个用户或用户的人员名字对象支持的配置文件。 这些 Uri 的域是`profile.xboxlive.com`。
 
  * [URI 参数](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| userId| 字符串| 可以是 xuid(12345)、 gt(myGamertag) 或 me。| 
| userList| 字符串| 若要获取设置的联系人的命名的列表。 目前，人是受支持的唯一列表。| 
  
<a id="ID4E1B"></a>

 
## <a name="valid-methods"></a>有效的方法

[GET (/users/{userId}/profile/settings/people/{userList})](uri-usersuseridprofilesettingspeopleuserlistget.md)

&nbsp;&nbsp;获取用户的配置文件或支持用户与用户的名字对象。
 
<a id="ID4EEC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EGC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[配置文件的 Uri](atoc-reference-profiles.md)

 [配置文件 (JSON)](../../json/json-profile.md)

   