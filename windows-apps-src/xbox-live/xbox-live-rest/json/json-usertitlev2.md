---
title: UserTitle (JSON)
assetID: 3e5767af-5704-8463-696b-42a7d2a1e231
permalink: en-us/docs/xboxlive/rest/json-usertitlev2.html
description: " UserTitle (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 33901a5bde25fd17072c2b45d587a33209424378
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623022"
---
# <a name="usertitle-json"></a>UserTitle (JSON)
包含用户标题数据。 
<a id="ID4EN"></a>

 
## <a name="usertitle"></a>UserTitle
 
UserTitle 对象具有以下规范。 所有属性都是必需的。
 
| 成员| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| lastUnlock| DateTime| 上次成就的时间。| 
| titleId| 32 位无符号的整数| 标题的唯一标识符。| 
| titleVersion| 字符串| 标题的版本。| 
| serviceConfigId| 字符串| 与标题关联的主服务配置集的 ID。| 
| titleType| 字符串| 标题类型中。| 
| 平台| 字符串| 受支持的平台。| 
| name| 字符串| 此标题的文本名称。 最大长度 22。| 
| earnedAchievements| 32 位无符号的整数| 获得的成就数获得的标题，其中包括解锁的成就和成功完成的挑战。| 
| currentGamerscore| 32 位无符号的整数| 总 gamerscore 此标题中获得此用户。| 
| maxGamerscore| 32 位无符号的整数| 此标题总可能 gamerscore。| 
  
<a id="ID4EFE"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EHE"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[JavaScript 对象表示法 (JSON) 对象引用](atoc-xboxlivews-reference-json.md)

   