---
title: /serviceconfigs/{scid}/batch
assetID: eb1b510f-d92e-ae9b-a3e6-0edf58b4f075
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidbatch.html
description: " /serviceconfigs/{scid}/batch"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 208ee92106563372dd4d92a8c800cc08f513e8c7
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589712"
---
# <a name="serviceconfigsscidbatch"></a>/serviceconfigs/{scid}/batch
支持在服务配置标识符级别批查询的 POST 操作。

> [!IMPORTANT]
> 此方法由 2015年之多人游戏，并且仅适用于该多玩家版本和更高版本。 它旨在用于具有模板协定 104/105 或更高版本，并需要标头元素的 X Xbl 协定版本：104/105 或更高版本上的每个请求。

<a id="ID4ER"></a>


## <a name="domain"></a>域
sessiondirectory.xboxlive.com  
<a id="ID4EW"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- | --- |
| scid| GUID| 服务配置标识符 (SCID)。 会话标识符的第 1 部分中。|

<a id="ID4ESB"></a>


## <a name="valid-methods"></a>有效的方法

[POST (/serviceconfigs/ {scid} / 批处理)](uri-serviceconfigsscidbatchpost.md)

&nbsp;&nbsp;上的服务配置的多个 Xbox 用户 Id 创建批查询中。

<a id="ID4E3B"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4E5B"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[会话目录 Uri](atoc-reference-sessiondirectory.md)
