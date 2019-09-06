---
author: mijacobs
Description: 许多企业使用防火墙来阻止不需要的流量。 此文档介绍了如何允许 WNS 流量通过防火墙。
title: 将 WNS 流量添加到防火墙允许列表
ms.assetid: 2125B09F-DB90-4515-9AA6-516C7E9ACCCD
template: detail.hbs
ms.author: mijacobs
ms.date: 05/20/2019
ms.topic: article
keywords: windows 10，uwp，WNS，windows 通知服务，通知，windows，防火墙，疑难解答，IP，流量，企业，网络，IPv4，VIP，FQDN，公共 IP 地址
ms.localizationpriority: medium
ms.openlocfilehash: 0ba6d2e678eee0d851b4f2e3897f9fc067b74580
ms.sourcegitcommit: 3360db6bc975516e01913d3d73599c964a411052
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/04/2019
ms.locfileid: "70296985"
---
# <a name="enterprise-firewall-and-proxy-configurations-to-support-wns-traffic"></a>支持 WNS 流量的企业防火墙和代理配置

## <a name="background"></a>后台
许多企业使用防火墙来阻止不需要的网络流量;遗憾的是，这也会阻止 Windows 通知服务通信等重要事项。 这意味着，通过 WNS 发送的所有通知将在某些网络配置下被丢弃。 为避免出现这种情况，网络管理员可以将已批准的 WNS 通道列表添加到其免除列表，以允许 WNS 流量通过防火墙。 下面更详细地介绍了如何以及如何添加哪些内容以及对不同代理类型的支持。

## <a name="proxy-support"></a>代理支持

> [!Note]
> Windows 客户端**不**支持所有代理，与 WNS 的连接必须是直接连接。

**即将推出！** 我们正在积极调查不同的网络配置、代理和防火墙。 我们将更新此页，其中包含有关常见企业方案和 WNS 支持的更多详细信息。


## <a name="what-information-should-be-added-to-the-allowlist"></a>应将哪些信息添加到允许列表
下面是一个列表，其中包含 Windows 通知服务使用的 Fqdn、Vip 和 IP 地址范围。 

> [!IMPORTANT]
> 我们强烈建议你允许按 FQDN 列出，因为这些将不会更改。 如果允许按 FQDN 列出，则不需要同时允许 IP 地址范围。

> [!IMPORTANT]
> IP 地址范围将定期更改;因此，它们不包括在此页中。 若要查看 IP 范围列表，可以从下载中心下载该文件：[Windows 通知服务（WNS） VIP 和 IP 范围](https://www.microsoft.com/download/details.aspx?id=44238)。 请定期检查以确保具有最新信息。 


### <a name="fqdns-vips-and-ips"></a>Fqdn、Vip 和 Ip
以下 XML 文档中的每个元素都在其后的表中进行了说明（[术语和表示法](#terms-and-notations)）。 此文档中特意遗留了 IP 范围，以鼓励你只使用 Fqdn，因为 Fqdn 将保持不变。 但是，你可以从下载中心下载包含完整列表的 XML 文件：[Windows 通知服务（WNS） VIP 和 IP 范围](https://www.microsoft.com/download/details.aspx?id=44238)。 新的 Vip 或 IP 范围将在**其上传一周后生效**。

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

### <a name="terms-and-notations"></a>术语和表示法
下面是上述 XML 代码片段中使用的表示法和元素的说明。

| 术语 | 说明 |
|---|---|
| **点-十进制表示法（即 64.4.28.0/26）** | 点-十进制表示法是一种描述 IP 地址范围的方法。 例如，64.4.28.0/26 表示64.4.28.0 的前26位是常量，而最后6位是可变的。  在这种情况下，IPv4 范围为 64.4.28.0-64.4.28.63。 |
| **ClientDNS** | 这些是从 WNS 接收通知的客户端设备（Windows 电脑、桌面）的完全限定的域名（FQDN）筛选器。 若要使 WNS 客户端使用 WNS 功能，必须通过防火墙允许使用这些功能。  建议按 Fqdn 而不是 IP/VIP 范围允许-列表，因为这些将永远不会更改。 |
| **ClientIPsIPv4** | 这些是从 WNS 接收通知的客户端设备（Windows 电脑、桌面）访问的服务器的 IPv4 地址。 |
| **CloudServiceDNS** | 这些是云服务将与发送 notificatios 到 WNS 的 WNS 服务器的完全限定的域名（FQDN）筛选器。 若要让服务发送 WNS 通知，必须通过防火墙允许使用这些命令。  建议按 Fqdn 而不是 IP/VIP 范围允许-列表，因为这些将永远不会更改。|
| **CloudServiceIPs** | CloudServiceIPs 是用于云服务的服务器 IPv4 地址，用于向 WNS 发送通知  |


## <a name="microsoft-push-notifications-service-mpns-public-ip-ranges"></a>Microsoft 推送通知服务（MPNS）公共 IP 范围
如果你使用的是旧通知服务，则可以从下载中心获取需要添加到允许列表的 IP 地址范围：[Microsoft 推送通知服务（MPNS）公共 IP 范围](https://www.microsoft.com/download/details.aspx?id=44535)。


## <a name="related-topics"></a>相关主题

* [快速入门：发送推送通知](https://docs.microsoft.com/previous-versions/windows/apps/hh868252(v=win.10))
* [如何请求、创建和保存通知通道](https://docs.microsoft.com/previous-versions/windows/apps/hh465412(v=win.10))
* [如何拦截正在运行的应用程序的通知](https://docs.microsoft.com/previous-versions/windows/apps/jj709907(v=win.10))
* [如何向 Windows 推送通知服务（WNS）进行身份验证](https://docs.microsoft.com/previous-versions/windows/apps/hh465407(v=win.10))
* [推送通知服务请求和响应标头](https://docs.microsoft.com/previous-versions/windows/apps/hh465435(v=win.10))
* [推送通知的指导原则和核对清单](https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview)
 
