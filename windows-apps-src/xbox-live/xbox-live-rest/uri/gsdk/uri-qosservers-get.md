---
title: GET (/qosservers)
assetID: 8b940c1b-947c-eab3-78ed-4384f57ea0bd
permalink: en-us/docs/xboxlive/rest/uri-qosservers-get.html
description: " GET (/qosservers)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 02d24dbf1d189b759784dbbfa7052e2c218ec27e
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2018
ms.locfileid: "8890497"
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
 
| 标头| 值| 描述| 
| --- | --- | --- | 
| 内容类型| 应用程序/json| 提交的数据的类型。| 
| Host| gameserverds.xboxlive.com|  | 
| Content-Length|  | 请求对象的长度。| 
| x xbl 协定版本| 1| API 协定版本。| 
  
<a id="ID4EUC"></a>

 
## <a name="required-response-headers"></a>所需的响应标头
 
响应将始终包括下表中所示的标头。
 
| 标头| 值| 描述| 
| --- | --- | --- | --- | --- | --- | 
| 内容类型| 应用程序/json| 响应正文中的数据的类型。| 
| Content-Length|  | 响应正文的长度。| 
  
<a id="ID4EVD"></a>

 
## <a name="response-body"></a>响应正文
 
如果在调用成功，该服务将返回一个具有以下成员的 JSON 对象。
 
| 成员| 描述| 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| qosservers| 服务器信息的数组。| 
| serverFqdn| 完全限定的域名服务器的名称。| 
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

  