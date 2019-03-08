---
title: Property (JSON)
assetID: 93de547e-d936-6fcc-92cb-e4dd284dd609
permalink: en-us/docs/xboxlive/rest/json-property.html
description: " Property (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7e2a721886509c49c60d663d491f8d49bc3c95e9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57659702"
---
# <a name="property-json"></a>Property (JSON)
包含客户端提供的匹配请求条件的属性数据。
<a id="ID4EN"></a>


## <a name="property"></a>属性

属性对象具有以下规范。

| 成员| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- |
| id| 字符串| 此属性的 id。|
| type| 32 位有符号的整数 | 属性的类型。 支持的值为： <ul><li>0 = integer</li><li>1 = 字符串</li></ul>| 
| value| 字符串| 此属性的值。|

<a id="ID4EGC"></a>


## <a name="sample-json-syntax"></a>示例 JSON 语法


```json
{
    "id":"1",
    "value":"20",
    "type":0
}

```


<a id="ID4EPC"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4ERC"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[JavaScript 对象表示法 (JSON) 对象引用](atoc-xboxlivews-reference-json.md)
