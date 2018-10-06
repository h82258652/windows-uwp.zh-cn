---
title: GET (/users/{ownerId}/clips)
assetID: da972b4e-bc38-66f5-2222-5e79d7c8a183
permalink: en-us/docs/xboxlive/rest/uri-usersowneridclipsget.html
author: KevinAsgari
description: " GET (/users/{ownerId}/clips)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a215e9e1abb6daad2c011f38d56c2e501bd16e40
ms.sourcegitcommit: 63cef0a7805f1594984da4d4ff2f76894f12d942
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/05/2018
ms.locfileid: "4388164"
---
# <a name="get-usersowneridclips"></a>GET (/users/{ownerId}/clips)
检索用户的剪辑的列表。
这些 Uri 的域是`gameclipsmetadata.xboxlive.com`和`gameclipstransfer.xboxlive.com`，则根据问题的 URI 的函数。

  * [备注](#ID4EX)
  * [URI 参数](#ID4EEB)
  * [查询字符串参数](#ID4EPB)
  * [请求正文](#ID4EPE)
  * [所需的响应标头](#ID4E1E)
  * [可选的响应标头](#ID4ENH)
  * [响应正文](#ID4EOAAC)
  * [相关的 Uri](#ID4EABAC)

<a id="ID4EX"></a>


## <a name="remarks"></a>备注

此 API 使的列表用户自己的剪辑以及其他用户的剪辑存储在服务中的不同方法。 多个入口点从不同的级别返回数据，并允许筛选通过查询参数。 如果声明中的 XUID 匹配 URI 中指定的所有者，然后在后内容隔离检查用户的剪辑将返回。 如果对 URI 中的所有者与 XUID 声明不匹配，然后指定的用户的剪辑是根据返回隐私检查和内容隔离检查针对请求的 XUID。

查询每位用户每个服务配置 id (scid) 进行了优化。 指定进一步筛选器或默认值以外的排序顺序如下所示可能在某些情况下需要更长的时间以返回。 这会更明显的较大的每个用户的视频集。

用于获取多个用户的列表，在相同的 API 调用中没有任何批处理 API。 从 SLS 设计师建议 （当前） 的模式是反之查询每个用户。

<a id="ID4EEB"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 类型| 描述|
| --- | --- | --- |
| ownerId| 字符串| 用户的用户身份的正在访问其资源。 支持的格式:"me"或"xuid(123456789)"。 最大长度： 16。|

<a id="ID4EPB"></a>


## <a name="query-string-parameters"></a>查询字符串参数

| 参数| 类型| 描述|
| --- | --- | --- | --- | --- | --- |
| skipItems| 32 位有符号整数| 可选。 返回的项目集合 （即，跳过 N 项目） 中的 N + 1 处开始。|
| ContinuationToken| 字符串| 可选。 返回在给定的延续令牌启动的项。 如果同时提供 continuationToken 参数优先于 skipItems。 换言之，如果存在 continuationToken 参数在 skipItems 参数将被忽略。 最大大小： 36。|
| maxItems| 32 位有符号整数| 可选。 要从集合 （可以使用 skipItems 和 continuationToken 返回一系列项相结合） 返回的项数的最大数量。 如果 maxItems 不存在，并且可能会返回 maxItems 少于 （即使尚未返回结果的最后一页），该服务可能会提供一个默认值。|
| 订单| Unicode 字符| 可选。 指定是否中 (D) escending 返回列表 （第一次最高值） 或 (A) scending （首次最低值） 顺序。 默认： d。|
| type| GameClipTypes| 可选。 组逗号分隔的剪辑返回的类型。 默认： 所有。|
| eventId| 字符串| 可选。 以逗号分隔的 eventIDs 筛选结果，则集。 默认： Null。|
| 限定符| 字符串| 可选。 指定要用于获取剪辑的顺序限定符。 <ul><li>创建-指定剪辑中的日期顺序返回到系统</li><li>评分-[顶级]-指定由他们的评分值返回剪辑</li><li>视图-[最查看]-指定剪辑返回的视图数</li></ul><br/> 最大大小： 12。 默认:"创建"。| 

<a id="ID4EPE"></a>


## <a name="request-body"></a>请求正文

没有此请求所需的成员。

<a id="ID4E1E"></a>


## <a name="required-response-headers"></a>所需的响应标头

| 标头| 类型| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X RequestedServiceVersion| 字符串| 生成此请求应定向到 Xbox LIVE 的服务的名称/数。 验证该标头、 身份验证令牌等中的声明的有效性后仅为请求路由到该服务。示例： 1，vnext。|
| Content-Type| 字符串| 响应正文的 MIME 类型。 示例：<b>应用程序/json</b>。|
| 缓存控制| 字符串| 若要指定缓存行为的礼貌请求。|
| 接受| 字符串| 内容类型的可接受的值。 示例：<b>应用程序/json</b>。|
| 重试后| 字符串| 指示客户端在不可用的服务器的情况下稍后重试。|
| 有所不同| 字符串| 指示下游代理如何缓存响应。|

<a id="ID4ENH"></a>


## <a name="optional-response-headers"></a>可选的响应标头

| 标头| 类型| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Etag| 字符串| 用于缓存优化。 示例:"686897696a7c876b7e"。|

<a id="ID4EOAAC"></a>


## <a name="response-body"></a>响应正文

<a id="ID4EUAAC"></a>


### <a name="sample-response"></a>示例响应


```cpp
{
       "gameClips":
       [
           {
               "xuid": "2716903703773872",
               "clipName": "[en-US] Localized Greatest Moment Name",
               "titleName": "[en-US] Localized Title Name",
               "gameClipLocale": "en-US",
               "gameClipId": "6873f6cf-af12-48a5-8be6-f3dfc3f61538-000",
               "state": "Published",
               "dateRecorded": "2013-06-14T01:02:55.4918465Z",
               "lastModified": "2013-06-14T01:05:41.3652693Z",
               "userCaption": "Set by user!",
               "type": "DeveloperInitiated",
               "source": "Console",
               "visibility": "Public",
               "durationInSeconds": 30,
               "scid": "00000000-0000-0012-0023-000000000070",
               "titleId": 354975,
               "rating": 3.75,
               "ratingCount": 245,
               "views": 7453,
               "titleData": "",
               "systemProperties": "",
               "savedByUser": false,
               "achievementId": "AchievementId",
               "greatestMomentId": "GreatestMomentId",
               "thumbnails": [
                   {
                       "uri": "http://localhost/users/xuid(2716903703773872)/scids/00000000-0000-0012-0023-000000000070/clips/6873f6cf-af12-48a5-8be6-f3dfc3f61538-000/thumbnails/large",
                       "fileSize": 637293,
                       "thumbnailType": "Large"
                   },
                   {
                       "uri": "http://localhost/users/xuid(2716903703773872)/scids/00000000-0000-0012-0023-000000000070/clips/6873f6cf-af12-48a5-8be6-f3dfc3f61538-000/thumbnails/small",
                       "fileSize": 163998,
                       "thumbnailType": "Small"
                   }
               ],
               "gameClipUris": [
                   {
                       "uri": "http://localhost/897f65a9-63f0-45a0-926f-05a3155c04fc/GameClip-Original_4000.ism/manifest",
                       "uriType": "SmoothStreaming",
                       "expiration": "2013-06-14T01:10:08.73652Z"
                   },
                   {
                       "uri": "http://localhost/897f65a9-63f0-45a0-926f-05a3155c04fc/GameClip-Original_4000.ism/manifest(format=m3u8-aapl)",
                       "uriType": "Ahls",
                       "expiration": "2013-06-14T01:10:08.73652Z"
                   },
                   {
                       "uri": "http://localhost/users/xuid(2716903703773872)/scids/00000000-0000-0012-0023-000000000070/clips/6873f6cf-af12-48a5-8be6-f3dfc3f61538-000",
                       "fileSize": 88820,
                       "uriType": "Download",
                       "expiration": "2999-12-31T11:59:40Z"
                   }
               ]
           }
       ],
   "pagingInfo":
       {
           "continuationToken": null,
           "totalItems": 1
       }
   }

```


<a id="ID4EABAC"></a>


## <a name="related-uris"></a>相关的 Uri

以下 URI 等同于在此文档中，但具有一个额外路径参数来指定 SCID 的主要通道。 将返回仅该用户的剪辑的 SCID。 请求的用户必须能够接触到请求的 SCID，否则 HTTP 403 将返回错误。

   * **/users/ {ownerId} /scids/ {scid} / 剪辑**

<a id="ID4ENBAC"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4EPBAC"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[/users/{ownerId}/clips](uri-usersowneridclips.md)
