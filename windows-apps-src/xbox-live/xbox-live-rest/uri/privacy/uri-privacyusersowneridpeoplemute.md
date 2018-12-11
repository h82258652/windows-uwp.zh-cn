---
title: /users/{ownerId}/people/mute
assetID: efb929d8-79a7-83f0-c348-c92ced42bc05
permalink: en-us/docs/xboxlive/rest/uri-privacyusersowneridpeoplemute.html
description: " /users/{ownerId}/people/mute"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 86010d11ff0a7ebe188766f51bc3160a4f2befa4
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2018
ms.locfileid: "8899412"
---
# <a name="usersowneridpeoplemute"></a>/users/{ownerId}/people/mute
访问用户的静音列表。

  * [URI 参数](#ID4EQ)

<a id="ID4EQ"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 类型| 描述|
| --- | --- | --- |
| ownerId| 字符串| 必需。 正在访问其资源的用户的标识符。 可能的值为"我" <code>xuid({xuid})</code>，或 gt({gamertag})。 必须经过身份验证的用户。 示例值： <code>xuid(2603643534573581)</code>， <code>gt(SomeGamertag)</code>。 最大大小： none。 |

<a id="ID4ETB"></a>


## <a name="valid-methods"></a>有效的方法

[GET (/users/{ownerId}/people/mute)](uri-privacyusersowneridpeoplemuteget.md)

&nbsp;&nbsp;获取用户的静音的列表。

<a id="ID4E4B"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4E6B"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[隐私 URI](atoc-reference-privacyv2.md)
