---
title: MultiplayerSessionRequest (JSON)
assetID: 2e311c6d-3427-5a39-1989-06dc08483057
permalink: en-us/docs/xboxlive/rest/json-multiplayersessionrequest.html
description: " MultiplayerSessionRequest (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 18929c060adeae47f0305422dd312e7410f93981
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628342"
---
# <a name="multiplayersessionrequest-json"></a>MultiplayerSessionRequest (JSON)
上的操作请求 JSON 对象传递**MultiplayerSession**对象。 
<a id="ID4EQ"></a>

  
 
MultiplayerSessionRequest JSON 对象具有以下规范。
 
| 成员| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| 常量| 对象| 与要为会话生成常量的会话模板合并的只读设置。 | 
| 属性 | 对象 | 要合并到会话属性的更改。| 
| members.me | 对象| 常量和大量的属性，如对应的顶级。 任何 PUT 方法要求用户属于会话，并添加用户，如有必要。 如果"me"被指定为 null，则会从会话中删除发出请求的成员。 | 
| 成员 | 对象| 其他对象，表示用户添加到会话，并从零开始的索引进行键控。 在请求中的成员数始终 0 开始，即使会话已包含的成员。 成员将添加到请求中的显示的顺序中的会话。 成员属性仅可以设置到其所属的用户。 | 
| 服务器 | 对象| 值，该值指示更新以及会话的新增功能的一组关联的服务器的参与者。 如果一台服务器指定为 null，是从会话中删除该服务器条目。 | 
  
<a id="ID4EZ"></a>

 
## <a name="request-structure"></a>请求结构
 

```json
{
  "constants": { /* Property Bag */ },
  "properties": { /* Property Bag */ },
  "members": {
    // Requires a service principal. Existing members can be deleted by index.
    // Not available on large sessions.
    "5": null,

    // Reservation requests must start with zero. New users will get added in order to the end of the session's member list.
    // Large sessions don't support reservations.
    "reserve_0": {
      "constants": { /* Property Bag */ }
    },
    "reserve_1": {
      "constants": { /* Property Bag */ }
    },

    // Requires a user principal with a xuid claim. Can be 'null' to remove myself from the session.
    "me": {
      "constants": { /* Property Bag */ },
      "properties": { /* Property Bag */ },
    }
  },

  // Requires a server principal.
  "servers": {
    // Can be any name. The value can be 'null' to remove the server from the session.
    "name": {
      "constants": {  /* Property Bag */ },
      "properties": {  /* Property Bag */ }
    }
  }
}
```

  
<a id="ID4EAB"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4ECB"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[JavaScript 对象表示法 (JSON) 对象引用](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EMB"></a>

 
##### <a name="reference"></a>参考 

[MultiplayerSession (JSON)](json-multiplayersession.md)

 [PUT (/serviceconfigs/ {scid} /sessiontemplates/ {sessionTemplateName} /sessions/ {sessionName})](../uri/sessiondirectory/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnameput.md)

   