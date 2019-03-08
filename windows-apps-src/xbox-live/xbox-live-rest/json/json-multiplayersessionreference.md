---
title: MultiplayerSessionReference (JSON)
assetID: 6e03e060-8c9b-b394-415f-af7e85be569f
permalink: en-us/docs/xboxlive/rest/json-multiplayersessionreference.html
description: " MultiplayerSessionReference (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5986079e1cae3338d8cc24a9e85f6941cf4fbec4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651242"
---
# <a name="multiplayersessionreference-json"></a>MultiplayerSessionReference (JSON)
一个 JSON 对象，表示**MultiplayerSessionReference**。 
<a id="ID4EQ"></a>

  
 
MultiplayerSessionReference JSON 对象具有以下规范。
 
| 成员| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| scid| GUID| 服务配置标识符 (SCID)。 会话标识符的第 1 部分中。| 
| templateName | 字符串 | 会话模板的当前实例的名称。 会话标识符的第 2 部分中。 | 
| name | 字符串 | 会话的名称。 会话标识符的第 3 部分。 | 
  
<a id="ID4EZ"></a>

 
## <a name="sample-json-syntax"></a>示例 JSON 语法 
 

```json
{
  "scid": "8d050174-412b-4d51-a29b-d55a34edfdb7",
  "templateName": "integration",
  "name": "19de0095d8bb41048f19edbbb6bc6b04"
}
  
    
```

  
<a id="ID4EJB"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4ELB"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[JavaScript 对象表示法 (JSON) 对象引用](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EVB"></a>

 
##### <a name="reference"></a>参考 

[MultiplayerSession (JSON)](json-multiplayersession.md)

   