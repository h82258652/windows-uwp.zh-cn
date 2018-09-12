---
title: /serviceconfigs/ {scid} /sessiontemplates/ {sessionTemplateName} / 批处理
assetID: 4f8e1ece-2ba8-9ea4-e551-2a69c499d7b9
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigscidsessiontemplatessessiontemplatenamebatch.html
author: KevinAsgari
description: " /serviceconfigs/ {scid} /sessiontemplates/ {sessionTemplateName} / 批处理"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 6cc0850d1fda69eae1c0f3774a3146de33c7b4c8
ms.sourcegitcommit: 72710baeee8c898b5ab77ceb66d884eaa9db4cb8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/12/2018
ms.locfileid: "3881143"
---
# <a name="serviceconfigsscidsessiontemplatessessiontemplatenamebatch"></a>/serviceconfigs/ {scid} /sessiontemplates/ {sessionTemplateName} / 批处理
支持 POST 操作在会话模板级别创建批处理查询。

> [!IMPORTANT]
> 此方法使用 2015年多人游戏和应用仅向该多人游戏版本及更高版本。 它旨在用于使用模板合约 104/105 或更高版本，并且需要 X Xbl 协定版本的标头元素： 104/105 或更高版本上的每个请求。

<a id="ID4ER"></a>


## <a name="domain"></a>域
sessiondirectory.xboxlive.com  
<a id="ID4EW"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 类型| 说明|
| --- | --- | --- | --- |
| scid| GUID| 服务配置标识符 (SCID)。 会话标识符的第 1 部分中。|
| sessionTemplateName| 字符串| 会话模板的当前实例的名称。 第 2 部分会话标识符。|

<a id="ID4E2B"></a>


## <a name="valid-methods"></a>有效的方法

[POST (/serviceconfigs/ {scid} /sessiontemplates/ {sessionTemplateName} / 批次)](uri-serviceconfigscidsessiontemplatessessiontemplatenamebatchpost.md)

&nbsp;&nbsp;在多个 Xbox 用户 Id 创建批处理查询。

<a id="ID4EFC"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4EHC"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[会话目录 Uri](atoc-reference-sessiondirectory.md)
