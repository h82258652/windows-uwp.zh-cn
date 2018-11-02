---
title: Achievement (JSON)
assetID: d3b52f66-ddc7-e676-b419-82209caf71d6
permalink: en-us/docs/xboxlive/rest/json-achievementv2.html
author: KevinAsgari
description: " Achievement (JSON)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 12a9cee88a2fdc5b0244119399620c3448f54f75
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/02/2018
ms.locfileid: "5923241"
---
# <a name="achievement-json"></a>Achievement (JSON)
成就对象 （版本 2）。
<a id="ID4EN"></a>


## <a name="achievement"></a>成就

成就对象具有以下规范。 所有成员都是必需的。

| 成员| 类型| 描述|
| --- | --- | --- |
| id| 字符串| 资源标识符。|
| serviceConfigId| 字符串| 此资源的 SCID。 标识与相关此成就的标题。 |
| name| 字符串| 本地化的成就名称。|
| titleAssociations| [TitleAssociation](json-titleassociation.md)的数组| TitleAssociation 数组。|
| progressState| **ProgressState**枚举| 进度的状态： <ul><li>无效 (0): 成就进度处于未知状态。</li><li>实现 (1): 已解锁成就。</li><li>(2) 进行： 成就处于锁定状态，但用户已解锁它的进度。</li><li>notStarted (3): 成就处于锁定状态和用户创建不了任何进度解锁它。</li></ul> | 
| 进度| [进度](json-progression.md)| 用户的成就内的进度。|
| mediaAssets| [MediaAsset](json-mediaasset.md)的数组| 与成就，如图像 Id 关联的媒体资产。 |
| 平台| 字符串| 平台成就已赢得上。|
| isSecret| 布尔值| 成就是否机密。|
| description| 字符串| 当解锁成就的说明。|
| lockedDescription| 字符串| 成就解锁之前的说明。|
| productId| 字符串| 成就的 ProductId 一起发布。|
| achievementType| **AchievementType**枚举| （不与相同传统的成就的以前类型） 的成就类型： <ul><li>无效 (0): 未知和不受支持的成就类型。</li><li>永久性 (1): 没有结束日期，可以随时解锁成就。</li><li>(2) 的挑战： 具有特定时段期间，它可以是一种解锁成就。</li></ul> |
| participationType| **ParticipationType**枚举| 成就参与类型。 有效值为个人或组。|
| timeWindow| TimeWindow| 在此期间可能会解锁成就的时段。 仅支持挑战。|
| 奖励| [奖励](json-reward.md)的数组| 解锁时获得的奖励的集合。|
| estimatedTime| TimeSpan| 估计的时间成就将需要获得。|
| deeplink| 字符串| 到游戏 deeplink。|
| isRevoked| 布尔值| 无论成就被吊销强制执行。|

<a id="ID4EIAAC"></a>


## <a name="sample-json-syntax"></a>JSON 语法示例


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
                "url":"http://www.xbox.com"
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

[JavaScript 对象表示法 (JSON) 对象参考](atoc-xboxlivews-reference-json.md)
