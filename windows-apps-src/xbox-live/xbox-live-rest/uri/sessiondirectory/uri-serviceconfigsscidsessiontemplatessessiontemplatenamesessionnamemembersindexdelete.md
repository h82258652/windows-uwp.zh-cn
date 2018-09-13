---
title: 删除 (/serviceconfigs/ {scid} /sessiontemplates/ {sessionTemplateName} {会话名} /sessions/ /members/ {索引})
assetID: 00aa2f3d-69a6-6d68-e99b-aad4b102aba3
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionnamemembersindexdelete.html
author: KevinAsgari
description: " 删除 (/serviceconfigs/ {scid} /sessiontemplates/ {sessionTemplateName} {会话名} /sessions/ /members/ {索引})"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 03adb20f796e7bff59214999febad38434a2a287
ms.sourcegitcommit: c8f6866100a4b38fdda8394ea185b02d7af66411
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/13/2018
ms.locfileid: "3958458"
---
# <a name="delete-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnamemembersindex"></a>删除 (/serviceconfigs/ {scid} /sessiontemplates/ {sessionTemplateName} {会话名} /sessions/ /members/ {索引})
从会话中删除指定的成员。

> [!IMPORTANT]
> 此 URI 方法需要 X Xbl 协定版本的标头元素： 104/105 或更高版本上每个请求。

  * [URI 参数](#ID4ET)
  * [HTTP 状态代码](#ID4E5)
  * [请求正文](#ID4EFB)
  * [响应正文](#ID4EOB)

<a id="ID4ET"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 类型| 说明|
| --- | --- | --- | --- |
| scid| GUID| 服务配置标识符 (SCID)。 第 1 部分会话标识符。|
| sessionTemplateName| 字符串| 会话模板的当前实例的名称。 第 2 部分会话标识符。|
| 会话名| GUID| 会话的唯一 ID。 会话标识符的第 3 部分。|

<a id="ID4E5"></a>


## <a name="http-status-codes"></a>HTTP 状态代码
该服务返回 HTTP 状态代码，因为它适用于 MPSD。  
<a id="ID4EFB"></a>


## <a name="request-body"></a>请求正文
请参阅[MultiplayerSessionRequest (JSON)](../../json/json-multiplayersessionrequest.md)中的请求结构。  
<a id="ID4EOB"></a>


## <a name="response-body"></a>响应正文
请参阅[MultiplayerSession (JSON)](../../json/json-multiplayersession.md)中的响应结构。  
<a id="ID4EYB"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4E1B"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[/serviceconfigs/ {scid} /sessiontemplates/ {sessionTemplateName} {会话名} /sessions/ /members/ {索引}](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionnamemembersindex.md)
