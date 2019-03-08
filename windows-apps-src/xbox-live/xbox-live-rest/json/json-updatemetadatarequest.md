---
title: UpdateMetadataRequest (JSON)
assetID: 0bc210e3-c1dc-9267-e322-aadb9f0a074a
permalink: en-us/docs/xboxlive/rest/json-updatemetadatarequest.html
description: " UpdateMetadataRequest (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a76e4b12e0ffadb112913775b500ac0d39d413d5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57597762"
---
# <a name="updatemetadatarequest-json"></a>UpdateMetadataRequest (JSON)
应为一个剪辑更新元数据。 
<a id="ID4EN"></a>

 
## <a name="updatemetadatarequest"></a>UpdateMetadataRequest
 
UpdateMetadataRequest 对象具有以下规范。
 
| 成员| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| userCaption| 字符串| 更改的用户输入的游戏剪辑非本地化字符串。| 
| visibility| [GameClipVisibility 枚举](../enums/gvr-enum-gameclipvisibility.md)| 更改游戏剪辑的可见性，因为它已在系统中。| 
| titleData| 字符串| 特定于标题的属性包中。 最大大小：10 KB。| 
  
<a id="ID4EBC"></a>

 
## <a name="sample-json-syntax"></a>示例 JSON 语法
 
更改用户名剪辑和可见性：
 

```json
{
  "userCaption": "I've changed this 100 Times!",
  "visibility": "Owner"
}

```

 
更改只需标题属性 （因为此字段的架构是由调用方，这是只是一个示例）：
 

```json
{
  "titleData": "{ 'Id': '123456', 'Location': 'C:\\videos\\123456.mp4' }"
}

```

  
<a id="ID4EQC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4ESC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[JavaScript 对象表示法 (JSON) 对象引用](atoc-xboxlivews-reference-json.md)

  
<a id="ID4E3C"></a>

 
##### <a name="reference"></a>参考 

[POST (/users/me/scids/{scid}/clips/{gameClipId})](../uri/dvr/uri-usersmescidclipsgameclipidpost.md)

   