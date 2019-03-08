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
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641542"
---
# <a name="usersowneridpeoplemute"></a>/users/{ownerId}/people/mute
访问用户的静音列表。

  * [URI 参数](#ID4EQ)

<a id="ID4EQ"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- |
| ownerId| 字符串| 必需。 其资源的访问的用户的标识符。 可能的值为"me" <code>xuid({xuid})</code>，或 gt({gamertag})。 必须是经过身份验证的用户。 示例值： <code>xuid(2603643534573581)</code>， <code>gt(SomeGamertag)</code>。 最大大小： 无。 |

<a id="ID4ETB"></a>


## <a name="valid-methods"></a>有效的方法

[GET (/users/{ownerId}/people/mute)](uri-privacyusersowneridpeoplemuteget.md)

&nbsp;&nbsp;获取用户静音的列表。

<a id="ID4E4B"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4E6B"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[隐私 Uri](atoc-reference-privacyv2.md)
