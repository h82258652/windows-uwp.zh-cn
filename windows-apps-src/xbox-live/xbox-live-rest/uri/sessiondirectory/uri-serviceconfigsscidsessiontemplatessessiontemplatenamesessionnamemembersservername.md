---
title: /serviceconfigs/ {scid} /sessiontemplates/ {sessionTemplateName} {会话名} /sessions/ /servers/ {server name}
assetID: aed0764f-4e3d-e0b3-1ea0-543c32f3f822
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionnamemembersservername.html
author: KevinAsgari
description: " /serviceconfigs/ {scid} /sessiontemplates/ {sessionTemplateName} {会话名} /sessions/ /servers/ {server name}"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b451d5aca9e855e204cb1178bf8ae30fb33ed015
ms.sourcegitcommit: 72710baeee8c898b5ab77ceb66d884eaa9db4cb8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/12/2018
ms.locfileid: "3880668"
---
# <a name="serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnameserversserver-name"></a>/serviceconfigs/ {scid} /sessiontemplates/ {sessionTemplateName} {会话名} /sessions/ /servers/ {server name}
支持删除操作，以删除指定的会话的服务器。
<a id="ID4EO"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 类型| 说明|
| --- | --- | --- |
| scid| GUID| 服务配置标识符 (SCID)。 会话标识符的第 1 部分中。|
| sessionTemplateName| 字符串| 会话模板的当前实例的名称。 第 2 部分会话标识符。|
| 会话名| GUID| 会话的唯一 ID。 会话标识符的第 3 部分。| 

<a id="ID4E3B"></a>


## <a name="valid-methods"></a>有效的方法

[删除 (/serviceconfigs/ {scid} /sessiontemplates/ {sessionTemplateName} {会话名} /sessions/ /servers/ {server name})](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionnamemembersservernamedelete.md)

&nbsp;&nbsp;从会话中删除指定的服务器。

<a id="ID4EGC"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4EIC"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[会话目录 Uri](atoc-reference-sessiondirectory.md)
