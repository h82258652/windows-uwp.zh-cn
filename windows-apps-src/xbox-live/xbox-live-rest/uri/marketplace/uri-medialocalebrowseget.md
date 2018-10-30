---
title: GET (media/{marketplaceId}/browse)
assetID: 024447a0-c615-e08b-f867-3b6c4c0db5dc
permalink: en-us/docs/xboxlive/rest/uri-medialocalebrowseget.html
author: KevinAsgari
description: " GET (media/{marketplaceId}/browse)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b5267444390fb1dde870423959c86a353e35d7af
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2018
ms.locfileid: "5753757"
---
# <a name="get-mediamarketplaceidbrowse"></a>GET (media/{marketplaceId}/browse)
允许浏览单个媒体组中的项。 这些 Uri 的域是`eds.xboxlive.com`。
 
  * [备注](#ID4EV)
  * [URI 参数](#ID4EFB)
  * [查询字符串参数](#ID4EQB)
  * [响应正文](#ID4E6B)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>备注
 
非连续而不使用延续令牌使用 skipItems 参数可以访问页面的搜索返回的数据。 此 API 将接受查询优化器。 
 
 **SandboxId**现在从 XToken 声明检索并强制执行。 如果**SandboxId**不存在，娱乐发现服务 (EDS) 将引发 400 错误请求错误。 
  
<a id="ID4EFB"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 说明| 
| --- | --- | --- | 
| marketplaceId| 字符串| 必需。 字符串从<b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>获得的值。| 
  
<a id="ID4EQB"></a>

 
## <a name="query-string-parameters"></a>查询字符串参数
 
此 API 接受以下查询参数： combinedContentRating，desiredMediaItemTypes，字段、 maxItems、 preferredProvider、 q、 queryRefiners、 skipItems、 firstPartyOnly、 freeOnly、 hasTrailer、 latestOnly、 subscriptionLevel，以及 topRatedOnly.
 
有关这些参数的详细信息，请参阅[EDS 参数](../../additional/edsparameters.md)。
  
<a id="ID4E6B"></a>

 
## <a name="response-body"></a>响应正文
 
<a id="ID4EFC"></a>

 
### <a name="sample-response"></a>示例响应
 
下面的代码 JSON 是为了响应在调用`/media/en-us/browse?orderBy=releaseDate&desiredMediaItemTypes=DGame&fields=all`。
 

```cpp
{
    "Items": [{
        "MediaGroup": "GameType",
        "MediaItemType": "DGame",
        "ID": "6c5402e4-3cd5-4b29-a9c4-bec7d2c7514a",
        "Name": "Tanks-A",
        "ReducedName": "Tanks-A short title",
        "ReleaseDate": "2013-05-14T00: 00: 00Z",
        "TitleId": "1676B413",
        "VuiDisplayName": "Tanks-A vui",
        "DeveloperName": "Microsoft Studios",
        "PublisherName": "Microsoft Studios",
        "SortName": "Tanks-A sort title. ",
        "KValue": "4",
        "KValueNamespace": "bingbox",
        "AllTimePlayCount": 30.0,
        "SevenDaysPlayCount": 30.0,
        "ThirtyDaysPlayCount": 30.0,
        "AllTimeRatingCount": 12.0,
        "ThirtyDaysRatingCount": 12.0,
        "SevenDaysRatingCount": 12.0,
        "AllTimeAverageRating": 0.8,
        "ThirtyDaysAverageRating": 0.8,
        "SevenDaysAverageRating": 0.8,
        "LegacyIds": [{
            "IdType": "TitleId",
            "Value": "1676B413"
        }],
        "Availabilities": [{
            "AvailabilityID": "",
            "ContentId": "c9fcb807-626d-5347-b918-65496f084219",
            "Devices": [{
                "Name": "Durango"
            }]
        }],
        "Packages": [{
            "CdnFileLocation": [{
                "SortOrder": null,
                "Uri": "www.microsoft.com"
            }],
            "ContentId": "c9fcb807-626d-5347-b918-65496f084219",
            "GameRegionMask": 1,
            "PackageType": "CAB",
            "LicenseBit": 1,
            "LicenseType": "SyncCast DTO"
        }],
        "SandboxId": "PART.Dev1"
    },
    {
        "MediaGroup": "GameType",
        "MediaItemType": "DGame",
        "ID": "fd16e2fb-eca4-4182-8f69-a98fdd6e57a1",
        "Name": "Vector based massively multiplayer Tanks game.",
        "ReducedName": "Tanks",
        "ReleaseDate": "2013-03-15T00: 00: 00Z",
        "TitleId": "69A60C76",
        "VuiDisplayName": "Tanks",
        "DeveloperName": "Microsoft Studios",
        "Images": [{
            "ID": "32ebd9fe-6a22-4111-954e-564ec69d802a",
            "ResizeUrl": "http: //images-eds.dnet.xboxlive.com/image?url=RIJNAEIo6.u.tudW9rXJ2lWDOsMikqfNiHE2Sp4qbgNbH6_drY8Ek2cyHXEnnUKPUXAH_m8a3Oe4_wpV7CkKA0Snc9puIYOGxsIfyyncTBv.MIluDZX6UqAPsJYHE5go_J_BBfxNWW6yrK4.K75aMQ--",
            "Purposes": ["Box_Art"],
            "Purpose": "Box_Art",
            "Height": 300,
            "Width": 219,
            "Order": 1
        },
        {
            "ID": "32ebd9fe-6a22-4111-954e-564ec69d802a",
            "ResizeUrl": "http: //images-eds.dnet.xboxlive.com/image?url=RIJNAEIo6.u.tudW9rXJ2lWDOsMikqfNiHE2Sp4qbgNbH6_drY8Ek2cyHXEnnUKPUXAH_m8a3Oe4_wpV7CkKA0Snc9puIYOGxsIfyyncTBv.MIluDZX6UqAPsJYHE5go_J_BBfxNWW6yrK4.K75aMQ--",
            "Purposes": ["Box_Art"],
            "Purpose": "Box_Art",
            "Height": 120,
            "Width": 85,
            "Order": 1
        },
        {
            "ID": "32ebd9fe-6a22-4111-954e-564ec69d802a",
            "ResizeUrl": "http: //images-eds.dnet.xboxlive.com/image?url=RIJNAEIo6.u.tudW9rXJ2lWDOsMikqfNiHE2Sp4qbgNbH6_drY8Ek2cyHXEnnUKPUXAH_m8a3Oe4_wpV7CkKA0Snc9puIYOGxsIfyyncTBv.MIluDZX6UqAPsJYHE5go_J_BBfxNWW6yrK4.K75aMQ--",
            "Purposes": ["Image"],
            "Purpose": "Image",
            "Height": 720,
            "Width": 1280,
            "Order": 1
        }],
        "PublisherName": "Microsoft Studios",
        "RatingId": "10",
        "ParentalRating": "E",
        "ParentalRatingSystem": "ESRB",
        "SortName": "\n            Vector based massively multiplayer Tanks game.\n          ",
        "Capabilities": [{
            "NonLocalizedName": "onlineMultiplayerMin",
            "Value": "3"
        },
        {
            "NonLocalizedName": "onelineMultiplayerMax",
            "Value": "5"
        }],
        "KValue": "5",
        "KValueNamespace": "bingbox",
        "AllTimePlayCount": 30.0,
        "SevenDaysPlayCount": 30.0,
        "ThirtyDaysPlayCount": 30.0,
        "AllTimeRatingCount": 12.0,
        "ThirtyDaysRatingCount": 12.0,
        "SevenDaysRatingCount": 12.0,
        "AllTimeAverageRating": 0.8,
        "ThirtyDaysAverageRating": 0.8,
        "SevenDaysAverageRating": 0.8,
        "LegacyIds": [{
            "IdType": "TitleId",
            "Value": "69A60C76"
        }],
        "Availabilities": [{
            "AvailabilityID": "",
            "ContentId": "7AE9DAB2-1162-488D-9F80-B804EC5AF879",
            "Devices": [{
                "Name": "Durango"
            }]
        }],
        "Packages": [{
            "CdnFileLocation": [{
                "SortOrder": null,
                "Uri": "https: //update.dnet.xboxlive.com/test_cdn/tanks-randomkey.xvc"
            },
            {
                "SortOrder": null,
                "Uri": "https: //update.dnet.xboxlive.com/test_cdn/tanks-randomkey.xvc"
            },
            {
                "SortOrder": null,
                "Uri": "https: //update.dnet.xboxlive.com/test_cdn/tanks-randomkey.xvc"
            }],
            "ContentId": "7AE9DAB2-1162-488D-9F80-B804EC5AF879",
            "PackageType": "XVC",
            "LicenseType": "Xbox Live Games Machine and User"
        }],
        "SandboxId": "PART.Dev1"
    }],
    "ImpressionGuid": "5d3085cf-7d17-43b4-a9c1-a1ccfe764eb1"
}
         
```

   
<a id="ID4EUC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EWC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[/media/{marketplaceId}/browse](uri-medialocalebrowse.md)

  
<a id="ID4EAD"></a>

 
##### <a name="further-information"></a>详细信息 

[EDS 通用标头](../../additional/edscommonheaders.md)

 [EDS 参数](../../additional/edsparameters.md)

 [EDS 查询优化器](../../additional/edsqueryrefiners.md)

 [市场 URI](atoc-reference-marketplace.md)

 [其他参考](../../additional/atoc-xboxlivews-reference-additional.md)

   