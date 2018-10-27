---
title: MediaAsset (JSON)
assetID: 858c720a-1648-738b-bb43-f050e7cac19e
permalink: en-us/docs/xboxlive/rest/json-mediaasset.html
author: KevinAsgari
description: " MediaAsset (JSON)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b50cb58f6c6262d653e90e1e6ca9666cecadf680
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2018
ms.locfileid: "5685688"
---
# <a name="mediaasset-json"></a>MediaAsset (JSON)
与成就或其奖励媒体资产。
<a id="ID4EN"></a>


## <a name="mediaasset"></a>MediaAsset

MediaAsset 对象具有以下规范。

| 成员| 类型| 说明|
| --- | --- | --- |
| name| 字符串| MediaAsset，如"tile01"的名称。|
| type| MediaAssetType 枚举| 媒体资产类型： <ul><li>图标 (0): 成就图标。</li><li>插图 (1): 数字艺术资源。</li></ul> | 
| url| 字符串| MediaAsset URL。|

<a id="ID4EFC"></a>


## <a name="sample-json-syntax"></a>JSON 语法示例


```json
{
  "name":"Icon Name",
  "type":"Icon",
  "url":"http://www.xbox.com"
}

```


<a id="ID4EOC"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4EQC"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[JavaScript 对象表示法 (JSON) 对象参考](atoc-xboxlivews-reference-json.md)
