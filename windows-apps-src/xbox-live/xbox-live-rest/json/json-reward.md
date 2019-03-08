---
title: Reward (JSON)
assetID: d1c92e8a-afbc-22c5-c0b5-6063963f8c4d
permalink: en-us/docs/xboxlive/rest/json-reward.html
description: " Reward (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1d58d34263e7e0e90091c41c1df4fd5e078f5055
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57607492"
---
# <a name="reward-json"></a>Reward (JSON)
这种成就与相关联的回报。
<a id="ID4EN"></a>


## <a name="reward"></a>奖励

奖励对象具有以下规范。

| 成员| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- |
| name| 字符串| 这种回报面向用户的名称。|
| description| 字符串| 这种回报面向用户的说明。|
| value| 字符串| 奖励的值。|
| type| RewardType 枚举| 奖励类型中： <ul><li>无效 (0):未知和不受支持的奖励类型已配置。</li><li>所有 (1):这种回报向播放机的所有点。</li><li>应用内 (2):定义和提供标题的回报。</li><li>Art (3):获得的回报是数字资产。</li></ul> | 
| valueType| ProgressValueDataType 枚举| 值的类型。 请参阅[要求 (JSON)](json-requirement.md)有关详细信息。|

<a id="ID4EBD"></a>


## <a name="sample-json-syntax"></a>示例 JSON 语法


```json
{
  "name":null,
  "description":null,
  "value":"10",
  "type":"Gamerscore",
  "valueType":"Int"
}

```


<a id="ID4EKD"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4EMD"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[JavaScript 对象表示法 (JSON) 对象引用](atoc-xboxlivews-reference-json.md)
