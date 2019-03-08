---
title: /serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}
assetID: 55ce6459-1714-49bc-6231-b547ddf04143
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionname.html
description: " /serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: da8d61634d0a849d51e2ab6ff143469ec05cc75c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57657472"
---
# <a name="serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionname"></a>/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}
支持 PUT 和 GET 操作来创建和检索会话。
<a id="ID4EO"></a>


## <a name="domain"></a>域
sessiondirectory.xboxlive.com  
<a id="ID4ET"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- |
| scid| GUID| 服务配置标识符 (SCID)。 会话标识符的第 1 部分中。|
| sessionTemplateName| 字符串| 会话模板的当前实例的名称。 会话标识符的第 2 部分中。|
| sessionName| GUID| 会话的唯一 ID。 会话标识符的第 3 部分。| 

<a id="ID4EBC"></a>


## <a name="valid-methods"></a>有效的方法

[GET (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName})](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnameget.md)

&nbsp;&nbsp;获取会话对象。

[PUT (/serviceconfigs/ {scid} /sessiontemplates/ {sessionTemplateName} /sessions/ {sessionName})](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnameput.md)

&nbsp;&nbsp;创建、 更新或加入会话时。

<a id="ID4EOC"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4EQC"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[会话目录 Uri](atoc-reference-sessiondirectory.md)
