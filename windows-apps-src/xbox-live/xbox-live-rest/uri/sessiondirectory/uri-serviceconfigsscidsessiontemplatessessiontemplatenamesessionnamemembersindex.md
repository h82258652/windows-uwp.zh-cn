---
title: /serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/{index}
assetID: ae6c6a25-2251-6ffd-ec58-e6c0576a34db
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionnamemembersindex.html
description: " /serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/{index}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 8698eabc57577746d32f9ee439d6cd7af4820b5e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57617472"
---
# <a name="serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnamemembersindex"></a>/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/{index}
支持删除操作删除指定的会话成员。
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

<a id="ID4EDC"></a>


## <a name="valid-methods"></a>有效的方法

[DELETE (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/{index})](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionnamemembersindexdelete.md)

&nbsp;&nbsp;从会话中删除指定的成员。

<a id="ID4ENC"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4EPC"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[会话目录 Uri](atoc-reference-sessiondirectory.md)
