---
title: Reward (JSON)
assetID: d1c92e8a-afbc-22c5-c0b5-6063963f8c4d
permalink: en-us/docs/xboxlive/rest/json-reward.html
author: KevinAsgari
description: " Reward (JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 0ddecacdf77305b6c9449bd5e903a5e4c0fa74d7
ms.sourcegitcommit: d10fb9eb5f75f2d10e1c543a177402b50fe4019e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/12/2018
ms.locfileid: "4574296"
---
# <a name="reward-json"></a>Reward (JSON)
与成就关联的奖励。
<a id="ID4EN"></a>


## <a name="reward"></a>奖励

奖励对象具有以下规范。

| 成员| 类型| 说明|
| --- | --- | --- |
| name| 字符串| 奖励面向用户的名称。|
| description| 字符串| 奖励面向用户的描述。|
| 值| 字符串| 奖励的值。|
| type| RewardType 枚举| 奖励类型： <ul><li>无效 (0): 未知和不受支持的奖励类型已配置。</li><li>玩家分数 (1): 奖励将点添加到玩家的玩家分数。</li><li>inApp (2): 定义和游戏提供奖励。</li><li>插图 (3): 奖励是数字资产。</li></ul> | 
| valueType| ProgressValueDataType 枚举| 值的类型。 有关详细信息，请参阅[要求 (JSON)](json-requirement.md) 。|

<a id="ID4EBD"></a>


## <a name="sample-json-syntax"></a>JSON 语法示例


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

[JavaScript 对象表示法 (JSON) 对象参考](atoc-xboxlivews-reference-json.md)
