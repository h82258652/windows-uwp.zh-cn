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
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2018
ms.locfileid: "8943854"
---
# <a name="get-serviceconfigsscidhoppersnamestats"></a>GET (/serviceconfigs/{scid}/hoppers/{name}/stats)

漏斗获取统计信息。

> [!IMPORTANT]
> 此方法旨在用于与合同 103 或更高版本，并且需要 X Xbl 协定版本的标头元素： 103 或更高版本上的每个请求。

  * [备注](#ID4ET)
  * [URI 参数](#ID4E5)
  * [授权](#ID4EJB)
  * [HTTP 状态代码](#ID4E3C)
  * [请求正文](#ID4EFD)
  * [响应正文](#ID4EQD)

<a id="ID4ET"></a>


## <a name="remarks"></a>备注
此 HTTP/REST 方法获取从命名漏斗在服务配置 ID (SCID) 级别的统计信息。 此方法可以由**Microsoft.Xbox.Services.Matchmaking.MatchmakingService.GetHopperStatisticsAsync** API 包装。  
<a id="ID4E5"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 类型| 描述|
| --- | --- | --- | --- |
| scid| GUID| 服务配置标识符 (SCID) 的会话。|
| name| 字符串| 漏斗的名称。|

<a id="ID4EJB"></a>


## <a name="authorization"></a>授权

| 类型| 必需| 描述| 如果缺少的响应|
| --- | --- | --- | --- | --- | --- | --- | --- |
| XUID (用户 ID)| 是| 发出请求的用户必须是指代票证的票证会话成员。 | 403|
| 权限和设备类型| 是| 当用户的 deviceType 设置为主机时，仅具有多人游戏中其声明特权的用户允许对匹配服务进行调用。 | 403|
| 主题作品 ID/概念证明购买/设备类型| 是| 正在匹配到游戏必须允许指定的主题作品声明，设备类型组合的匹配。 | 403|

<a id="ID4E3C"></a>


## <a name="http-status-codes"></a>HTTP 状态代码
该服务返回 HTTP 状态代码，因为它适用于 MPSD。  
<a id="ID4EFD"></a>


## <a name="request-body"></a>请求正文

此请求的正文中不发送任何对象。

<a id="ID4EQD"></a>


## <a name="response-body"></a>响应正文

| 成员| 类型| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| hopperName| 字符串| 所选的漏斗的名称。|
| waitTime| 32 位有符号整数| 匹配漏斗 （不可或缺秒数） 时间的平均。 |
| 填充| 32 位有符号整数| 等待匹配漏斗中的用户数。|

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
