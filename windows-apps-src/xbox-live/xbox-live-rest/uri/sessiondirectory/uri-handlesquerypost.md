---
title: POST (/handles/query)
assetID: a1a47d49-5c3f-8021-a213-13eb8bddf16a
permalink: en-us/docs/xboxlive/rest/uri-handlesquerypost.html
author: KevinAsgari
description: " POST (/handles/query)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 058612c7b8dc8b5576b471482655f28e17608481
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2018
ms.locfileid: "6248563"
---
# <a name="post-handlesquery"></a>POST (/handles/query)
创建查询会话句柄。

> [!IMPORTANT]
> 此方法使用 2015年多人游戏和应用仅向该多人游戏版本及更高版本。 它旨在用于使用模板合约 104/105 或更高版本，并且需要 X Xbl 协定版本的标头元素： 104/105 或更高版本上每个请求。

  * [备注](#ID4ET)
  * [URI 参数](#ID4EDB)
  * [查询字符串参数](#ID4EQB)
  * [HTTP 状态代码](#ID4EBF)
  * [请求正文](#ID4EIF)
  * [响应正文](#ID4ETF)

<a id="ID4ET"></a>


## <a name="remarks"></a>备注

此 HTTP/REST 方法创建查询句柄仅用于数据，而无需任何会话信息。 它可以通过**Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetActivitiesForSocialGroupAsync**换行。

对于此方法，请求正文中的类型字段必须设置为"活动"。 响应正文中的项直接映射到**MultiplayerActivityDetails**的属性。

<a id="ID4EDB"></a>


## <a name="uri-parameters"></a>URI 参数

<a id="ID4EQB"></a>


## <a name="query-string-parameters"></a>查询字符串参数

可以使用下表中的查询字符串参数修改查询。

| <b>参数</b>| <b>类型</b>| <b>说明</b>|
| --- | --- | --- | --- |
| 关键字| 字符串| 一个关键字，例如，"foo"，如果它们是要检索必须在会话或模板中找到的。 |
| xuid| 64 位无符号的整数| Xbox 用户 ID，例如，"123"，以在查询中包含的会话。 默认情况下，用户必须是它要包括在会话中处于活动状态。 |
| 预订| 布尔型| 若要包括的会话用户设置为保留玩家，但并未加入成为活动玩家。 在查询你自己的会话，或查询特定用户的会话服务器到服务器时，仅使用此参数。 |
| 处于非活动状态| 布尔型| 若要包括用户已接受但不会主动玩游戏的会话，则为 true。 计数为非活动状态的用户已"准备好"但不是"活动"的会话。 |
| 专用| 布尔型| 如果为 true，以包含专用会话。 在查询你自己的会话，或查询特定用户的会话服务器到服务器时，仅使用此参数。 |
| 可见性| 字符串| 会话的可见性状态。 由<b>MultiplayerSessionVisibility</b>定义可能的值。 如果此参数设置为"打开"，该查询应包括仅打开的会话。 如果它被设置为"private"，<i>专用</i>的参数必须设置为 true。 |
| version| 32 位有符号整数| 应包含最大会话版本。 例如，值为 2 指定主要的会话版本 2 的唯一会话或更少应包含。 版本号必须小于或等于请求的协定版本 mod 100。 |
| 参加| 32 位有符号整数| 若要检索的会话的最大数量。 此数字必须介于 0 和 100 之间。 |


设置为 true 的*私有*或*预订*需要对会话的服务器级访问权限。 此外，这些设置要求调用方的 XUID 声明以匹配 URI 中的 XUID 筛选器。 否则，HTTP/403 状态代码返回，无论确实存在任何此类会话。

<a id="ID4EBF"></a>


## <a name="http-status-codes"></a>HTTP 状态代码
该服务返回 HTTP 状态代码，因为它适用于 MPSD。  
<a id="ID4EIF"></a>


## <a name="request-body"></a>请求正文

有关用户的"收藏夹"社交图片的所有活动，请求正文中如下所示：


```cpp
{
  "type": "activity",
  "scid": "B5B1F71F-A328-4371-89E0-C3AD222D0E92"  // optional filter on scid
  "owners": {
    "people": {
      "moniker": "favorites",
      "monikerXuid": "3210"
    }
  }
}

```


<a id="ID4ETF"></a>


## <a name="response-body"></a>响应正文

句柄被检索的句柄结构数组具有嵌入在每个结构中的唯一 ID。


```cpp
{
  "results": [
    {
      "id": "11111111-ebe0-42da-885f-033860a818f6",
      "type": "activity",
      "version": 1,
      "sessionRef": {
        "scid": "8dfb0100-ebe0-42da-885f-033860a818f6",
        "templateName": "party",
        "name": "e3a836aeac6f4cbe9bcab985494d3175"
      },
      "titleId": "1234567",
      "ownerXuid": "3212",
      {
        "id": "11111111-ebe0-42da-885f-033860a818f7",
      "type": "activity",
      "version": 1,
      "sessionRef": {
         "scid": "8dfb0100-ebe0-42da-885f-033860a818f6",
        "templateName": "TitleStorageTestDefault",
        "name": "795fcaa7-8377-4281-bd7e-e86c12843632"
      },
      "titleId": "1234567",
      "ownerXuid": "3212",
    }
  }]
}

```


<a id="ID4E4F"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4E6F"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[/handles/query](uri-handlesquery.md)
