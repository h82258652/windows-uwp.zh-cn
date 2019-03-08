---
title: /serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions
assetID: 8d55818f-99fd-146a-896b-0f100e78799f
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessions.html
description: " /serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 99d9a82cb419e6598fc1113692b031487950aa8b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57656092"
---
# <a name="serviceconfigsscidsessiontemplatessessiontemplatenamesessions"></a>/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions
支持 GET 操作来检索一组会话模板使用指定的模板名称。 
<a id="ID4EO"></a>

 
## <a name="domain"></a>域
sessiondirectory.xboxlive.com  
<a id="ID4ET"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| scid| GUID| 服务配置标识符 (SCID)。 第 1 部分会话的 id。| 
| 关键字| 字符串| 用于结果筛选为只使用该字符串标识的会话的关键字。| 
| xuid| GUID| 为其检索会话的用户的 Xbox 用户 Id。 用户必须是会话中处于活动状态。 | 
| 保留项| 字符串| 值，该值指示如果会话的列表包括用户未接受。 仅可以将此参数设置为 true。 此设置要求调用方拥有对该会话，服务器级访问权限，或调用方的 XUID 声明以与 Xbox 用户 ID 筛选器匹配。 | 
| 非活动状态| 字符串| 值，该值指示会话的列表包括那些用户已接受但不是主动播放。 仅可以将此参数设置为 true。 | 
| 专用| 字符串| 值，该值指示是否会话的列表包括专用会话。 仅可以将此参数设置为 true。 仅当查询您自己的会话，或查询服务器到服务器时，它是有效的。 此参数设置为 true 要求调用方拥有对该会话，服务器级访问权限，或调用方的 XUID 声明以与 Xbox 用户 ID 筛选器匹配。 | 
| visibility| 字符串| 一个枚举值，该值指示在筛选结果中使用的可见性状态。 当前此参数可以仅将设置为打开包含打开的会话。 请参阅<b>MultiplayerSessionVisibility</b>。 | 
| version| 字符串| 一个正整数，该值指示会话主要版本或较低的会话包括。 值必须小于或等于请求的协定版本取模 100。 | 
| Take| 字符串| 一个正整数，该值指示会话的最大数目来检索。| 
  
<a id="ID4EZD"></a>

 
## <a name="valid-methods"></a>有效的方法

[GET (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions)](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionsget.md)

&nbsp;&nbsp;检索会话模板文档。
 
<a id="ID4EDE"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EFE"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[会话目录 Uri](atoc-reference-sessiondirectory.md)

   