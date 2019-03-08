---
title: POST (/handles/query)
assetID: a1a47d49-5c3f-8021-a213-13eb8bddf16a
permalink: en-us/docs/xboxlive/rest/uri-handlesquerypost.html
description: " POST (/handles/query)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7540c117931c01c24c79cef6c8ab6540cb65bbcb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651502"
---
# <a name="post-handlesquery"></a>POST (/handles/query)
创建查询的会话句柄。

> [!IMPORTANT]
> 此方法由 2015年之多人游戏，并且仅适用于该多玩家版本和更高版本。 它旨在用于具有模板协定 104/105 或更高版本，并需要标头元素的 X Xbl 协定版本：104/105 或更高版本上的每个请求。

  * [备注](#ID4ET)
  * [URI 参数](#ID4EDB)
  * [查询字符串参数](#ID4EQB)
  * [HTTP 状态代码](#ID4EBF)
  * [请求正文](#ID4EIF)
  * [响应正文](#ID4ETF)

<a id="ID4ET"></a>


## <a name="remarks"></a>备注

此 HTTP/REST 方法不包含任何会话信息创建仅限，句柄数据的查询。 两端可加**Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetActivitiesForSocialGroupAsync**。

此方法在请求正文中的类型字段必须设置为"活动"。 响应正文中的项直接映射到的属性**MultiplayerActivityDetails**。

<a id="ID4EDB"></a>


## <a name="uri-parameters"></a>URI 参数

<a id="ID4EQB"></a>


## <a name="query-string-parameters"></a>查询字符串参数

在下一个表中使用查询字符串参数，可以修改查询。

| <b>参数</b>| <b>Type</b>| <b>描述</b>|
| --- | --- | --- | --- |
| 关键字| 字符串| 一个关键字，例如，"foo"，若要检索必须在会话或模板中找到的。 |
| xuid| 64 位无符号的整数| Xbox 用户 ID，例如，"123"，对于要包含在查询中的会话。 默认情况下，用户必须是它要包含的会话中处于活动状态。 |
| 保留项| 布尔值| 为 true，包括会话为其用户设置为保留的播放机，但未联接变为活动状态的播放机。 当查询您自己的会话，或查询特定用户的会话的服务器时，才使用此参数。 |
| 非活动状态| 布尔值| 若要包括的用户已接受但不是播放会话，则为 true。 在其中用户已"就绪"，但不是"活动"的会话计数为非活动状态。 |
| 专用| 布尔值| 若要包含专用会话，则为 true。 当查询您自己的会话，或查询特定用户的会话的服务器时，才使用此参数。 |
| visibility| 字符串| 会话的可见性状态。 可能的值定义由<b>MultiplayerSessionVisibility</b>。 如果此参数设置为"打开"，该查询应包含仅打开的会话。 如果将其设置为"专用，"<i>专用</i>参数必须设置为 true。 |
| version| 32 位有符号的整数| 应包含会话最大版本。 例如，值 2 指定 2 的主要会话版本具有该唯一会话或小于应将包括在内。 版本号必须小于或等于请求的协定版本 mod 100。 |
| Take| 32 位有符号的整数| 若要检索的会话的数目上限。 此数字必须介于 0 和 100 之间。 |


设置*私有*或*预订*以 true 需要对该会话的服务器级访问权限。 或者，这些设置要求调用方的 XUID 声明以在 URI 中的 XUID 筛选器匹配。 否则，HTTP/403 状态代码返回，无论实际存在任何此类会话。

<a id="ID4EBF"></a>


## <a name="http-status-codes"></a>HTTP 状态代码
同样适用于 MPSD，服务将返回 HTTP 状态代码。  
<a id="ID4EIF"></a>


## <a name="request-body"></a>请求正文

有关用户的"收藏夹"社交图的所有活动，在请求正文如下所示：


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

句柄数组的句柄结构形式检索其嵌入每个结构中的唯一 ID。


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
