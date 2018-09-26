---
title: GET (/qosservers)
assetID: 8b940c1b-947c-eab3-78ed-4384f57ea0bd
permalink: en-us/docs/xboxlive/rest/uri-qosservers-get.html
author: KevinAsgari
description: " GET (/qosservers)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 71a4787bf6b139d1a638ec783c0293d70a8ee239
ms.sourcegitcommit: e4f3e1b2d08a02b9920e78e802234e5b674e7223
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/26/2018
ms.locfileid: "4208584"
---
# <a name="get-qosservers"></a>GET (/qosservers)
URI 由客户端使用 Xbox Live 计算获取可用的 QoS 服务器的列表。 这些 Uri 的域是`gameserverds.xboxlive.com`和`gameserverms.xboxlive.com`。
 
  * [需的请求标头](#ID4EBB)
  * [所需的响应标头](#ID4EUC)
  * [响应正文](#ID4EVD)
 
<a id="ID5EG"></a>

 
## <a name="host-name"></a>主机名

gameserverds.xboxlive.com
 
<a id="ID4EBB"></a>

 
## <a name="required-request-headers"></a>需的请求标头
 
当发出请求下, 表中所示的标头是必需的。
 
| 标头| 值| 说明| 
| --- | --- | --- | 
| 内容类型| 应用程序/json| 正在提交的数据的类型。| 
| Host| gameserverds.xboxlive.com|  | 
| Content-Length|  | 请求对象的长度。| 
| x xbl 协定版本| 1| API 协定版本。| 
  
<a id="ID4EUC"></a>

 
## <a name="required-response-headers"></a>所需的响应标头
 
响应将始终会包括下表中所示的标头。
 
| 标头| 值| 说明| 
| --- | --- | --- | --- | --- | --- | 
| 内容类型| 应用程序/json| 响应正文中的数据的类型。| 
| Content-Length|  | 响应正文的长度。| 
  
<a id="ID4EVD"></a>

 
## <a name="response-body"></a>响应正文
 
如果在调用成功，该服务将返回一个具有以下成员的 JSON 对象。
 
| 成员| 说明| 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| qosservers| 服务器信息的数组。| 
| serverFqdn| 服务器的完全限定的域名。| 
| serverSecureDeviceAddress| 服务器的安全设备地址。| 
| targetLocation| 服务器的地理位置。| 
 
<a id="ID4EUE"></a>

 
### <a name="sample-response"></a>示例响应
 

```cpp
{ 
  "qosServers" : 
  [ 
    { "serverFqdn" : "xblqosncus.cloudapp.net", "serverSecureDeviceAddress" : "&lt;base-64 encoded blob>", "targetLocation" : "North Central US" },
    { "serverFqdn" : "xblqoswus.cloudapp.net", "serverSecureDeviceAddress" : "&lt;base-64 encoded blob>", "targetLocation" : "West US" },
  ]
}

      
```

   
<a id="ID4EBF"></a>

 
## <a name="see-also"></a>另请参阅
 [/qosservers](uri-qosservers.md)

  