---
title: EDS 通用标头
assetID: 91297c9e-709d-7886-1da0-8a896c4953f4
permalink: en-us/docs/xboxlive/rest/edscommonheaders.html
description: " EDS 通用标头"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 51a85383ee75fa51cb8376451ac955dcc4fa2edf
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57630702"
---
# <a name="eds-common-headers"></a>EDS 通用标头

<a id="ID4EO"></a>



## <a name="entertainment-discovery-services-eds-common-request-headers"></a>娱乐发现服务 (EDS) 常见请求标头

| 头名称| 描述| 是否为必需？| 注释|
| --- | --- | --- | --- |
| <b>x-xbl-contract-version</b>| EDS 服务版本| 是| 3.2|
| <b>x-xbl-client-type</b>| 客户端类型标头| 是| 团队以获取您自己的客户端类型进行朗读。|
| <b>x-xbl-client-version</b>| 客户端版本| 是| 任何非空字符串。|
| <b>x-xbl-parent-ig</b>| 印象 guid| 是| 用于跟踪日志中并在其他服务调用之间的请求。|
| <b>x-xbl-device-type</b>| 设备类型| 是| 表示客户端的设备。|
| <b>接受</b>| 接受的类型| 是| XML 或 JSON。|
| <b>Authorization</b>| 身份验证标头| 是|  |
| <b>x-xbl-autoSuggest-seed-text</b>| 自动建议的种子| 否| 使用 BI 和相关性|
| <b>x-xbl-search-term</b>| 搜索词| 否|  |
| <b>x-xbl-input-method</b>| 用户使用的输入的法| 否| 控制器、 语音、 Kinect。|
| <b>x-xbl-kinect-enabled</b>| 已启用的 Kinect| 否| 是/否。|
| <b>x-xbl-speech-session-id</b>| 语音会话 ID| 否| 是否使用语音启动会话。|
| <b>x-xbl-client-id</b>| 匿名客户端 Id| 否| 用于 BI 报告和相关性。|
| <b>x-xbl-device-id</b>| 设备 ID| 否| 用于 BI 报告和相关性。|
| <b>x-xbl-user-agent</b>| 客户端用户代理| 否| 使用 bi。 "&lt;名称 > /&lt;版本 > (&lt;OS 版本 >;&lt;平台 >;&lt;功能 >;&lt;制造 >;&lt;模型 >)"。|
| <b>x-xbl-parent-ig</b>| 对于"Chained"调用的上一印象 Guid| 否 （但强烈建议）| 重要 BI 相关性。 例如，浏览调用的 IG 是以下父 IG 详细信息调用。|
| <b>delid</b>| 将标识委托| 否| 内部服务使用它来代表用户工作。|

## <a name="common-response-headers"></a>常见响应标头

| 头名称| 描述| 是否为必需？| 注释|
| --- | --- | --- | --- | --- | --- | --- | --- |
| <b>Cache</b>| 缓存标头| 是| 请参阅<a href="https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9"> http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9 </a>。|
| <b>x-xbl-errors</b>| 错误| 否| 从各种数据提供程序错误的列表。|
| <b>x-xbl-traceid</b>| 日志中的跟踪 Id| 是| 用于跟踪请求特定日志。|
| <b>x-server-name</b>| 处理请求的服务器的经过模糊处理的名称。| 是| 用于内部调试。|

<a id="ID4EECAC"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4EGCAC"></a>


##### <a name="parent"></a>Parent 的子磁盘）  

[其他参考](atoc-xboxlivews-reference-additional.md)


<a id="ID4ESCAC"></a>


##### <a name="further-information"></a>详细信息

[Marketplace Uri](../uri/marketplace/atoc-reference-marketplace.md)
