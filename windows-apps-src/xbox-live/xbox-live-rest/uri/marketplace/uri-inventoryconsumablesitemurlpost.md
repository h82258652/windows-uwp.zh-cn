---
title: POST ({itemID})
assetID: 2c3c166b-e638-cfb9-d68e-9f8ab9a838d3
permalink: en-us/docs/xboxlive/rest/uri-inventoryconsumablesitemurlpost.html
description: " POST ({itemID})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 877986ce9d48269295a68dbfd644f14785916b88
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640852"
---
# <a name="post-itemid"></a>POST ({itemID})
指示是否已使用全部或部分可使用清单项并减少可使用的所需的数量。
这些 Uri 的域是`inventory.xboxlive.com`。

  * [备注](#ID4EX)
  * [URI 参数](#ID4EQB)
  * [请求正文](#ID4E2B)
  * [响应正文](#ID4ENC)

<a id="ID4EX"></a>


## <a name="remarks"></a>备注

   * 如果调用方需要使用的数量超出了剩余的项提供，该调用将被拒绝。
   * 调用方需要使用的数量必须大于 0 的正整数。 使用值为 0 或更低的调用将被拒绝。
   * 如果调用方提供了一个空的事务 ID，将拒绝该请求。
   * 如果可用，以便将有可能以确定哪些标题报告使用情况将记录标题声明。
   * 将忽略某个时间段内的使用相同的 transactionId 的其他文章。


> [!NOTE]
> <b>X xbl 约定版本标头</b>此 API 为"4"。


<a id="ID4EQB"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- | --- |
| itemID| 字符串| 唯一的单数形式的清单项的每个用户 ID|

<a id="ID4E2B"></a>


## <a name="request-body"></a>请求正文

<a id="ID4EBC"></a>


### <a name="sample-request"></a>示例请求


```cpp
{
  "transactionId": String
  "removeQuantity": Int
}

```


删除 quantity 字段允许调用方以指示可使用者想要删除与可使用的剩余数量的数量。 事务 ID 字段提供了一种方法来重试限制两次计数相同的使用情况的风险时使用可使用内容的操作的调用方。

<a id="ID4ENC"></a>


## <a name="response-body"></a>响应正文

对开机自检，假定它将传递身份验证，并分配适当的授权上下文的响应是使用相同的 transactionId 的回执 acknolodgement 传递给在 POST 请求，可使用项的 URL 和项的新服务数量值。

<a id="ID4EVC"></a>


### <a name="sample-response"></a>示例响应


```cpp
{
  "transactionId": String
  "url": String
  "newQuantity": Int
}

```


<a id="ID4E6C"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4EBD"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[/users/me/consumables/{itemID}](uri-inventoryconsumablesitemurl.md)


<a id="ID4ELD"></a>


##### <a name="further-information"></a>详细信息

[EDS 常见标头](../../additional/edscommonheaders.md)

 [EDS 参数](../../additional/edsparameters.md)

 [EDS 查询精简将](../../additional/edsqueryrefiners.md)

 [Marketplace Uri](atoc-reference-marketplace.md)

 [其他参考](../../additional/atoc-xboxlivews-reference-additional.md)
