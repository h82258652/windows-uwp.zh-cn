---
title: GET (/media/{marketplaceId}/fields)
assetID: 1d535344-8522-0fd1-4daa-cd0f0a0f1121
permalink: en-us/docs/xboxlive/rest/uri-medialocalefieldsget.html
author: KevinAsgari
description: " GET (/media/{marketplaceId}/fields)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 26d172c54dec94990918780bd67b9d47954f8552
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/09/2018
ms.locfileid: "6193290"
---
# <a name="get-mediamarketplaceidfields"></a>GET (/media/{marketplaceId}/fields)
获取字段的标记。 这些 Uri 的域是`eds.xboxlive.com`。
 
  * [备注](#ID4EV)
  * [URI 参数](#ID4EGC)
  * [查询字符串参数](#ID4ERC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>备注
 
娱乐发现服务 (EDS) Api，默认情况下返回一组很少最小的每个项目的字段：
 
   * 媒体项类型
   * 媒体组
   * ID
   * 姓名
  
若要获取详细信息，这些 Api 接受指定应返回的数据的其他部分的**字段**参数。 有许多可能的域，因为在每个 API 调用的完整中指定其名称将大大膨胀请求。 相反，可以将名称传递到此 API，这会生成可以传递到其他 Api 的很多较小的值。
 
对于接受此参数的任何 API，所提供的值必须中指定的媒体项的所有类型的所有字段超集。 不能指定不同的用于不同的媒体项类型的字段集。 但是，如果字段适用于一个媒体项类型，但不是另一个，它将仅出现在媒体项类型存在数据 (例如，如果"AvatarBodyType"包含在调用[GET (/media/ {marketplaceId} / 字段)]()，仅 AvatarItems 将包含字段）。
 
从该 API 返回的值是高度缓存-实际上，它们不应更改除外之间的 EDS 部署。 建议，如果需要缓存，缓存的最后一个不会超过用户的会话。
 
除了接受的实际的字段名称，此 API 接受"全部"作为有效的值。 这将生成包含可以指定每个字段的值。 使用"全部"值仅适合开发、 调试和测试目的。
 
或者，你可以发送`desired={list of fields separated by a '.'}`到接受**字段**令牌的任何 API。
 
不能一起传递**所需**和**字段**中。
  
<a id="ID4EGC"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 说明| 
| --- | --- | --- | 
| marketplaceId| 字符串| 必需。 字符串从<b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>获得的值。| 
  
<a id="ID4ERC"></a>

 
## <a name="query-string-parameters"></a>查询字符串参数
 
| 参数| 类型| 说明| 
| --- | --- | --- | --- | --- | --- | 
| 所需的| 字符串| 必需。 '。-分隔的应返回除了最小集的字段列表。 保证所有字段并非全都指定要返回的每个对象 （数据可能只需不存在某些项目），但不此处指定没有字段以外的最小集将会返回。 | 
  
<a id="ID4EMD"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EOD"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[/media/{marketplaceId}/fields](uri-medialocalefields.md)

  
<a id="ID4EYD"></a>

 
##### <a name="further-information"></a>详细信息 

[EDS 通用标头](../../additional/edscommonheaders.md)

 [EDS 参数](../../additional/edsparameters.md)

 [EDS 查询优化器](../../additional/edsqueryrefiners.md)

 [市场 URI](atoc-reference-marketplace.md)

 [其他参考](../../additional/atoc-xboxlivews-reference-additional.md)

   