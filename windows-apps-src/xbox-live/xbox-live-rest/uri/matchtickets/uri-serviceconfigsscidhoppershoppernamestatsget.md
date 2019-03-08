---
title: GET (/serviceconfigs/{scid}/hoppers/{name}/stats)
assetID: 4de5b07d-93e1-8ff0-05dd-1d3bb1802088
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidhoppershoppernamestatsget.html
description: " GET (/serviceconfigs/{scid}/hoppers/{name}/stats)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 95de95b35de496331dd3fe0a4c69f18e047c1020
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57621692"
---
# <a name="get-serviceconfigsscidhoppersnamestats"></a>GET (/serviceconfigs/{scid}/hoppers/{name}/stats)

Hopper 获取统计信息。

> [!IMPORTANT]
> 此方法旨在用于具有协定 103 或更高版本，并要求 X Xbl 约定版本标头元素：103 或更高版本上的每个请求。

  * [备注](#ID4ET)
  * [URI 参数](#ID4E5)
  * [Authorization](#ID4EJB)
  * [HTTP 状态代码](#ID4E3C)
  * [请求正文](#ID4EFD)
  * [响应正文](#ID4EQD)

<a id="ID4ET"></a>


## <a name="remarks"></a>备注
此 HTTP/REST 方法从在服务配置 ID (SCID) 级别命名 hopper 获取统计信息。 此方法的两端可加**Microsoft.Xbox.Services.Matchmaking.MatchmakingService.GetHopperStatisticsAsync** API。  
<a id="ID4E5"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- | --- |
| scid| GUID| 服务配置 (SCID) 会话标识符。|
| name| 字符串| Hopper 的名称。|

<a id="ID4EJB"></a>


## <a name="authorization"></a>授权

| 在任务栏的搜索框中键入| 必需| 描述| 如果缺少的响应|
| --- | --- | --- | --- | --- | --- | --- | --- |
| XUID (用户 ID)| 是| 发出请求的用户必须是引用该票证的票证会话的成员。 | 403|
| 权限和设备类型| 是| 当用户的设备类型设置为控制台时，只有具有在其声明中的多玩家特权的用户允许对匹配服务进行调用。 | 403|
| 标题 ID/采购/设备类型的概念验证| 是| 要匹配到标题必须允许指定的标题声明、 设备类型组合匹配。 | 403|

<a id="ID4E3C"></a>


## <a name="http-status-codes"></a>HTTP 状态代码
同样适用于 MPSD，服务将返回 HTTP 状态代码。  
<a id="ID4EFD"></a>


## <a name="request-body"></a>请求正文

此请求的正文中不发送的任何对象。

<a id="ID4EQD"></a>


## <a name="response-body"></a>响应正文

| 成员| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| hopperName| 字符串| 所选 hopper 的名称。|
| waitTime| 32 位有符号的整数| 平均匹配 hopper （整数秒数） 的时间。 |
| 填充| 32 位有符号的整数| 等待 hopper 中的匹配项的人员数。|

<a id="ID4E1D"></a>


### <a name="sample-response"></a>示例响应


```cpp
{
      "hopperName":"contosoawesome2",
      "waitTime":30,
      "population":1
    }


```


<a id="ID4EJE"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4ELE"></a>


##### <a name="parent"></a>Parent 的子磁盘）  

[/serviceconfigs/{scid}/hoppers/{name}/stats](uri-serviceconfigsscidhoppershoppernamestats.md)
