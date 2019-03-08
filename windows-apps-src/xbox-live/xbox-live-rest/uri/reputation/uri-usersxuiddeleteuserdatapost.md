---
title: POST (/users/xuid({xuid})/deleteuserdata)
assetID: 8be13ff9-5d42-43a1-f2fa-d550d845a552
permalink: en-us/docs/xboxlive/rest/uri-usersxuiddeleteuserdatapost.html
description: " POST (/users/xuid({xuid})/deleteuserdata)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: dab43079dbba3729ff39f3a2116c377c3b73142a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57604062"
---
# <a name="post-usersxuidxuiddeleteuserdata"></a>POST (/users/xuid({xuid})/deleteuserdata)
完全重置的测试用户的信誉数据。 仅用于测试。

  * [备注](#ID4EQ)
  * [URI 参数](#ID4E5)
  * [Authorization](#ID4EJB)
  * [所需的请求标头](#ID4E3B)
  * [HTTP 状态代码](#ID4EHC)
  * [响应正文](#ID4EJF)

<a id="ID4EQ"></a>


## <a name="remarks"></a>备注

调用此 API 将所有反馈项和信誉数据从用户都删除。 合作伙伴可能会调用此 API 针对除零售任何沙盒。 强制执行团队可能会调用此 API 与任何沙盒 id。

这些 Uri 的域是`reputation.xboxlive.com`。 端口 10443 上始终调用此 URI。

<a id="ID4E5"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- |
| xuid| 64 位无符号的整数| Xbox 用户 ID (XUID) 正在删除其数据的用户。|

<a id="ID4EJB"></a>


## <a name="authorization"></a>授权

零售沙盒**PartnerClaim**强制执行团队。

为所有其他沙箱**PartnerClaim**并**SandboxIdClaim**。

<a id="ID4E3B"></a>


## <a name="required-request-headers"></a>所需的请求标头

**内容类型： application/json**并**X Xbl 协定版本**（当前版本为 101）。

<a id="ID4EHC"></a>


## <a name="http-status-codes"></a>HTTP 状态代码

服务将返回其中一个状态代码在本部分中使用此方法在此资源上发出的请求的响应中。 有关与 Xbox Live 服务一起使用的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。

| 代码| 原因短语| 描述|
| --- | --- | --- | --- | --- | --- |
| 200| 确定| 已成功检索该会话。|
| 400| 无效的请求| 服务无法理解请求格式不正确。 通常是一个无效的参数。|
| 401| 未经授权| 请求需要用户身份验证。|
| 404| 未找到| 找不到指定的资源。|
| 500| 内部服务器错误| 服务器遇到意外的情况，致使无法履行请求。|
| 503| 服务不可用| 请求已达到限制，以秒为单位 （例如 5 秒更高版本） 的客户端-重试值后重试请求。|

<a id="ID4EJF"></a>


## <a name="response-body"></a>响应正文

无; 如果成功否则为[服务错误 (JSON)](../../json/json-serviceerror.md)文档。

<a id="ID4EWF"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4EYF"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[/users/xuid({xuid})/deleteuserdata](uri-usersxuiddeleteuserdata.md)
