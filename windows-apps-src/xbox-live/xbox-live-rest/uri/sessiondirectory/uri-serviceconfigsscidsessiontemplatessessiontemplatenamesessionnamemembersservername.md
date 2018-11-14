---
title: /serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/servers/{server-name}
assetID: aed0764f-4e3d-e0b3-1ea0-543c32f3f822
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionnamemembersservername.html
author: KevinAsgari
description: " /serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/servers/{server-name}"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: d799db37aef00aaaa7a992b2bda5cb535a12b569
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2018
ms.locfileid: "6266780"
---
# <a name="serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnameserversserver-name"></a>/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/servers/{server-name}
支持删除操作，以删除指定的会话的服务器。
<a id="ID4EO"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 类型| 说明|
| --- | --- | --- |
| scid| GUID| 服务配置标识符 (SCID)。 第 1 部分会话标识符。|
| sessionTemplateName| 字符串| 会话模板的当前实例的名称。 第 2 部分会话标识符。|
| 会话名| GUID| 在会话的唯一 ID。 会话标识符的第 3 部分。| 

<a id="ID4E3B"></a>


## <a name="valid-methods"></a>有效的方法

[DELETE (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/servers/{server-name})](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionnamemembersservernamedelete.md)

&nbsp;&nbsp;从会话中删除指定的服务器。

<a id="ID4EGC"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4EIC"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[会话目录 URI](atoc-reference-sessiondirectory.md)
