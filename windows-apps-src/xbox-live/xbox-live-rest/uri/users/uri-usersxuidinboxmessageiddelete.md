---
title: DELETE (/users/xuid({xuid})/inbox/{messageId})
assetID: c54eede3-3e3b-2cbe-1be9-8bf3a48171bc
permalink: en-us/docs/xboxlive/rest/uri-usersxuidinboxmessageiddelete.html
description: " DELETE (/users/xuid({xuid})/inbox/{messageId})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 80ec2a462648177cc6bfc846b9c84278821b0e5e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594102"
---
# <a name="delete-usersxuidxuidinboxmessageid"></a>DELETE (/users/xuid({xuid})/inbox/{messageId})
删除用户的收件箱中的用户消息。 这些 Uri 的域是`msg.xboxlive.com`。
 
  * [备注](#ID4EV)
  * [URI 参数](#ID4ECB)
  * [Authorization](#ID4EPB)
  * [请求正文](#ID4E1B)
  * [HTTP 状态代码](#ID4EHC)
  * [JavaScript 对象表示法 (JSON) 响应](#ID4EAE)
  * [资源上的隐私设置的效果](#ID4EYF)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>备注 
 
删除操作是幂等。
 
此 API 支持只包含内容类型为"application/json"; 所需的每个调用的 HTTP 标头中。 
  
<a id="ID4ECB"></a>

 
## <a name="uri-parameters"></a>URI 参数 
 
| 参数| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| xuid | 64 位无符号的整数 | Xbox 用户 ID (XUID) 正在发出请求的播放器。 | 
| messageId | string[50] | 正在检索或删除的消息的 ID。 | 
  
<a id="ID4EPB"></a>

 
## <a name="authorization"></a>授权 
 
您必须具有用户声明以删除用户消息。
  
<a id="ID4E1B"></a>

 
## <a name="request-body"></a>请求正文 
 
此请求的正文中不发送的任何对象。
  
<a id="ID4EHC"></a>

 
## <a name="http-status-codes"></a>HTTP 状态代码 
 
服务将返回其中一个状态代码在本部分中使用此方法在此资源上发出的请求的响应中。 有关与 Xbox Live 服务一起使用的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。
 
| 代码| 描述| 
| --- | --- | --- | --- | --- | 
| 204| 成功。| 
| 403| 不能转换 XUID 或找不到有效的 XUID 声明。| 
| 404| 无法分析在 URI 中的消息 ID 或 XUID 缺少在 URI 中。| 
| 500| 常规服务器端错误。| 
  
<a id="ID4EAE"></a>

 
## <a name="javascript-object-notation-json-response"></a>JavaScript 对象表示法 (JSON) 响应 
 
如果出现错误，该服务可能返回 errorResponse 对象，其中可能包含服务的环境中的值。
 
| 属性| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| errorSource| 字符串| 指示错误源于何处。| 
| errorCode| int| 与错误 （可以为 null） 关联的数值代码。| 
| errorMessage| 字符串| 如果配置为显示详细信息的错误的详细信息。| 
  
<a id="ID4EYF"></a>

 
## <a name="effect-of-privacy-settings-on-resource"></a>资源上的隐私设置的效果 
 
仅可以删除您自己的用户消息。 
  
<a id="ID4EDG"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EFG"></a>

 
##### <a name="parent"></a>Parent 的子磁盘）  

[/users/xuid({xuid})/inbox](uri-usersxuidinbox.md)

  
<a id="ID4ETG"></a>

 
##### <a name="reference--standard-http-status-codesadditionalhttpstatuscodesmd"></a>引用[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)

   