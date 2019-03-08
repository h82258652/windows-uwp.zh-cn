---
title: BatchRequest (JSON)
assetID: 2ca34506-8801-efc5-7c83-3c9ec5572276
permalink: en-us/docs/xboxlive/rest/json-batchrequest.html
description: " BatchRequest (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 51073c71700613edcd7c22e18cc0c00a9222d7e5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57654152"
---
# <a name="batchrequest-json"></a>BatchRequest (JSON)
要用于筛选状态显示信息，例如用户、 设备和标题的属性的数组。
<a id="ID4EN"></a>


## <a name="batchrequest"></a>BatchRequest

BatchRequest 对象具有以下规范。

| 成员| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- |
| 用户| 字符串数组| 用户想要了解，最多一次 1100 XUIDs 其存在状态的列表 XUIDs。|
| deviceTypes| 字符串数组| 你想要了解有关的用户使用的设备类型的列表。 如果数组为空，则默认为所有可能的设备类型 （即，不过滤掉）。|
| 标题| 32 位无符号整数数组| 你想要了解有关其的用户类型的设备的列表。 如果数组为空，则默认为所有可能的标题 （即，不过滤掉）。|
| level| 字符串| 可能值： <ul><li>用户-获取用户节点</li><li>设备-获取用户和设备节点</li><li>标题-获取基本标题级别信息</li><li>全部-获取丰富的状态显示信息、 媒体信息和 / 或</li></ul>默认值为"title"。| 
| onlineOnly| 布尔值| 如果此属性为 true，批处理操作将筛选出为脱机用户 （包括掩蔽的） 记录。 如果未提供，将返回联机和脱机用户。|

<a id="ID4EAD"></a>


## <a name="sample-json-syntax"></a>示例 JSON 语法


```json
{
  users:
  [
    "1234567890",
    "4567890123",
    "7890123456"
  ]
}


```


<a id="ID4EJD"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4ELD"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[JavaScript 对象表示法 (JSON) 对象引用](atoc-xboxlivews-reference-json.md)


<a id="ID4EXD"></a>


##### <a name="reference"></a>参考   
