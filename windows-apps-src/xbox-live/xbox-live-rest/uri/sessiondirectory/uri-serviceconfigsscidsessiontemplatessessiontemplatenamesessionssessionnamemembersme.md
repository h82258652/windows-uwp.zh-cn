---
title: /serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/me
assetID: a6c2fa17-8bed-d0df-d7ff-db1aa60f44b3
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnamemembersme.html
author: KevinAsgari
description: " /serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/me"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 0d7e61fe76fc0f322c93d55448d53ee6444ffec5
ms.sourcegitcommit: 63cef0a7805f1594984da4d4ff2f76894f12d942
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/05/2018
ms.locfileid: "4390413"
---
# <a name="serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnamemembersme"></a>/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/me
支持删除操作，以删除会话成员。
<a id="ID4EO"></a>


## <a name="domain"></a>域
sessiondirectory.xboxlive.com  
<a id="ID4ET"></a>

 
## <a name="remarks"></a>备注

所有会话成员资源操作都需要 Xbox 用户 ID (XUID) 用户声明授权。

<a id="ID4EAB"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 类型| 描述|
| --- | --- | --- |
| scid| GUID| 服务配置标识符 (SCID)。 第 1 部分会话标识符。|
| sessionTemplateName| 字符串| 会话模板的当前实例的名称。 第 2 部分会话标识符。|
| 会话名| GUID| 会话的唯一 ID。 会话标识符的第 3 部分。|

<a id="ID4EOC"></a>


## <a name="valid-methods"></a>有效的方法

[DELETE (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/me)](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnamemembersmedelete.md)

&nbsp;&nbsp;从会话中删除成员。

<a id="ID4EYC"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4E1C"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[会话目录 URI](atoc-reference-sessiondirectory.md)
