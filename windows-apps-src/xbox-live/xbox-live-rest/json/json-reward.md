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
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2018
ms.locfileid: "8334365"
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
| type| RewardType 枚举| 奖励类型： <ul><li>无效 (0): 未知和不受支持的奖励类型已配置。</li><li>玩家分数 (1): 奖励将点添加到玩家的玩家分数。</li><li>inApp (2): 定义和游戏提供奖励。</li><li>插图 (3): 奖励是一项数字资产。</li></ul> | 
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
