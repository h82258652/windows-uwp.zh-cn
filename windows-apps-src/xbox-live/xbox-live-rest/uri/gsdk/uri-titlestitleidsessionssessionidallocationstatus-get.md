---
title: GET (/titles/{titleId}/sessions/{sessionId}/allocationStatus)
assetID: 613ba53f-03cb-5ed3-a5ba-be59e5a146d1
permalink: en-us/docs/xboxlive/rest/uri-titlestitleidsessionssessionidallocationstatus-get.html
author: KevinAsgari
description: " GET (/titles/{titleId}/sessions/{sessionId}/allocationStatus)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1e351bed37e0761be1f884400f81a3da537967d2
ms.sourcegitcommit: 933e71a31989f8063b020746fdd16e9da94a44c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/11/2018
ms.locfileid: "4540497"
---
# <a name="get-titlestitleidsessionssessionidallocationstatus"></a>GET (/titles/{titleId}/sessions/{sessionId}/allocationStatus)
返回由其 sessionId sessionhost 分配状态。 这些 Uri 的域是`gameserverds.xboxlive.com`和`gameserverms.xboxlive.com`。
 
  * [需的请求标头](#ID4E4)
  * [所需的响应标头](#ID4EEB)
  * [响应正文](#ID4ELB)
 
<a id="ID4E4"></a>

 
## <a name="required-request-headers"></a>需的请求标头
 
无。
  
<a id="ID4EEB"></a>

 
## <a name="required-response-headers"></a>所需的响应标头
 
无。
  
<a id="ID4ELB"></a>

 
## <a name="response-body"></a>响应正文
 
如果在调用成功，该服务将返回一个具有以下成员的 JSON 对象。
 
| 成员| 说明| 
| --- | --- | 
| description| 返回空字符串 （左中的向后兼容性）。| 
| clusterId| 返回空字符串 （左中的向后兼容性）。| 
| 主机名| 会话主机的 URL。| 
| status| 指示排队、 已完成，或者中止。| 
| sessionHostId| 会话主机 id。| 
| sessionId| （在分配时） 提供的客户端会话 id。| 
| secureContext| 安全设备地址。| 
| portMappings| 该实例端口映射。| 
| 区域| 实例的位置。| 
| 票证 Id| 当前会话 ID （左中的向后兼容性）。| 
| gameHostId| 当前 sessionHostId （左中的向后兼容性）。| 
 
<a id="ID4EGD"></a>

 
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
        "status",:"Fulfilled",
        "region": "WestUS",
        "secureContext": "AQDc8Hen/QCDJwWRPcW/1QEEAiABAACdOJU8JNujcXyUPwUBCnue+g==",
        "sessionId": "05328154-1bbe-4f5b-8caa-4e44106712f9",
        "description": "",
        "clusterId": "",
        "sessionHostId": "r111ybf4drgo12kq25tc-082yo7y9sz72f2odtq1ya5yhda-155169995-ncus.GSDKAgent_IN_0.0",
        "ticketId": "05328154-1bbe-4f5b-8caa-4e44106712f9",
        "gameHostId": "r111ybf4drgo12kq25tc-082yo7y9sz72f2odtq1ya5yhda-155169995-ncus.GSDKAgent_IN_0.0"

      
```

  
<a id="remarks"></a>

 
### <a name="remarks"></a>备注
 
收到以下响应代码时，游戏应仅重试对服务调用：
 
   * 200 — 成功 
   * 400-请求包含无效参数 
   * 401-未授权 
   * 404-的主题作品 ID 或票证 ID 已无效，或未找到 
   * 500-意外的服务器错误。 
    