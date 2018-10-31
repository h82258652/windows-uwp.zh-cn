---
title: /serviceconfigs/{scid}/batch
assetID: eb1b510f-d92e-ae9b-a3e6-0edf58b4f075
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidbatch.html
author: KevinAsgari
description: " /serviceconfigs/{scid}/batch"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: f32b4d3198d073a13d48ef5ec0cbb817a2a29d6e
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2018
ms.locfileid: "5818598"
---
# <a name="serviceconfigsscidbatch"></a>/serviceconfigs/{scid}/batch
支持在服务配置标识符级别的批处理查询 POST 操作。

> [!IMPORTANT]
> 此方法使用 2015年多人游戏和应用仅向该多人游戏版本及更高版本。 它旨在用于使用模板合约 104/105 或更高版本，并且需要 X Xbl 协定版本的标头元素： 104/105 或更高版本上每个请求。

<a id="ID4ER"></a>


## <a name="domain"></a>域
sessiondirectory.xboxlive.com  
<a id="ID4EW"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 类型| 说明|
| --- | --- | --- | --- |
| scid| GUID| 服务配置标识符 (SCID)。 第 1 部分会话标识符。|

<a id="ID4ESB"></a>


## <a name="valid-methods"></a>有效的方法

[POST (/serviceconfigs/{scid}/batch)](uri-serviceconfigsscidbatchpost.md)

&nbsp;&nbsp;在服务配置的多个 Xbox 用户 Id 创建批处理查询。

<a id="ID4E3B"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4E5B"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[会话目录 URI](atoc-reference-sessiondirectory.md)
