---
title: GET (/users/{ownerId}/clips)
assetID: da972b4e-bc38-66f5-2222-5e79d7c8a183
permalink: en-us/docs/xboxlive/rest/uri-usersowneridclipsget.html
description: " GET (/users/{ownerId}/clips)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7c52daf4a07914c34f1aadc84a7238771669d65f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623202"
---
# <a name="get-usersowneridclips"></a>GET (/users/{ownerId}/clips)
检索用户的剪辑列表。
这些 Uri 的域是`gameclipsmetadata.xboxlive.com`和`gameclipstransfer.xboxlive.com`，取决于 URI 相关的函数。

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

此 API 允许不同的方法来用户自己的剪辑以及其他用户的存储服务中的多个剪辑的列表。 多个入口点返回中不同级别的数据，并允许通过查询参数进行筛选。 如果在声明中的 XUID 与匹配的 URI 中指定的所有者，则用户自己的剪辑返回后内容隔离检查。 如果在 URI 中的所有者与声明 XUID 不匹配，则指定的用户的剪辑返回基于上隐私检查和针对请求 XUID 内容隔离检查。

每个用户的每个服务配置 id (scid) 优化查询。 进一步筛选器并或指定非默认的排序顺序为以下指定的内容可能在某些情况下需要更长时间才能返回。 这是对于较大的每个用户视频集更为明显。

没有任何批处理 API 用于获取相同的 API 调用中的多个用户的列表。 从 SLS 架构师建议 （当前） 的模式是反过来查询为每个用户。

<a id="ID4EEB"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- |
| ownerId| 字符串| 其资源的访问的用户的用户标识。 支持的格式:"me"或"xuid(123456789)"。 最大长度：16.|

<a id="ID4EPB"></a>


## <a name="query-string-parameters"></a>查询字符串参数

| 参数| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- | --- | --- | --- |
| skipItems| 32 位有符号的整数| 可选。 返回集合 （即，跳过 N 项） 中的 N + 1 开始的项。|
| ContinuationToken| 字符串| 可选。 返回从给定的继续标记处开始的项。 如果二者均提供继续标记参数优先于 skipItems。 换而言之，如果继续标记参数存在，则忽略 skipItems 参数。 最大大小：36.|
| maxItems| 32 位有符号的整数| 可选。 要从集合 （可以组合使用 skipItems 和 continuationToken 要返回的项的范围） 中返回的项的最大数目。 如果 maxItems 不存在，并且可能会返回 maxItems 少于 （即使尚未返回结果的最后一页），该服务可能会提供默认值。|
| 顺序| Unicode 字符| 可选。 指定是否在 (D) escending 中返回列表 （第一次最高值） 或 (A) 升序 （第一次最小值） 顺序。 Default：D.|
| type| GameClipTypes| 可选。 以逗号分隔的剪辑，以返回的类型集。 Default：所有。|
| eventId| 字符串| 可选。 以逗号分隔的 eventIDs 按筛选结果集。 Default：为 null。|
| 限定符| 字符串| 可选。 指定要用于获取剪辑的顺序限定符。 <ul><li>创建-指定剪辑的日期顺序以返回到系统</li><li>评级的 [最受欢迎的]-指定通过其评级值返回的剪辑</li><li>视图-[查看次数最多]-指定剪辑返回的视图数目</li></ul><br/> 最大大小：12. 默认值:"创建"。| 

<a id="ID4EPE"></a>


## <a name="request-body"></a>请求正文

没有为此请求所需的成员。

<a id="ID4E1E"></a>


## <a name="required-response-headers"></a>所需的响应标头

| 标头| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X-RequestedServiceVersion| 字符串| 生成此请求应定向到 Xbox LIVE 的服务的名称/编号。 验证标头中的身份验证令牌等的声明的有效性后仅为将请求路由到该服务。示例：1，vnext。|
| 内容类型| 字符串| 响应正文的 MIME 类型。 示例：<b>应用程序 /json</b>。|
| Cache-Control| 字符串| 请求正常，以指定缓存行为。|
| 接受| 字符串| 内容类型的可接受的值。 示例：<b>应用程序 /json</b>。|
| 重试间隔| 字符串| 指示客户端在不可用的服务器的情况下请稍后再试。|
| 改变| 字符串| 指示下游代理如何缓存响应。|

<a id="ID4ENH"></a>


## <a name="optional-response-headers"></a>可选的响应标头

| 标头| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Etag| 字符串| 用于缓存优化。 示例："686897696a7c876b7e"。|

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

下面的 URI 是等同于本文档中，但具有额外的 path 参数指定 SCID 主要的代码。 将返回有关该 SCID 仅该用户的剪辑。 发出请求的用户必须有权访问请求 SCID，否则 HTTP 403 将返回错误。

   * **/users/{ownerId}/scids/{scid}/clips**

<a id="ID4ENBAC"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4EPBAC"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[/users/{ownerId}/clips](uri-usersowneridclips.md)
