---
title: MultiplayerSessionReference (JSON)
assetID: 6e03e060-8c9b-b394-415f-af7e85be569f
permalink: en-us/docs/xboxlive/rest/json-multiplayersessionreference.html
author: KevinAsgari
description: " MultiplayerSessionReference (JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7dfa27115e1c7ebc9be657ff4fb3f6946406dd8b
ms.sourcegitcommit: 72710baeee8c898b5ab77ceb66d884eaa9db4cb8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/12/2018
ms.locfileid: "3881402"
---
# <a name="multiplayersessionreference-json"></a>MultiplayerSessionReference (JSON)
表示**MultiplayerSessionReference**的 JSON 对象。 
<a id="ID4EQ"></a>

  
 
MultiplayerSessionReference JSON 对象具有以下规范。
 
| 成员| 类型| 说明| 
| --- | --- | --- | 
| scid| GUID| 服务配置标识符 (SCID)。 会话标识符的第 1 部分中。| 
| 模板名称 | 字符串 | 会话模板的当前实例的名称。 第 2 部分会话标识符。 | 
| name | 字符串 | 会话名称。 会话标识符的第 3 部分。 | 
  
<a id="ID4EZ"></a>

 
## <a name="sample-json-syntax"></a>JSON 语法示例 
 

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

   