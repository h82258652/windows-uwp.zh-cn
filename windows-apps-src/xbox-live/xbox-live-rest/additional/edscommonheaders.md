---
title: EDS 通用标头
assetID: 91297c9e-709d-7886-1da0-8a896c4953f4
permalink: en-us/docs/xboxlive/rest/edscommonheaders.html
author: KevinAsgari
description: " EDS 通用标头"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 2452be9eacd5efe0b28229a14579e838e9b62d0e
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/23/2018
ms.locfileid: "5407889"
---
# <a name="eds-common-headers"></a>EDS 通用标头

<a id="ID4EO"></a>



## <a name="entertainment-discovery-services-eds-common-request-headers"></a>娱乐发现服务 (EDS) 常见请求标头

| 标头名称| 说明| 是否为必需？| 注释|
| --- | --- | --- | --- |
| <b>x xbl 协定版本</b>| EDS 服务版本| 是| 3.2|
| <b>x xbl 客户端类型</b>| 客户端类型标头| 是| 与团队，获取你自己的客户端类型进行交流。|
| <b>x xbl 客户端版本</b>| 客户端版本| 是| 任何非空字符串。|
| <b>x xbl 父 ig</b>| 印象 guid| 是| 用于跟踪日志中以及跨其他服务调用的请求。|
| <b>x xbl 设备类型</b>| 设备类型| 是| 表示客户端的设备。|
| <b>接受</b>| 接受类型| 是| XML 或 JSON。|
| <b>授权</b>| 身份验证标头| 是|  |
| <b>x xbl 自动建议-种子-文本转换功能</b>| 自动建议种子文本| 否| 使用为 BI 和相关性|
| <b>x xbl 搜索词</b>| 搜索词| 否|  |
| <b>x xbl 输入方法</b>| 用户使用的输入的法| 否| 控制器，语音、 Kinect。|
| <b>x-xbl-kinect 启用</b>| 启用 Kinect| 否| 是/否。|
| <b>x-xbl-语音的会话的 id</b>| 语音会话 ID| 否| 是否已使用语音启动会话。|
| <b>x xbl 客户端 id</b>| 匿名客户端 Id| 否| 用于 BI 报告和相关性。|
| <b>x xbl 设备 id</b>| 设备 ID| 否| 用于 BI 报告和相关性。|
| <b>x xbl 用户代理</b>| 客户端用户代理| 否| 用于 BI。 "&lt;名称 > /&lt;版本 > (&lt;操作系统版本 >;&lt;平台 >;&lt;功能 >;&lt;制造 >;&lt;模型 >)"。|
| <b>x xbl 父 ig</b>| 对于"Chained"调用之前印象 Guid| 否 （但强烈建议）| BI 相关性来说很重要。 例如，浏览调用 IG 是以下父 IG 细节调用。|
| <b>delid</b>| 委托标识| 否| 使用内部服务来代表用户工作。|

## <a name="common-response-headers"></a>常见的响应标头

| 标头名称| 说明| 是否为必需？| 注释|
| --- | --- | --- | --- | --- | --- | --- | --- |
| <b>缓存</b>| 缓存标头| 是| 请参阅<a href="http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9">http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9</a>。|
| <b>xbl 的 x 错误</b>| 错误| 否| 从各种数据提供程序的错误的列表。|
| <b>x-xbl traceid</b>| 从日志的跟踪 Id| 是| 用于跟踪回请求特定日志。|
| <b>x 服务器名称</b>| 处理请求的服务器名称混淆。| 是| 用于内部调试。|

<a id="ID4EECAC"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4EGCAC"></a>


##### <a name="parent"></a>Parent 的子磁盘）  

[其他参考](atoc-xboxlivews-reference-additional.md)


<a id="ID4ESCAC"></a>


##### <a name="further-information"></a>详细信息

[市场 URI](../uri/marketplace/atoc-reference-marketplace.md)
