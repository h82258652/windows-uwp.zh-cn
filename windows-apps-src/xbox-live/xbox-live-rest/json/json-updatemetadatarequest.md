---
title: UpdateMetadataRequest (JSON)
assetID: 0bc210e3-c1dc-9267-e322-aadb9f0a074a
permalink: en-us/docs/xboxlive/rest/json-updatemetadatarequest.html
author: KevinAsgari
description: " UpdateMetadataRequest (JSON)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a0b7a3d90e5a69807e3ac9532a58a845568c93f2
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2018
ms.locfileid: "7563752"
---
# <a name="updatemetadatarequest-json"></a>UpdateMetadataRequest (JSON)
应更新剪辑元数据。 
<a id="ID4EN"></a>

 
## <a name="updatemetadatarequest"></a>UpdateMetadataRequest
 
UpdateMetadataRequest 对象具有以下规范。
 
| 成员| 类型| 说明| 
| --- | --- | --- | 
| userCaption| 字符串| 更改游戏剪辑的用户输入非本地化字符串。| 
| 可见性| [GameClipVisibility 枚举](../enums/gvr-enum-gameclipvisibility.md)| 更改游戏剪辑的可见性，因为它在系统中发布。| 
| titleData| 字符串| 特定于游戏的属性包中。 最大大小： 10 KB。| 
  
<a id="ID4EBC"></a>

 
## <a name="sample-json-syntax"></a>JSON 语法示例
 
更改用户剪辑名称和可见性：
 

```json
{
  "userCaption": "I've changed this 100 Times!",
  "visibility": "Owner"
}

```

 
更改只是标题属性 （这只是一个示例，由于此字段的架构是由调用方负责）：
 

```json
{
  "titleData": "{ 'Id': '123456', 'Location': 'C:\\videos\\123456.mp4' }"
}

```

  
<a id="ID4EQC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4ESC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[JavaScript 对象表示法 (JSON) 对象参考](atoc-xboxlivews-reference-json.md)

  
<a id="ID4E3C"></a>

 
##### <a name="reference"></a>参考 

[POST (/users/me/scids/{scid}/clips/{gameClipId})](../uri/dvr/uri-usersmescidclipsgameclipidpost.md)

   