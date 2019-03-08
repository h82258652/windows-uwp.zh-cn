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
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632062"
---
# <a name="get-qosservers"></a>GET (/qosservers)
客户端以获取可用的 QoS 服务器的列表用于 Xbox Live 计算调用的 URI。 这些 Uri 的域是`gameserverds.xboxlive.com`和`gameserverms.xboxlive.com`。
 
  * [所需的请求标头](#ID4EBB)
  * [所需的响应标头](#ID4EUC)
  * [响应正文](#ID4EVD)
 
<a id="ID5EG"></a>

 
## <a name="host-name"></a>主机名

gameserverds.xboxlive.com
 
<a id="ID4EBB"></a>

 
## <a name="required-request-headers"></a>所需的请求标头
 
当发出请求，如下表中所示的标头是必需的。
 
| 标头| 值| 描述| 
| --- | --- | --- | 
| 内容类型| 应用程序/json| 正在提交的数据类型。| 
| 主机| gameserverds.xboxlive.com|  | 
| 内容长度|  | 请求对象的长度。| 
| x-xbl-contract-version| 1| API 协定版本。| 
  
<a id="ID4EUC"></a>

 
## <a name="required-response-headers"></a>所需的响应标头
 
响应将始终包括下表中所示的标头。
 
| 标头| 值| 描述| 
| --- | --- | --- | --- | --- | --- | 
| 内容类型| 应用程序/json| 响应正文中的数据类型。| 
| 内容长度|  | 响应正文的长度。| 
  
<a id="ID4EVD"></a>

 
## <a name="response-body"></a>响应正文
 
如果调用成功，服务将返回具有以下成员的 JSON 对象。
 
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

  