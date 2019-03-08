---
title: GET (/media/{marketplaceId}/fields)
assetID: 1d535344-8522-0fd1-4daa-cd0f0a0f1121
permalink: en-us/docs/xboxlive/rest/uri-medialocalefieldsget.html
description: " GET (/media/{marketplaceId}/fields)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: cc3ae8abfe04b7a0b9728d07b9488f9ed7c27816
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57606672"
---
# <a name="get-mediamarketplaceidfields"></a>GET (/media/{marketplaceId}/fields)
获取字段的标记。 这些 Uri 的域是`eds.xboxlive.com`。
 
  * [备注](#ID4EV)
  * [URI 参数](#ID4EGC)
  * [查询字符串参数](#ID4ERC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>备注
 
娱乐发现服务 (EDS) Api，默认情况下，将返回一组很少最小的每个项的字段：
 
   * 媒体项类型
   * 媒体组
   * ID
   * 名称
  
若要获取详细信息，Api 接受**字段**参数，用于指定应返回数据的其他部分。 由于有许多可能的字段，每个 API 调用的完全指定其名称将极大地膨胀，请求。 相反，名称可传递到此 API，这会导致生成可以传递到其他 Api 的更小的值。
 
对于任何接受此参数的 API，提供的值必须是指定的介质的所有项类型中的所有字段的超集。 不能指定不同的字段的不同的媒体项类型集。 但是，如果字段应用于一个媒体项类型，但不是另一个，它才会出现在媒体项类型存在数据 (例如，如果"AvatarBodyType"包含在调用[获取 (/media/ {marketplaceId} / 字段)](uri-medialocalefields.md)、 仅AvatarItems 将包含该字段）。
 
此 API 返回的值是高度可缓存-实际上，它们不应更改除 EDS 部署之间。 建议，如果需要缓存，则缓存持续时间不超过用户会话。
 
除了接受的实际字段名称，此 API 接受"all"作为有效的值。 这将生成一个包含可以指定每个字段的值。 只能用于开发、 调试和测试目的被建议使用"全部"的值。
 
或者，可以发送`desired={list of fields separated by a '.'}`接受任何 api**字段**令牌。
 
不能在这种传递**需**并**字段**在一起。
  
<a id="ID4EGC"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| marketplaceId| 字符串| 必需。 从获取值的字符串<b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>。| 
  
<a id="ID4ERC"></a>

 
## <a name="query-string-parameters"></a>查询字符串参数
 
| 参数| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | --- | --- | --- | 
| 所需的| 字符串| 必需。 '。-应返回除了最小集的字段的分隔列表。 指定的不是所有字段都保证返回每个对象 （数据可能只是不存在的某些项），但将返回最小集之外未在此处指定的任何字段。 | 
  
<a id="ID4EMD"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EOD"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[/media/{marketplaceId}/fields](uri-medialocalefields.md)

  
<a id="ID4EYD"></a>

 
##### <a name="further-information"></a>详细信息 

[EDS 常见标头](../../additional/edscommonheaders.md)

 [EDS 参数](../../additional/edsparameters.md)

 [EDS 查询精简将](../../additional/edsqueryrefiners.md)

 [Marketplace Uri](atoc-reference-marketplace.md)

 [其他参考](../../additional/atoc-xboxlivews-reference-additional.md)

   