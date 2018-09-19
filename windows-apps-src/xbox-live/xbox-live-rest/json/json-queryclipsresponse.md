---
title: QueryClipsResponse (JSON)
assetID: 5d668588-54d6-3cf3-20ad-bb2600a156b3
permalink: en-us/docs/xboxlive/rest/json-queryclipsresponse.html
author: KevinAsgari
description: " QueryClipsResponse (JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 4efe0e93527560e31a471fce2c74b1cc254101ad
ms.sourcegitcommit: 68fcac3288d5698a13dbcbd57f51b30592f24860
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/19/2018
ms.locfileid: "4057026"
---
# <a name="queryclipsresponse-json"></a>QueryClipsResponse (JSON)
包装返回的游戏剪辑，以及分页列表信息的列表。 
<a id="ID4EN"></a>

 
## <a name="queryclipsresponse"></a>QueryClipsResponse
 
QueryClipsResponse 对象具有以下规范。
 
| 成员| 类型| 说明| 
| --- | --- | --- | 
| <b>gameClips</b>| GameClip 的数组| 满足的请求限制 (<b>maxItems</b>) 查询的游戏剪辑的数组。| 
| <b>pagingInfo</b>| PagingInfo| 包含所需的延续任务和列表的后续调用分页的超过请求限制 (<b>maxItems</b>) 的信息。| 
  
<a id="ID4E2B"></a>

 
## <a name="sample-json-syntax"></a>JSON 语法示例
 

```json
{
 "gameClips": [
   {
     "id": "7ce5c1a7-1255-46d3-a90e-34a0e2dfab06",
     "xuid": "2716903703773872",
     "state": "Published", 
     "dateRecorded": "2012-12-23T12:00:00Z",
     "lastModified": "2012-10-31T10:45:00Z",
     "clipName": "Forza 4",
     "userCaption": "My awesome car flip",
     "eventId": "mymagicmomentABCDEFG",
     "type": "DeveloperInitiated, Achievement",
     "source": "TitleDirect",
     "visibility": "Default",
     "durationInSeconds": 30,
     "scid": "ecb5497e-76d4-4a8a-870d-e76a26306b7d",
     "rating": 1.0,
     "views": 5,
     "thumbnails": [
       {
         "uri": "http://gameclips.xbox.com/thumbnails/7ce5c1a7-1255-46d3-a90e-34a0e2dfab06/small.jpg",
         "fileSize": 123,
         "width": 120,
         "height": 250
       }
     ],
     "gameClipUris": [
       {
         "uri": "http://gameclips.xbox.com/clips/7ce5c1a7-1255-46d3-a90e-34a0e2dfab06/clip.mp4",
         "fileSize": 1234565,
         "uriType": "Download",
         "expiration": "9999-12-31T23:59:59.9999999"
       }
     ]
   },
   {
     "id": "7ce5c1a7-1255-46d3-a90e-34a0e2dfab06",
     "xuid": "2716903703773872",
     "state": "Published", 
     "dateRecorded": "2012-12-23T12:00:00Z",
     "lastModified": "2012-12-23T12:00:00Z",
     "clipName": "Halo 4 Magic Moment",
     "userCaption": "You've been pwn3d!",
     "eventId": "123456",
     "type": "Achievement",
     "source": "Console",
     "visibility": "Public",
     "durationInSeconds": 50,
     "scid": "d4dd889f-813f-42cf-ad2f-3063e6ded066",
     "rating": 0.5,
     "views": 5,
     "properties": "{ 'Boss': 'The Invincible' }",
     "systemProperties": "{ 'Id': '123456', 'Location': 'C:\\videos\\123456.mp4' }",
     "thumbnails": [
       {
         "uri": "http://gameclips.xbox.com/thumbnails/7ce5c1a7-1255-46d3-a90e-34a0e2dfab06/small.jpg",
         "fileSize": 123,
         "width": 120,
         "height": 250
       }
     ],
     "gameClipUris": [
       {
         "uri": "http://gameclips.xbox.com/clips/7ce5c1a7-1255-46d3-a90e-34a0e2dfab06/clip.mp4",
         "fileSize": 1234567,
         "uriType": "Download",
         "expiration": "9999-12-31T23:59:59.9999999"
       },
       {
         "uri": "http://gameclips.xbox.com/clips/7ce5c1a7-1255-46d3-a90e-34a0e2dfab06/manifest",
         "fileSize": 0,
         "uriType": "SmoothStreaming",
         "expiration": "2013-01-18T11:25:51.6522794Z"
       }
     ]
   }
 ],
 "pagingInfo": {
   "continuationToken": "",
   "totalItems": 2
 }
}
    
```

  
<a id="ID4EEC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EGC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[JavaScript 对象表示法 (JSON) 对象参考](atoc-xboxlivews-reference-json.md)

  
<a id="ID4ESC"></a>

 
##### <a name="reference"></a>参考   