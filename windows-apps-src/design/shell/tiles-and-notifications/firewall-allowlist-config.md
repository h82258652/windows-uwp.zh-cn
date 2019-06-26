---
author: mijacobs
Description: 许多企业使用防火墙来阻止不需要的流量。 本文档介绍了如何才能让 WNS 流量通过防火墙。
title: 将 WNS 流量添加到防火墙允许列表
ms.assetid: 2125B09F-DB90-4515-9AA6-516C7E9ACCCD
template: detail.hbs
ms.author: mijacobs
ms.date: 05/20/2019
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp，WNS，windows 通知服务、 通知、 windows、 防火墙故障排除、 IP、 流量、 企业版、 网络、 IPv4、 VIP、 FQDN，公共 IP 地址
ms.localizationpriority: medium
ms.openlocfilehash: 23a9b11cd961e03217aba8ca3d1d988447a2f80b
ms.sourcegitcommit: b0edd3c09f931b9b62f9c2d17037fb58d826174f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/25/2019
ms.locfileid: "67349856"
---
# <a name="allowing-windows-notification-traffic-through-enterprise-firewalls"></a>允许 Windows 通知流量通过企业防火墙

## <a name="background"></a>后台
许多企业使用防火墙来阻止不需要的网络流量;遗憾的是，这还可以阻止 Windows 通知服务通信等的重要事项。 这意味着所有通过 WNS 发送的通知将被删除。 若要避免此问题，网络管理员可以将已批准的 WNS 频道列表添加到要允许 WNS 流量通过防火墙，其例外列表。 以下是如何更多详细信息和要添加的内容。 

> [!Note] 
自 6/24/2019，Windows 客户端起**不这样做**支持代理，到 WNS 的连接必须直接连接。

## <a name="what-information-should-be-added-to-the-allowlist"></a>应将哪些信息添加到允许列表
下面的列表包含 Fqdn、 Vip 和和 IP 地址使用 Windows 通知服务的范围。 

> [!IMPORTANT]
> 我们强烈建议您允许列表通过 FQDN，因为它们不会更改。 如果允许通过 FQDN 的列表，您不需要也允许的 IP 地址范围。

> [!IMPORTANT]
> 将定期; 更改 IP 地址范围因此，它们不包含此页上。 如果想要查看的 IP 范围列表，您可以从下载中心下载该文件：[Windows 通知服务 (WNS) VIP 和 IP 范围](https://www.microsoft.com/download/details.aspx?id=44238)。 请检查后定期以确保具有最新信息。 


### <a name="fqdns-vips-and-ips"></a>Fqdn、 Vip 和和 IPs
在它后面的表中解释的以下 XML 文档中的元素的每个 (在[条款和表示法](#terms-and-notations)。 IP 范围已有意留出此文档，以鼓励你将仅 Fqdn 用作 Fqdn 将保持不变。 但是，您可以下载包含从下载中心的完整列表的 XML 文件：[Windows 通知服务 (WNS) VIP 和 IP 范围](https://www.microsoft.com/download/details.aspx?id=44238)。 新的 Vip 或 IP 范围将是**有效一周后会将它们上载**。

```XML
<?xml version="1.0" encoding="UTF-8"?>
<WNSPublicIpAddresses xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <!-- This file contains the FQDNs, VIPs, and IP address ranges used by the Windows Notification Service. A new text file will be uploaded every time a new VIP or IP range is released in production.  Please copy the below information and perform the necessary changes on your site. Endpoints in CloudService nodes are used for cloud services to send notifications to WNS. Endpoints in Client nodes are used by devices to receive notifications from WNS. --> 
    <CloudServiceDNS>
    <DNS FQDN="*.notify.windows.com"/>
    </CloudServiceDNS>
    <ClientDNS>
        <DNS FQDN="*.wns.windows.com"/>
        <DNS FQDN="*.notify.live.net"/>
    </ClientDNS>
    <CloudServiceIPs>
        <IpRange Subnet=""/>
        <!-- See the file in Download Center for the complete list of IP ranges -->
    </CloudServiceIPs>
    <ClientIPsIPv4>
        <IpRange Subnet=""/>
        <!-- See the file in Download Center for the complete list of IP ranges -->
    </ClientIPsIPv4>
</WNSPublicIpAddresses>

```

### <a name="terms-and-notations"></a>条款和表示法
下面是在表示法和更高版本的 XML 代码段中使用的元素的说明。

| 术语 | 说明 |
|---|---|
| **十进制表示法 (即 64.4.28.0/26)** | 十进制表示法是一种描述范围的 IP 地址的方法。 例如，64.4.28.0/26 意味着 64.4.28.0 前 26 位是常量，而最后一个 6 位均为变量。  在这种情况下，IPv4 范围是 64.4.28.0-64.4.28.63。 |
| **ClientDNS** | 这些是客户端设备 （Windows 电脑、 台式计算机） 的完全限定域名 (FQDN) 筛选器从 WNS 收到通知。 这些必须允许通过使 WNS 客户端将使用 WNS 功能中的防火墙。  建议到允许列表通过 Fqdn 而非 IP/VIP 范围，因为它们将永远不会更改。 |
| **ClientIPsIPv4** | 这些是由客户端设备 （Windows 电脑、 台式计算机） 访问的服务器的 IPv4 地址从 WNS 收到通知。 |
| **CloudServiceDNS** | 这些是你的云服务将与进行通信以将通知发送到 WNS 的 WNS 服务器的完全限定域名 (FQDN) 筛选器。 这些必须允许通过防火墙服务以发送 WNS 通知的顺序。  建议到允许列表通过 Fqdn 而非 IP/VIP 范围，因为它们将永远不会更改。|
| **CloudServiceIPs** | CloudServiceIPs 是用于将通知发送到 WNS 的云服务的服务器的 IPv4 地址  |


## <a name="microsoft-push-notifications-service-mpns-public-ip-ranges"></a>Microsoft 推送通知服务 (MPNS) 公共 IP 范围
如果使用旧的通知服务，MPNS，将需要添加到允许列表的 IP 地址范围是从下载中心中：[Microsoft 推送通知服务 (MPNS) 公共 IP 范围](https://www.microsoft.com/download/details.aspx?id=44535)。


## <a name="related-topics"></a>相关主题

* [快速入门：发送推送通知](https://docs.microsoft.com/previous-versions/windows/apps/hh868252(v=win.10))
* [如何请求、 创建和保存通知通道](https://docs.microsoft.com/previous-versions/windows/apps/hh465412(v=win.10))
* [如何截获用于运行应用程序的通知](https://docs.microsoft.com/previous-versions/windows/apps/jj709907(v=win.10))
* [如何进行身份验证使用 Windows 推送通知服务 (WNS)](https://docs.microsoft.com/previous-versions/windows/apps/hh465407(v=win.10))
* [推送通知服务请求和响应标头](https://docs.microsoft.com/previous-versions/windows/apps/hh465435(v=win.10))
* [指导原则和清单的推送通知](https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview)
 
