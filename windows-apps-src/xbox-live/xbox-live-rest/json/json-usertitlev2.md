---
title: UserTitle (JSON)
assetID: 3e5767af-5704-8463-696b-42a7d2a1e231
permalink: en-us/docs/xboxlive/rest/json-usertitlev2.html
author: KevinAsgari
description: " UserTitle (JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 068ae15566d73dfc4610f8540972b7e80329de8e
ms.sourcegitcommit: a160b91a554f8352de963d9fa37f7df89f8a0e23
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2018
ms.locfileid: "4125387"
---
# <a name="usertitle-json"></a>UserTitle (JSON)
包含用户的游戏数据。 
<a id="ID4EN"></a>

 
## <a name="usertitle"></a>UserTitle
 
UserTitle 对象具有以下规范。 所有属性都是必需的。
 
| 成员| 类型| 说明| 
| --- | --- | --- | 
| lastUnlock| DateTime| 上次成就的时间。| 
| titleId| 32 位无符号的整数| 游戏的唯一标识符。| 
| titleVersion| 字符串| 标题的版本。| 
| serviceConfigId| 字符串| 与游戏相关联的主要服务配置集 ID。| 
| 标题键入| 字符串| 游戏类型。| 
| 平台| 字符串| 受支持的平台。| 
| name| 字符串| 此标题文本名称。 最大长度 22。| 
| earnedAchievements| 32 位无符号的整数| 成就数而获得的游戏，包括已解锁的成就和成功完成挑战。| 
| currentGamerscore| 32 位无符号的整数| 在此游戏中获得此用户总玩家分数。| 
| maxGamerscore| 32 位无符号的整数| 此标题的总可能玩家分数。| 
  
<a id="ID4EFE"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EHE"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[JavaScript 对象表示法 (JSON) 对象参考](atoc-xboxlivews-reference-json.md)

   