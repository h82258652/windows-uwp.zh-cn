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
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2018
ms.locfileid: "8888928"
---
# <a name="post-itemid"></a>POST ({itemID})
指示，已使用所有或易耗型库存项目的部分和递减所请求的量由该消耗品的数量。
这些 Uri 的域是`inventory.xboxlive.com`。

  * [备注](#ID4EX)
  * [URI 参数](#ID4EQB)
  * [请求正文](#ID4E2B)
  * [响应正文](#ID4ENC)

<a id="ID4EX"></a>


## <a name="remarks"></a>备注

   * 如果调用方要求占用的数量超过该项目的剩余电源，调用将被拒绝。
   * 调用方要求占用的数量必须大于 0 正整数。 消耗值为 0 或更低的调用将被拒绝。
   * 如果调用方提供一个空的事务 ID，将拒绝该请求。
   * 如果可用，以便它将能够确定哪些游戏报告消耗，将会记录的主题作品声明。
   * 使用相同的 transactionId 的其他文章将被忽略某个时间段。


> [!NOTE]
> 此 API <b>x xbl 协定版本标头</b>是"4"。


<a id="ID4EQB"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 类型| 描述|
| --- | --- | --- | --- |
| itemID| 字符串| 唯一的每个用户单数库存项目的 ID|

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


删除数量字段允许调用方指示他们想要删除该消耗品的剩余数量的易耗品的数量。 交易 ID 字段提供了一种方法重试限制两次计数相同的使用情况的风险时使用易耗型内容的操作与调用方。

<a id="ID4ENC"></a>


## <a name="response-body"></a>响应正文

POST，假设传递身份验证并分配适当授权上下文的响应是具有相同的 transactionId 收据 acknolodgement 传递给中 POST 请求、 在易耗型项的 URL 和项目的新的服务数量值。

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

[EDS 通用标头](../../additional/edscommonheaders.md)

 [EDS 参数](../../additional/edsparameters.md)

 [EDS 查询优化器](../../additional/edsqueryrefiners.md)

 [市场 URI](atoc-reference-marketplace.md)

 [其他参考](../../additional/atoc-xboxlivews-reference-additional.md)
