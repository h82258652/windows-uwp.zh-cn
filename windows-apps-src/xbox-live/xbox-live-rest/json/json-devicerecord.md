---
title: DeviceRecord (JSON)
assetID: aca4f4d3-f9b4-8919-5b6d-5ae0fe11e162
permalink: en-us/docs/xboxlive/rest/json-devicerecord.html
author: KevinAsgari
description: " DeviceRecord (JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 0ae61df42706ea3ff3f52678feef8510974b5534
ms.sourcegitcommit: 63cef0a7805f1594984da4d4ff2f76894f12d942
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/05/2018
ms.locfileid: "4390730"
---
# <a name="devicerecord-json"></a>DeviceRecord (JSON)
有关设备，包括其类型和游戏在其上的信息。 
<a id="ID4EN"></a>

 
## <a name="devicerecord"></a>DeviceRecord
 
DeviceRecord 对象具有以下规范。
 
| 成员| 类型| 描述| 
| --- | --- | --- | 
| type| 字符串| 设备的设备类型。 可能性包括"D"、"Xbox360"、"MoLIVE"(Windows)、"WindowsPhone"、"WindowsPhone7"和"PC"(G4WL)。 如果的类型是未知 （适用于示例 iOS、 Android 或嵌入在 web 浏览器标题），则返回"Web"。| 
| 标题| [TitleRecord](json-titlerecord.md)的数组| 在此设备上的活动的标题的列表。| 
  
<a id="ID4EWB"></a>

 
## <a name="sample-json-syntax"></a>JSON 语法示例
 

```json
[{
    type:"D",
    titles:
    [{
      id:"12341234",
      name:"Contoso 5",
      state:"active",
      placement:"fill",
      timestamp:"2012-09-17T07:15:23.4930000",
      activity:
      {
        richPresence:"Team Deathmatch on Nirvana"
      }
    },
    {
      id:"12341235",
      name:"Contoso Waypoint",
      timestamp:"2012-09-17T07:15:23.4930000",
      placement:"snapped",
      state:"active",
      activity:
      {
        richPresence:"Using radar"
      }
    }]
  }]
    
```

  
<a id="ID4E6B"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EBC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[JavaScript 对象表示法 (JSON) 对象参考](atoc-xboxlivews-reference-json.md)

  
<a id="ID4ENC"></a>

 
##### <a name="reference"></a>参考   