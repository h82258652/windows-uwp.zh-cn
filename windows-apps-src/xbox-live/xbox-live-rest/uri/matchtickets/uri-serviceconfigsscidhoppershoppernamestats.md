---
title: /serviceconfigs/{scid}/hoppers/{name}/stats
assetID: 56bb4398-445b-e8c5-a4ce-1651576ee7e7
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidhoppershoppernamestats.html
description: " /serviceconfigs/{scid}/hoppers/{name}/stats"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 04376505b76296e052ea431f2a4e5fcfeac7b9e4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613932"
---
# <a name="serviceconfigsscidhoppersnamestats"></a>/serviceconfigs/{scid}/hoppers/{name}/stats

支持 GET 操作用于检索 hopper 的统计信息。

> [!IMPORTANT]
> 此 URI 适用于使用具有协定 103 或更高版本，并需要标头元素的 X Xbl 协定版本：103 或更高版本上的每个请求。

<a id="ID4ER"></a>


## <a name="domain"></a>域
momatch.xboxlive.com  
<a id="ID4EW"></a>


## <a name="remarks"></a>备注
此 URI 支持值 xuid、 gt、 和我的目标用户的配置中的所有者标识符。 只有票证的创建者可以删除该票证或检索的 URI 的状态。  
<a id="ID4E6"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- | --- |
| scid| GUID| 服务配置 (SCID) 会话标识符。|
| name| 字符串| Hopper 的名称。|

<a id="ID4EEC"></a>


## <a name="valid-methods"></a>有效的方法

[GET (/serviceconfigs/{scid}/hoppers/{name}/stats)](uri-serviceconfigsscidhoppershoppernamestatsget.md)

&nbsp;&nbsp;Hopper 获取统计信息。

<a id="ID4EQC"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4ESC"></a>


##### <a name="parent"></a>Parent 的子磁盘）  

[匹配 Uri](atoc-reference-matchtickets.md)
