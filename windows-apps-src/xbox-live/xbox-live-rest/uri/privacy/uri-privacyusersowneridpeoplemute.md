---
title: /users/ {ownerId} / 人/静音
assetID: efb929d8-79a7-83f0-c348-c92ced42bc05
permalink: en-us/docs/xboxlive/rest/uri-privacyusersowneridpeoplemute.html
author: KevinAsgari
description: " /users/ {ownerId} / 人/静音"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a5de74be5e82fde007d6680eaf4c9e5a543afc64
ms.sourcegitcommit: 2a63ee6770413bc35ace09b14f56b60007be7433
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/12/2018
ms.locfileid: "3931665"
---
# <a name="usersowneridpeoplemute"></a>/users/ {ownerId} / 人/静音
访问用户的静音列表。

  * [URI 参数](#ID4EQ)

<a id="ID4EQ"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 类型| 说明|
| --- | --- | --- |
| ownerId| 字符串| 必需。 正在访问其资源的用户的标识符。 可能的值为"me" <code>xuid({xuid})</code>，或 gt({gamertag})。 必须经过身份验证的用户。 示例值： <code>xuid(2603643534573581)</code>， <code>gt(SomeGamertag)</code>。 最大大小： none。 |

<a id="ID4ETB"></a>


## <a name="valid-methods"></a>有效的方法

[获取 （/users/ {ownerId} / 人/静音）](uri-privacyusersowneridpeoplemuteget.md)

&nbsp;&nbsp;获取用户的静音的列表。

<a id="ID4E4B"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4E6B"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[隐私 Uri](atoc-reference-privacyv2.md)
