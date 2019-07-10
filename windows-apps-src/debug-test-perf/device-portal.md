---
ms.assetid: 60fc48dd-91a9-4dd6-a116-9292a7c1f3be
title: Windows Device Portal 概述
description: 了解 Windows Device Portal 如何支持你通过网络或 USB 连接远程配置和管理你的设备。
ms.date: 04/09/2019
ms.topic: article
keywords: windows 10，uwp，设备门户
ms.localizationpriority: medium
ms.openlocfilehash: 1f776a9d0ffe15f4bec26fbf8a26ce52a73345e9
ms.sourcegitcommit: 139717a79af648a9231821bdfcaf69d8a1e6e894
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2019
ms.locfileid: "67713869"
---
# <a name="windows-device-portal-overview"></a>Windows Device Portal 概述

Windows Device Portal 可使你通过网络或 USB 连接远程配置和管理你的设备。 它还提供高级诊断工具来帮助您进行故障排除和查看你的 Windows 设备的实时性能。

Windows Device Portal 是可以从一台 PC 上的 web 浏览器连接到在设备上的 web 服务器。 如果你的设备有 web 浏览器，您还可以连接本地与该设备上的浏览器。

Windows Device Portal 位于每个设备系列，但功能和安装程序因每个设备的要求。 本文提供了 Device Portal 的常规说明以及指向包含每个设备系列的更具体信息的文章链接。

Windows Device Portal 的功能通过实现[REST Api](device-portal-api-core.md) ，可用于直接访问数据，并以编程方式控制你的设备。

## <a name="setup"></a>安装

每台设备都有有关连接到 Device Portal 的特定说明，但每台设备都需要以下常规步骤：

1. （在设置应用中配置） 在设备上启用开发人员模式和设备门户。

2. 通过本地网络或使用 USB 连接设备和 PC。

3. 在浏览器中导航到 Device Portal 页面。 此表显示的端口和协议使用的每个设备系列。

设备系列 | 默认启用？ | HTTP | HTTPS | USB
--------------|----------------|------|-------|----
HoloLens | 是，处于开发人员模式下 | 80（默认值） | 443（默认值） | http://127.0.0.1:10080
IoT | 是，处于开发人员模式下 | 8080 | 通过注册表项启用 | 不可用
Xbox | 在开发人员模式内启用 | Disabled | 11443 | 不可用
桌面设备| 在开发人员模式内启用 | 50080\* | 50043\* | 不可用
Phone | 在开发人员模式内启用 | 80| 443 | http://127.0.0.1:10080

\* 这并不总是这种情况，如桌面上设备门户声明临时范围中的端口 (> 50,000) 以防止与在设备上的现有端口声明的碰撞。 若要了解详细信息，请参阅适用于桌面的[端口设置](device-portal-desktop.md#registry-based-configuration-for-device-portal)部分。  

有关特定于设备的设置说明，请参阅：

- [适用于 HoloLens 的设备门户](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-hololens)
- [适用于 IoT 的设备门户](https://go.microsoft.com/fwlink/?LinkID=616499)
- [适用于移动设备的设备门户](device-portal-mobile.md)
- [适用于 Xbox 的设备门户](../xbox-apps/device-portal-xbox.md)
- [适用于台式机的设备门户](device-portal-desktop.md#set-up-device-portal-on-windows-desktop)

## <a name="features"></a>功能

### <a name="toolbar-and-navigation"></a>工具栏和导航

在页面顶部的工具栏提供了对常用功能的访问。

- **Power**:访问电源选项。
  - **关闭**:关闭设备。
  - **重新启动**:在设备上关闭后再打开。
- **帮助**:将打开帮助页。

使用沿页面左侧的导航窗格中的链接导航到适用于你的设备的可用管理和监视工具。

此处介绍了在设备系列常见的工具。 根据设备的不同可能提供其他选项。 有关详细信息，请参阅你的设备类型的特定页。

### <a name="apps-manager"></a>应用管理器

应用程序管理器提供了安装/卸载和应用程序的管理功能包和捆绑主机设备上。

![设备门户应用程序管理器页](images/device-portal/WDP_AppsManager2.png)

* **将应用部署**:部署打包应用程序从本地、 网络或 web 主机，并注册从网络共享的松散文件。
* **已安装的应用**:使用下拉列表菜单中删除或启动设备上安装的应用。
* **正在运行的应用**:获取有关当前正在运行，请根据需要关闭它们的应用的信息。

#### <a name="install-sideload-an-app"></a>安装 （旁加载） 应用

开发期间使用 Windows Device Portal，您可以旁加载应用程序：

1. 创建应用程序包后，可以远程将它安装到你的设备。 在 Visual Studio 中生成它后，将生成一个输出文件夹。

    ![应用安装](images/device-portal/iot-installapp0.png)

2. 在 Windows Device Portal，导航到**应用程序管理器**页。

3. 在中**部署应用**部分中，选择**本地存储**。

4. 下**选择的应用程序包**，选择**选择文件**并浏览到所需旁加载应用程序包。

5. 下**选择证书文件 (.cer) 用于签署应用程序包**，选择**选择文件**并浏览到与该应用程序包关联的证书。

6. 检查相应的框，如果您要安装可选或框架包以及应用程序安装过程中，并选择**下一步**选择它们。

7. 选择**安装**启动安装。

8. 如果设备正在运行 Windows 10 S 模式下，它是给定的证书已安装在设备第一次重启设备。

#### <a name="install-a-certificate"></a>安装证书

或者，可以通过 Windows Device Portal 将证书安装和安装通过其他方式应用：

1. 在 Windows Device Portal，导航到**应用程序管理器**页。

2. 在中**部署应用**部分中，选择**安装证书**。

3. 下**选择证书文件 (.cer) 用于签名应用程序包**，选择**选择文件**并浏览到与所需旁加载应用程序包关联的证书。

4. 选择**安装**启动安装。

5. 如果设备正在运行 Windows 10 S 模式下，它是给定的证书已安装在设备第一次重启设备。

#### <a name="uninstall-an-app"></a>卸载应用

1. 确保应用未在运行。
2. 如果是，请转到**正在运行的应用**并将其关闭。 如果你尝试卸载该应用程序运行时，它将尝试重新安装该应用时导致问题。
3. 从下拉列表中单击选择的应用**删除**。

### <a name="running-processes"></a>正在运行的进程

此页显示有关当前主机设备上运行的进程的详细信息。 这包括应用和系统进程。 在某些平台 （桌面、 IoT 和 HoloLens），可以终止进程。

![设备门户运行处理页](images/device-portal/mob-device-portal-processes.png)

### <a name="file-explorer"></a>文件资源管理器

此页可以查看并处理由任何旁加载应用存储的文件。 请参阅[使用应用程序文件资源管理器](https://blogs.windows.com/buildingapps/2016/06/08/using-the-app-file-explorer-to-see-your-app-data/)博客文章，了解有关在文件资源管理器以及如何使用它的详细信息。

![设备门户文件资源管理器页](images/device-portal/mob-device-portal-AppFileExplorer.png)

### <a name="performance"></a>性能

性能页显示的系统诊断信息，如电源使用情况，帧速率的实时图形和 CPU 负载。

可用的指标如下所示：

- **CPU**:总可用 CPU 使用率百分比
- **内存**:总计，在使用中，可用、 已提交页面，和非页面缓冲
- **I/O**:读取和写入数据的数量
- **网络**：接收和发送数据
- **GPU**:总的可用 GPU 引擎使用率的百分比

![设备门户性能页](images/device-portal/mob-device-portal-perf.png)

### <a name="event-tracing-for-windows-etw-logging"></a>事件跟踪 Windows (ETW) 日志记录

ETW 日志记录页管理设备上的实时事件跟踪 Windows (ETW) 信息。

![设备门户 ETW 日志记录页](images/device-portal/mob-device-portal-etw.png)

选中**隐藏提供程序**以仅显示“事件”列表。

- **注册的提供程序**:选择事件提供程序和跟踪级别。 跟踪级别是下列值之一：
  1. 异常退出或终止
  2. 严重错误
  3. 警告
  4. 非错误警告
  5. 详细的跟踪

  单击或点击**启用**以开始跟踪。 提供程序将添加到**已启用的提供程序**下拉列表。
- **自定义提供程序**:选择自定义的 ETW 提供程序和跟踪级别。 根据其 GUDI 标识提供程序。 不包含括号的 guid。
- **已启用提供程序**:这会列出已启用提供程序。 从下拉列表中选择一个提供程序，然后单击或点击**禁用**来停止跟踪。 单击或点击**全部停止**来暂停所有跟踪。
- **提供程序历史记录**:这将显示在当前会话期间已启用 ETW 提供程序。 单击或点击**启用**来激活已禁用的提供程序。 单击或点击**清除**来清除历史记录。
- **筛选器 / 事件**:**事件**部分列出了从表格格式中的所选提供程序的 ETW 事件。 实时更新表。 使用**筛选器**菜单来设置自定义筛选器将为其显示事件。 单击**清除**按钮以从表中删除所有 ETW 事件。 这不会禁用任何提供程序。 可以单击**保存到文件**将当前收集的 ETW 事件导出到本地的 CSV 文件。

使用 ETW 日志记录的更多详细信息，请参阅[使用设备门户查看调试日志](https://blogs.windows.com/buildingapps/2016/06/10/using-device-portal-to-view-debug-logs-for-uwp/)博客文章。

### <a name="performance-tracing"></a>性能跟踪

性能跟踪页面允许您查看[Windows 性能记录器 (WPR)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh448205(v=win.10))来自主机设备的跟踪。

![设备门户性能跟踪页面](images/device-portal/mob-device-portal-perf-tracing.png)

- **可用的配置文件**:从下拉列表中，然后单击或点击选择 WPR 配置文件**启动**要启动跟踪。
- **自定义配置文件**:单击或点击**浏览**WPR 配置文件选择你的 PC 中。 单击或点击**上载并启动**以开始跟踪。

若要停止跟踪，请单击**停止**。 停留在此页上，直到跟踪文件 (。下载完成后 ETL)。

捕获。ETL 文件可以打开在中进行分析[Windows 性能分析器](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh448170(v=win.10))。

### <a name="device-manager"></a>设备管理器

设备管理器页枚举所有外围设备连接到你的设备。 您可以单击设置图标以查看每个属性。

![设备门户设备管理器页](images/device-portal/mob-device-portal-devices.png)

### <a name="networking"></a>网络

网络页管理在设备上的网络连接。 除非您连接到通过 USB 设备门户，更改这些设置将很可能你断开与连接设备门户。

- **可用网络**:显示可用于设备的 WiFi 网络。 单击或点击某个网络将允许你连接到该网络，并提供密钥（如果需要）。 设备门户尚不支持企业身份验证。 此外可以使用**配置文件**下拉列表中以尝试连接到任何已知的设备的 WiFi 配置文件。
- **IP 配置**:显示的地址有关的每个主机设备的网络端口的信息。

![设备网络门户页](images/device-portal/mob-device-portal-network.png)

## <a name="service-features-and-notes"></a>服务功能和说明

### <a name="dns-sd"></a>DNS-SD

Device Portal 使用 DNS-SD 公布其存在于本地网络上。 所有 Device Portal 实例都将在“WDP._wdp._tcp.local”下公布，无论其设备类型如何。 服务实例的 TXT 记录提供以下信息：

键 | type | 描述
----|------|-------------
S | int | Device Portal 的安全端口。 如果为 0（零），则表示 Device Portal 不侦听 HTTPS 连接。
D | string | 设备类型。 这将采用“Windows.*”格式，例如 Windows.Xbox 或 Windows.Desktop
A | string | 设备体系结构。 这将为 ARM、x86 或 AMD64。  
T | 字符串的 null 字符分隔列表 | 用户应用的设备标记。 有关其用法的信息，请参阅标记 REST API。 列表以双 null 结尾。  

推荐 HTTPS 端口上的连接，因为并非所有设备都侦听 DNS-SD 记录所公布的 HTTP 端口。

### <a name="csrf-protection-and-scripting"></a>CSRF 保护和脚本

为了防止受到 [CSRF 攻击](https://en.wikipedia.org/wiki/Cross-site_request_forgery)，所有非 GET 请求上都需要唯一的令牌。 此令牌 X-CSRF-Token 请求标头派生自会话 Cookie CSRF-Token。 在 Device Portal Web UI 中，CSRF-Token Cookie 将复制到每个请求的 X-CSRF-Token 标头中。

> [!IMPORTANT]
> 这种保护会阻止从独立的客户端 （如命令行实用程序） 的 REST api 的用法。 这可以通过 3 种方法解决：
> - 使用"自动-"用户名。 将“auto-”置于其用户名前面的客户端将绕过 CSRF 保护。 此用户名不能用于通过浏览器登录到 Device Portal，这一点很重要，因为它将针对 CSRF 攻击打开服务。 例如：如果设备门户的用户名是"admin"```curl -u auto-admin:password <args>```应该用于绕过 CSRF 保护。
> - 在客户端中实现 Cookie 到标头的方案。 这需要 GET 请求来建立会话 Cookie，并包含所有后续请求的标头和 Cookie。
> - 禁用身份验证并使用 HTTP。 CSRF 保护仅适用于 HTTPS 终结点，因此 HTTP 终结点上的连接无需执行上述任一操作。

#### <a name="cross-site-websocket-hijacking-cswsh-protection"></a>跨站点 WebSocket 劫持 (CSWSH) 保护

若要防止受到 [CSWSH 攻击](https://www.christian-schneider.net/CrossSiteWebSocketHijacking.html)，用于打开对 Device Portal 的 WebSocket 连接的所有客户端还必须提供与主机头匹配的 Origin 标头。 这向 Device Portal 证明了请求来自 Device Portal UI 或有效的客户端应用程序。 如果没有 Origin 标头，将拒绝你的请求。
