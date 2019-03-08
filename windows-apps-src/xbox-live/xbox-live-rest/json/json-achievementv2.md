---
title: Achievement (JSON)
assetID: d3b52f66-ddc7-e676-b419-82209caf71d6
permalink: en-us/docs/xboxlive/rest/json-achievementv2.html
description: " Achievement (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b0e20f46a0d97cba496df5c6fb9cda14fbeccccd
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628472"
---
# <a name="achievement-json"></a>Achievement (JSON)
成就对象 （第 2 版）。
<a id="ID4EN"></a>


## <a name="achievement"></a>成就

成就对象具有以下规范。 所有成员都是必需的。

| 成员| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- |
| id| 字符串| 资源标识符。|
| serviceConfigId| 字符串| 此资源 SCID。 标识与此成就的标题。 |
| name| 字符串| 本地化的成就名称。|
| titleAssociations| 数组[TitleAssociation](json-titleassociation.md)| TitleAssociation 的数组。|
| progressState| **ProgressState**枚举| 进度的状态： <ul><li>无效 (0):成就进度处于未知状态。</li><li>实现 (1):成就已解锁。</li><li>正在进行 (2):锁定成就，但用户已对其解除锁定的进度。</li><li>notStarted (3):成就已锁定，且用户尚未建立任何进度对其解除锁定。</li></ul> | 
| 进度| [进度](json-progression.md)| 在实现中的用户的进度。|
| mediaAssets| 数组[MediaAsset](json-mediaasset.md)| 与目标得以实现，如映像 Id 相关联的媒体资产。 |
| 平台| 字符串| 平台成就的获得上。|
| isSecret| 布尔值| 是否成就是机密。|
| description| 字符串| 成就解除锁定时的说明。|
| lockedDescription| 字符串| 成就之前未锁定的说明。|
| productId| 字符串| ProductId 成就随一起发布。|
| achievementType| **AchievementType**枚举| 成就 （不与相同旧成就上前一种类型） 的类型： <ul><li>无效 (0):未知和不受支持的成就类型。</li><li>永久性 (1):无结束日期，随时可以将其解锁某项成就。</li><li>挑战 (2):具有特定的时间窗口在此期间它可以解锁某项成就。</li></ul> |
| participationType| **ParticipationType**枚举| 成就参与类型。 有效值为个人或组。|
| timeWindow| TimeWindow| 在此期间实现可能将其解锁的时段。 仅支持挑战。|
| 奖励| 数组[奖励](json-reward.md)| 解除锁定时获得奖励的集合。|
| estimatedTime| TimeSpan| 估计的时间成就需要获得。|
| 深层链接| 字符串| 深层链接到标题。|
| isRevoked| 布尔值| 该值指示是否强制吊销成就。|

<a id="ID4EIAAC"></a>


## <a name="sample-json-syntax"></a>示例 JSON 语法


```json
{
        "id":"3",
        "serviceConfigId":"b5dd9daf-0000-0000-0000-000000000000",
        "name":"Default NameString for Microsoft Achievements Sample Achievement 3",
        "titleAssociations":
        [{
                "name":"Microsoft Achievements Sample",
                "id":3051199919,
                "version":"abc"
        }],
        "progressState":"Achieved",
        "progression":
        {
          "requirements":
          [{
            "id":"12345678-1234-1234-1234-123456789012",
            "current":null,
            "target":"100"
          }],
          "timeUnlocked":"2013-01-17T03:19:00.3087016Z",
        },
        "mediaAssets":
        [{
                "name":"Icon Name",
                "type":"Icon",
                "url":"https://www.xbox.com"
        }],
        "platform":"D",
        "isSecret":true,
        "description":"Default DescriptionString for Microsoft Achievements Sample Achievement 3",
        "lockedDescription":"Default UnachievedString for Microsoft Achievements Sample Achievement 3",
        "productId":"12345678-1234-1234-1234-123456789012",
        "achievementType":"Challenge",
        "participationType":"Individual",
        "timeWindow":
        {
                "startDate":"2013-02-01T00:00:00Z",
                "endDate":"2100-07-01T00:00:00Z"
        },
        "rewards":
        [{
                "name":null,
                "description":null,
                "value":"10",
                "type":"Gamerscore",
                "valueType":"Int"
        },
        {
                "name":"Default Name for InAppReward for Microsoft Achievements Sample Achievement 3",
                "description":"Default Description for InAppReward for Microsoft Achievements Sample Achievement 3",
                "value":"1",
                "type":"InApp",
                "valueType":"String"
        }],
        "estimatedTime":"06:12:14",
        "deeplink":"aWFtYWRlZXBsaW5r",
        "isRevoked":false
    }

```


<a id="ID4ERAAC"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4ETAAC"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[JavaScript 对象表示法 (JSON) 对象引用](atoc-xboxlivews-reference-json.md)
