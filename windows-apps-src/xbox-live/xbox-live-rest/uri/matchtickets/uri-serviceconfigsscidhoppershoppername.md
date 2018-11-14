---
title: /serviceconfigs/{scid}/hoppers/{hoppername}
assetID: ba1e129d-b4c4-6535-46ce-fd184465c85f
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidhoppershoppername.html
author: KevinAsgari
description: " /serviceconfigs/{scid}/hoppers/{hoppername}"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 29bbde83fd5480575ff4f9ff6620ab3bf5a15797
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2018
ms.locfileid: "6252134"
---
# <a name="serviceconfigsscidhoppershoppername"></a>/serviceconfigs/{scid}/hoppers/{hoppername}

支持 POST 操作以创建匹配票证。

> [!IMPORTANT]
> 此 URI 用于合同 103 或更高版本，并且需要 X Xbl 协定版本的标头元素： 103 或更高版本上每个请求。

<a id="ID4ER"></a>


## <a name="domain"></a>域
momatch.xboxlive.com  
<a id="ID4EW"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 类型| 说明|
| --- | --- | --- | --- |
| scid| GUID| 服务配置标识符 (SCID) 会话。|
| hoppername | 字符串 | 漏斗的名称。 |

<a id="ID4E2B"></a>


## <a name="valid-methods"></a>有效的方法

[POST (/serviceconfigs/{scid}/hoppers/{hoppername})](uri-serviceconfigsscidhoppershoppernamepost.md)

&nbsp;&nbsp;创建指定的匹配票证。

<a id="ID4EFC"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4EHC"></a>


##### <a name="parent"></a>Parent 的子磁盘）  

[匹配 URI](atoc-reference-matchtickets.md)
