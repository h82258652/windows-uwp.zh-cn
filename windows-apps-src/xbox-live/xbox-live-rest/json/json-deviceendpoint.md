---
title: DeviceEndpoint (JSON)
assetID: bd6c4af8-e491-8885-970e-e53d1d60642b
permalink: en-us/docs/xboxlive/rest/json-deviceendpoint.html
author: KevinAsgari
description: " DeviceEndpoint (JSON)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7dafe60c0d26846d10113a641986842cc52e0334
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2018
ms.locfileid: "5824271"
---
# <a name="deviceendpoint-json"></a>DeviceEndpoint (JSON)
 
<a id="ID4EO"></a>

 
## <a name="deviceendpoint"></a>DeviceEndpoint
 
DeviceEndpoint 对象具有以下规范。
 
| 成员| 类型| 说明| 
| --- | --- | --- | 
| 设备名称| 字符串| 可选。 设备，如果适用的友好名称。 目前不使用此值。| 
| endpointUri| 字符串| 必需。 客户端平台 （Windows 或 Windows Phone） 已获得从推送通知服务 （WNS 或 MPNS） URL。| 
| 区域设置| 字符串| 必需。 发送到此终结点的通知所需的语言。 可以按优先顺序的逗号分隔的列表。 示例:"DE-DE、 EN-US、 en"。| 
| 平台| 字符串| 可选。 当前受支持的值是"WindowsPhone"和"Windows"。 如果未指定，则将它派生的设备令牌。| 
| platformVersion| 字符串| 可选。 此字符串的格式是特定于每个平台。 目前不使用此值。| 
| systemId| GUID| 必需。 "应用"实例的唯一标识符 （设备/用户组合）。 最佳做法实施应用生成一个随机的 GUID 后安装/首次运行，并继续上后续的运行的应用使用该值。| 
| titleId| 32 位无符号的整数| 必需。 发出对服务调用的游戏的游戏 ID。| 
  
<a id="ID4EGD"></a>

 
## <a name="sample-json-syntax"></a>JSON 语法示例
 

```json
{
  "systemId": "e9af05b4-70de-4920-880f-026c6fb67d1b",
  "userId" : 1234567890
  "endpointUri": "http://cloud.notify.windows.com/.../",
  "platform": "Windows",
  "platformVersion": "1.0",
  "deviceName": "Test Endpoint",
  "locale": "en-US",
  "titleId": 1297290219
}
    
```

  
<a id="ID4EPD"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4ERD"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[JavaScript 对象表示法 (JSON) 对象参考](atoc-xboxlivews-reference-json.md)

  
<a id="ID4E4D"></a>

 
##### <a name="reference"></a>参考   