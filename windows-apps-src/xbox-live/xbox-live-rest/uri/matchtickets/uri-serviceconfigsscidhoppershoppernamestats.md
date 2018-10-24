---
title: /serviceconfigs/{scid}/hoppers/{name}/stats
assetID: 56bb4398-445b-e8c5-a4ce-1651576ee7e7
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidhoppershoppernamestats.html
author: KevinAsgari
description: " /serviceconfigs/{scid}/hoppers/{name}/stats"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 0742ad87e68f7d0c6ed6346873aa3dd6c56edee0
ms.sourcegitcommit: 4b97117d3aff38db89d560502a3c372f12bb6ed5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2018
ms.locfileid: "5436387"
---
# <a name="serviceconfigsscidhoppersnamestats"></a>/serviceconfigs/{scid}/hoppers/{name}/stats

支持获取操作来检索漏斗的统计信息。

> [!IMPORTANT]
> 此 URI 旨在用于与合同 103 或更高版本，并且需要 X Xbl 协定版本的标头元素： 103 或更高版本上每个请求。

<a id="ID4ER"></a>


## <a name="domain"></a>域
momatch.xboxlive.com  
<a id="ID4EW"></a>


## <a name="remarks"></a>备注
此 URI 支持的值 xuid、 gt，和我的目标用户的配置中配置的所有者标识符。 只有的票证创建者可以删除票证或检索 URI 的状态。  
<a id="ID4E6"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 类型| 说明|
| --- | --- | --- | --- |
| scid| GUID| 服务配置标识符 (SCID) 会话。|
| name| 字符串| 漏斗的名称。|

<a id="ID4EEC"></a>


## <a name="valid-methods"></a>有效的方法

[GET (/serviceconfigs/{scid}/hoppers/{name}/stats)](uri-serviceconfigsscidhoppershoppernamestatsget.md)

&nbsp;&nbsp;漏斗中获取统计信息。

<a id="ID4EQC"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4ESC"></a>


##### <a name="parent"></a>Parent 的子磁盘）  

[匹配 URI](atoc-reference-matchtickets.md)
