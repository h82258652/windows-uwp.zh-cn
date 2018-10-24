---
title: POST (/titles/{Title Id}/sessionhosts)
assetID: 8558b336-1af9-8143-9752-477ceb3a8e4e
permalink: en-us/docs/xboxlive/rest/uri-titlestitleidsessionhosts-post.html
author: KevinAsgari
description: " POST (/titles/{Title Id}/sessionhosts)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 147df5a3032aa950b7b301f7990c5456db200d2c
ms.sourcegitcommit: 4b97117d3aff38db89d560502a3c372f12bb6ed5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2018
ms.locfileid: "5431070"
---
# <a name="post-titlestitle-idsessionhosts"></a>POST (/titles/{Title Id}/sessionhosts)
创建新群集请求。 这些 Uri 的域是`gameserverms.xboxlive.com`。
 
  * [URI 参数](#ID4EX)
  * [需的请求标头](#ID4EGB)
  * [请求正文](#ID4E5B)
  * [所需的响应标头](#ID4ELD)
  * [响应正文](#ID4ESD)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 描述| 
| --- | --- | 
| titleId| 游戏应在其中操作该请求 ID。| 
  
<a id="ID5EG"></a>

 
## <a name="host-name"></a>主机名

gameserverms.xboxlive.com
 
<a id="ID4EGB"></a>

 
## <a name="required-request-headers"></a>需的请求标头
 
发出请求时, 显示下表中的标头是必需的。
 
| 标头| 值| 说明| 
| --- | --- | --- | --- | --- | 
| 内容类型| 应用程序/json| 提交的数据的类型。| 
  
<a id="ID4E5B"></a>

 
## <a name="request-body"></a>请求正文
 
请求必须包含一个具有以下成员的 JSON 对象。
 
| 成员| 说明| 
| --- | --- | --- | --- | --- | --- | --- | 
| sessionId| 这是调用方指定的标识符。 它已分配给会话主机进行分配和返回。 以后，你可以通过此标识符来引用特定 sessionhost。 它必须是全局唯一 (即 GUID)。| 
| SandboxId| 你想要中分配的会话主机沙盒。| 
| cloudGameId| 云游戏标识符。| 
| 位置| 你想要从分配的会话的首选位置的排序的列表。| 
| sessionCookie| 这是调用方指定不透明的字符串。 它与 sessionhost 相关联，并可以在你的游戏代码中引用。 使用此成员从客户端向服务器 （最大大小为 4 KB） 传递少量的信息。| 
| gameModelId| 游戏模式标识符。| 
 
<a id="ID4EDD"></a>

 
### <a name="sample-request"></a>示例请求
 

```cpp
{
        "sessionId": "3536d3e6-e85d-4f47-b898-9617d19dabcd",
        "sandboxId": "ISST.0",
        "cloudGameId": "1b7f9925-369c-4301-b1f7-1125dce25776",
        "locations": [
        "West US",
        "East US",
        "West Europe"
        ],
        "sessionCookie": "Caller provided opaque string",
        "gameModeId": "2162d32c-7ac8-40e9-9b1f-56676b8b2513"
        }
      
```

   
<a id="ID4ELD"></a>

 
## <a name="required-response-headers"></a>所需的响应标头
 
无。
  
<a id="ID4ESD"></a>

 
## <a name="response-body"></a>响应正文
 
如果在调用成功，该服务将返回一个具有以下成员的 JSON 对象。
 
| 成员| 说明| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 主机名| 实例的主机名。| 
| portMappings| 端口映射中。| 
| 区域| 区域实例中托管。| 
| secureContext| 安全设备地址。| 
 
<a id="ID4ESE"></a>

 
### <a name="sample-response"></a>示例响应
 

```cpp
{
        "hostName": "r111ybf4drgo12kq25tc-082yo7y9sz72f2odtq1ya5yhda-155169995-ncus.cloudapp.net",
        "portMappings": [
        {
        "Key": "GSDKTCPTest",
        "Value": {
        "internal": 10100,
        "external": 10103
        }
        },
        {
        "Key": "GSDKUDPTest",
        "Value": {
        "internal": 5000,
        "external": 5000
        }
        }
        ],
        "region": "WestUS",
        "secureContext": "AQDc8Hen/QCDJwWRPcW/1QEEAiABAACdOJU8JNujcXyUPwUBCnue+g=="
        }
      
```

   
<a id="remarks"></a>

 
## <a name="remarks"></a>备注
 
收到以下响应代码时，游戏应仅重试对服务调用：
 
   * 200 — 成功-返回响应。
   * 400-参数无效或格式不正确的请求正文。
   * 401-未授权
   * 404-主题作品 id 不具有任何订阅分配给它。
   * 409 — 相同的请求大约在同一时间进行 (相同 sessionId)，此响应时，可以。 如果分配请求和会话主机已指定的 sessionId 和已处于活动状态，我们将返回有关该 sessionhost 详细信息。 如果会话主机，但是，并不活动状态，但你将收到冲突。
   * 500-意外的服务器错误。
   * 503 — 无 sessionhosts StandingBy。 当这些资源的一些是免费重试请求。
   
<a id="ID4EFG"></a>

 
## <a name="see-also"></a>另请参阅
 [/titles/{titleId}/sessionhosts](uri-titlestitleidsessionhosts.md)

  