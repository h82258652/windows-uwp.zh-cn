---
author: DelfCo
ms.assetid: 7bb9fd81-8ab5-4f8d-a854-ce285b0669a4
description: "用于访问网络和 Web 服务的技术。"
title: "网络和 Web 服务"
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: df62edbf63777a8fcec4ab08c1fb56ab76ea6624

---

# 网络和 Web 服务

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

以下网络和 Web 服务技术可供通用 Windows 平台 (UWP) 开发人员使用。

| 主题                                                                                   | 说明                                                                      |
|-----------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|
| [网络基础知识](networking-basics.md)                                               | 针对任何支持网络的应用的必做事项。                     |
| [选择哪一种网络技术？](which-networking-technology.md)                          | 适用于 UWP 开发人员的网络技术概述，附带关于如何选择适合应用的技术的建议。               |
| [后台网络通信](network-communications-in-the-background.md) | 当应用不在前台时，它们使用后台任务和两个主机制来保持通信：套接字代理和控制通道触发器。                  |
| [套接字](sockets.md)                                                                   | 作为一名 UWP 应用开发人员，你可以将 [Windows.Networking.Sockets](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.networking.sockets.aspx) 与 [Winsock](https://msdn.microsoft.com/library/windows/desktop/ms737523) 结合使用，以便与其他设备通信。 本主题提供有关使用 Windows.Networking.Sockets 命名空间执行网络操作的详细指南。 |
| [WebSockets](websockets.md)                                                             | WebSocket 提供了一种机制，用于使用 HTTP 通过 Web 在客户端与服务器之间进行既快捷又安全的双向通信。                 |
| [HttpClient](httpclient.md)                                                             | 依据 HTTP 2.0 和 HTTP 1.1 协议，使用 [Windows.Web.Http](https://msdn.microsoft.com/library/windows/apps/dn279692) 命名空间 API 发送和接收信息。             |
| [RSS/Atom 源](web-feeds.md)                                                          | 使用综合源检索或创建最新和最热门的 Web 内容，这些源通过 [Windows.Web.Syndication](https://msdn.microsoft.com/library/windows/apps/br243632) 命名空间中的功能根据 RSS 和 Atom 标准生成。                   |
| [后台传输](background-transfers.md)                                         | 使用后台传输 API 以通过网络可靠地复制文件。           |
| [使用 EDP 标识标记网络连接](tagging_network_connections_with_edp_identity.md) | 本主题介绍了如何在企业数据保护 (EDP) 方案中创建网络连接之前创建受保护的线程上下文。 |



<!--HONumber=Jun16_HO4-->


