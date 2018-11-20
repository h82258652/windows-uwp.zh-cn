---
author: stevewhims
ms.assetid: 2CC2E526-DACB-4008-9539-DA3D0C190290
description: 适用于 UWP 开发人员的网络技术概述，以及关于如何选择适合自己应用的技术的建议。
title: 选择哪一种网络技术？
ms.author: stwhi
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3c617f3ec6d189f3ee75fcf2fd0ff96d386ec32c
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/19/2018
ms.locfileid: "7299809"
---
# <a name="which-networking-technology"></a>选择哪一种网络技术？


适用于 UWP 开发人员的网络技术概述，以及关于如何选择适合自己应用的技术的建议。

## <a name="sockets"></a>套接字

如果你正在与另一台设备通信而想要使用自己的协议，可使用[套接字](sockets.md)。

有以下两种套接字实现可供通用 Windows 平台 (UWP) 开发人员使用：[**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960) 和 [Winsock](https://msdn.microsoft.com/library/windows/desktop/ms740673)。 如果你需要编写新的代码，则 Windows.Networking.Sockets 的优势在于 API 不仅先进且专为 UWP 开发人员“量身打造”。 若要使用跨平台网络库或其他现有 Winsock 代码，或者想要使用 Winsock API，则可以使用该套接字。

### <a name="when-to-use-sockets"></a>何时使用套接字

-   借助这两种套接字实现，你可以使用自己选择的协议（TCP 或 UDP）与其他设备通信。

-   根据体验和使用的任何现有代码，选择最能满足你需求的套接字 API。

### <a name="when-not-to-use-sockets"></a>何时不使用套接字

-   不要使用套接字实现你自己的 HTTP 堆栈。 请改用 [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639)。
-   如果 WebSocket（[**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) 和 [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) 类）符合你的通信需要（指向/来自 Web 服务器的 TCP），请考虑使用它们，而不是花费自己的时间和开发资源通过套接字实现类似的功能。

## <a name="websockets"></a>WebSocket

[WebSocket](websockets.md) 协议定义了客户端与服务器之间通过 Web 进行快速而又安全的双向通信的机制。 数据通过全双工套接字连接立即传输，从而允许从两个终结点实时发送和接收消息。 WebSocket 非常适合在实时游戏中使用，由于即时社交网络通知和显示的最新信息（例如游戏统计信息）都需要是安全的，且需使用快速的数据传输。 UWP 开发人员可以使用 [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) 和 [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) 类与支持 Websocket 协议的服务器建立连接。

### <a name="when-to-use-websockets"></a>何时使用 Websocket

-   当你想要在设备和服务器之间不断地发送和接收数据时。

### <a name="when-not-to-use-websockets"></a>何时不使用 Websocket

-   如果你不经常发送或接收数据，你可能会发现将各个 HTTP 请求从设备发送到服务器更简单些，而不是建立并保持 WebSocket 连接。
-   WebSocket 可能不适用于容量非常大的情形。 请考虑先为你的数据流建模并通过 WebSocket 模拟流量，之后再将它们用于你的设计中。

## <a name="httpclient"></a>HttpClient

当你使用 HTTP 与 Web 服务或 Web 服务器通信时，请使用 [HttpClient](httpclient.md)（和其余的 [**Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692) 命名空间 API）。

### <a name="when-to-use-httpclient"></a>何时使用 HttpClient

-   当使用 HTTP 与 Web 服务通信时。
-   当上载或下载少量小文件时。
-   如果 WebSocket（[**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) 和 [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) 类）符合你的通信需要（指向/来自 Web 服务器的 TCP）且相关 Web 服务器支持 WebSocket，请考虑使用它们，而不是花费自己的时间和开发资源通过 HttpClient 实现类似的功能。
-   当你正在通过网络流式传输内容时。

### <a name="when-not-to-use-httpclient"></a>不使用 HttpClient 的情况

-   如果你要传输大型文件或大量的文件，请考虑改为使用后台传输。
-   如果你希望能够根据连接类型来限制上载/下载限制，或者希望保存进度并在中断后恢复上载/下载，则必须使用后台传输。
-   如果你要在两台设备之间通信且这两者都不是设计用于充当 HTTP 服务器，则应使用套接字。 请不要尝试实现你自己的 HTTP 服务器和使用 [HttpClient](httpclient.md) 与之通信。

## <a name="background-transfers"></a>后台传输

当你希望通过网络可靠地传输文件时，请使用[后台传输 API](background-transfers.md)。 后台传输 API 提供了高级的上载和下载功能，这些功能可在应用暂停期间在后台运行，并且在应用终止后仍保持运行。 该 API 监视网络状态以及在失去连接时自动暂停和恢复传输，而且传输同样也既为数据敏感型又为电池敏感型，这意味着下载活动将根据当前连接性和设备电池状态进行调整。 当你的应用在移动设备或电池供电的设备上运行时，这些功能是必不可少的。 该 API 非常适合于使用 HTTP 上载和下载大型文件。 还支持 FTP，但只能用于下载。

Windows 10 中的新后台传输功能是能够文件传输完成，以便你可以更新本地目录，激活其他应用，或在下载完成时通知用户时触发后期处理。

### <a name="when-to-use-background-transfers"></a>何时使用后台传输

-   若要可靠地传输大型文件或大量的文件，可使用后台传输。
-   如果你希望将以后台任务的形式传输处理后文件，可将后台传输与后台传输完成组搭配使用。
-   如果你想要在网络中断后恢复进行中的传输，请使用后台传输。
-   如果你希望能够根据网络条件（例如使用数据流量套餐时）来更改传输行为，请使用后台传输。

### <a name="when-not-to-use-background-transfers"></a>不使用后台传输的情况

-   如果你要传输少量的小文件，并且无需在传输完成后执行任何后续处理，应考虑使用 [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) PUT 或 POST 方法。
-   如果你希望流式传输数据并在其到达时在本地使用，请使用 [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639)。

## <a name="additional-network-related-technologies"></a>其他与网络相关的技术

### <a name="connection-quality"></a>连接质量

[**Windows.Networking.Connectivity**](https://msdn.microsoft.com/library/windows/apps/br207308) API 使你能够访问网络连接、成本以及使用情况信息。 有关使用此 API 的详细信息，请参阅[访问网络连接状态和管理网络成本](https://msdn.microsoft.com/library/windows/apps/hh452983)

### <a name="dns-service-discovery"></a>DNS 服务发现

[**Windows.Networking.ServiceDiscovery.Dnssd**](https://msdn.microsoft.com/library/windows/apps/dn895183) API 使你能够使用 DNS-SD 协议（如 IETF [RFC 2782](http://go.microsoft.com/fwlink/?LinkId=524158) 中所述）在网络上向其他设备宣传某种网络服务。

### <a name="communicating-over-bluetooth"></a>通过蓝牙进行通信

与其他 API 相比，[**Windows.Devices.Bluetooth**](https://msdn.microsoft.com/library/windows/apps/dn263413) API 允许你使用蓝牙连接到其他设备并传输数据。 有关详细信息，请参阅[使用 RFCOMM 发送或接收文件](https://msdn.microsoft.com/library/windows/apps/mt270289)。

### <a name="push-notifications-wns"></a>推送通知 (WNS)

[**Windows.Networking.PushNotifications**](https://msdn.microsoft.com/library/windows/apps/br241307) API 使你能够使用 Windows 通知服务 (WNS) 通过网络接收推送通知。 有关使用此 API 的详细信息，请参阅 [Windows 推送通知服务 (WNS) 概述](https://msdn.microsoft.com/library/windows/apps/mt187203)

### <a name="near-field-communications"></a>近场通信

[**Windows.Networking.Proximity**](https://msdn.microsoft.com/library/windows/apps/br241250) API 允许你针对使用邻近感应或点击设备的应用使用近场通信，从而可轻松传输数据。 有关使用此 API 的详细信息，请参阅[支持邻近感应和点击](https://msdn.microsoft.com/library/windows/apps/hh465229)。

### <a name="rssatom-feeds"></a>RSS/Atom 订阅源

[**Windows.Web.Syndication**](https://msdn.microsoft.com/library/windows/apps/br243632) API 使你能够以 RSS 和 Atom 格式管理联合源。 有关使用此 API 的详细信息，请参阅 [RSS/Atom 订阅源](web-feeds.md)。

### <a name="wi-fi-enumeration-and-connection-control"></a>WLAN 枚举和连接控制

[**Windows.Devices.WiFi**](https://msdn.microsoft.com/library/windows/apps/dn975224) API 使你能够枚举 Wi-Fi 适配器、扫描可用的 Wi-Fi 网络，以及将适配器连接到网络。

### <a name="radio-control"></a>无线控制

[**Windows.Devices.Radios**](https://msdn.microsoft.com/library/windows/apps/dn996447) API 使你能够查找和控制本地设备上的无线，其中包括 Wi-Fi 和蓝牙。

### <a name="wi-fi-direct"></a>Wi-Fi Direct

[**Windows.Devices.WiFiDirect**](https://msdn.microsoft.com/library/windows/apps/dn297687) API 使你能够通过使用 Wi-Fi Direct 创建临时的本地无线网络，来与其他本地设备进行连接和通信。

### <a name="wi-fi-direct-services"></a>Wi-Fi Direct Services

[**Windows.Devices.WiFiDirect.Services**](https://msdn.microsoft.com/library/windows/apps/dn996481) API 使你能够提供 Wi-Fi Direct Services 并连接到它们。 借助 Wi-Fi Direct Services，Wi-Fi Direct 临时网络上的设备（服务广告方）便能提供通过 Wi-Fi Direct 连接而连接到其他设备（服务寻求方）的功能。

### <a name="mobile-operators"></a>移动运营商

Windows 10 向开发人员受众公开具有之前只公开给设备制造商和移动运营商的某些 Api。 请注意，尽管这些 API 现已公开，但它们仍受特定应用功能的限制，即必须得到 Microsoft 批准之后，才能发布应用。 这些 API 的实际使用主要限于设备制造商和移动运营商。

### <a name="network-operations"></a>网络运营

[**Windows.Networking.NetworkOperators**](https://msdn.microsoft.com/library/windows/apps/br241148) API 主要用于处理手机的配置和预配。 因此，用于对其进行控制的功能的相关权限仅限于设备制造商和电信提供商。

### <a name="sms"></a>短信

[**Windows.Devices.Sms**](https://msdn.microsoft.com/library/windows/apps/br206567) 命名空间用于以低级别实体形式处理短信和相关消息。 提供它供移动运营商作应用定向的短信之用，并使其受某一功能控制，该功能不被大多数应用开发人员批准使用。 如果你要编写一个用于处理消息的应用，应改为使用 [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/dn642321) API，由于其设计初衷并非仅用于处理短信，还能用于处理来自其他来源（如实时聊天应用）的消息，从而能提供更为丰富的聊天/消息传递体验。

