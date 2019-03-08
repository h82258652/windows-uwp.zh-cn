---
title: MediaAsset (JSON)
assetID: 858c720a-1648-738b-bb43-f050e7cac19e
permalink: en-us/docs/xboxlive/rest/json-mediaasset.html
description: " MediaAsset (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 308a65b7c049a6aba0405865bab63fb9d28b8506
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623472"
---
# <a name="mediaasset-json"></a>MediaAsset (JSON)
与成就或其奖励相关联的媒体资产。
<a id="ID4EN"></a>


## <a name="mediaasset"></a>MediaAsset

MediaAsset 对象具有以下规范。

| 成员| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- |
| name| 字符串| MediaAsset，如"tile01"的名称。|
| type| MediaAssetType 枚举| 媒体资产类型： <ul><li>图标 (0):成就图标中。</li><li>art (1):数字艺术资产中。</li></ul> | 
| url| 字符串| MediaAsset 的 URL。|

<a id="ID4EFC"></a>


## <a name="sample-json-syntax"></a>示例 JSON 语法


```json
{
  "name":"Icon Name",
  "type":"Icon",
  "url":"https://www.xbox.com"
}

```


<a id="ID4EOC"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4EQC"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[JavaScript 对象表示法 (JSON) 对象引用](atoc-xboxlivews-reference-json.md)
