---
title: UserSettings (JSON)
assetID: 17c030cb-05e0-f78e-5ab1-cdbd8b801ceb
permalink: en-us/docs/xboxlive/rest/json-usersettings.html
description: " UserSettings (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5451c59ab608105677a657ade41154bd2b622f5e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655042"
---
# <a name="usersettings-json"></a>UserSettings (JSON)
返回当前经过身份验证的用户的设置。 
<a id="ID4EN"></a>

 
## <a name="usersettings"></a>UserSettings
 
用户设置对象具有以下规范。
 
| 成员| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| id| 32 位无符号的整数| 设置的标识符。| 
| 源| 32 位无符号的整数| 表示设置的源。 | 
| titleId| 32 位无符号的整数| 标题与设置关联的标识符。 | 
| value| 8 位无符号整数数组| 表示设置的值。 正在检索设置的客户端必须了解要将无法读取数据的表示形式格式。 | 
  
<a id="ID4EJC"></a>

 
## <a name="sample-json-syntax"></a>示例 JSON 语法
 

```json
{
   "id":268697600,
   "source":1,
   "titleId":1297287259,
   "value":"CAAAAA=="
}
    
```

  
<a id="ID4ESC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EUC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[JavaScript 对象表示法 (JSON) 对象引用](atoc-xboxlivews-reference-json.md)

   