---
title: /serviceconfigs/ {scid} {hoppername} /hoppers/ /tickets/ {票证 id}
assetID: 25deb7fe-859c-01d2-d14f-455a36c08a7c
permalink: en-us/docs/xboxlive/rest/uri-scidhoppernameticketid.html
author: KevinAsgari
description: " /serviceconfigs/ {scid} {hoppername} /hoppers/ /tickets/ {票证 id}"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: dd7ed139fffb8bdb10ac5074d5e9725753678f1c
ms.sourcegitcommit: c8f6866100a4b38fdda8394ea185b02d7af66411
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/13/2018
ms.locfileid: "3959690"
---
# <a name="serviceconfigsscidhoppershoppernameticketsticketid"></a>/serviceconfigs/ {scid} {hoppername} /hoppers/ /tickets/ {票证 id}

支持的匹配票证的删除操作。

> [!IMPORTANT]
> 此 URI 旨在用于与合同 103 或更高版本，并且需要 X Xbl 协定版本的标头元素： 103 或更高版本上每个请求。

<a id="ID4ER"></a>


## <a name="domain"></a>域
momatch.xboxlive.com  
<a id="ID4EW"></a>


## <a name="remarks"></a>备注
此 URI 支持值 xuid、 gt，和我的目标用户的配置中配置的所有者标识符。  
<a id="ID4E2"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 类型| 说明|
| --- | --- | --- | --- |
| scid| GUID| 服务配置标识符 (SCID) 的会话。|
| name| 字符串| 漏斗的名称。|
| 票证 Id| GUID| 票证 id。|

<a id="ID4EJC"></a>


## <a name="valid-methods"></a>有效的方法

[删除 (/serviceconfigs/ {scid} {hoppername} /hoppers/ /tickets/ {票证 id})](uri-scidhoppernameticketiddelete.md)

&nbsp;&nbsp;删除匹配票证。

<a id="ID4ETC"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4EVC"></a>


##### <a name="parent"></a>Parent 的子磁盘）  

[匹配 Uri](atoc-reference-matchtickets.md)
