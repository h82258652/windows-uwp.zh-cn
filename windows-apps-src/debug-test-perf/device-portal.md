---
author: PatrickFarley
ms.assetid: 60fc48dd-91a9-4dd6-a116-9292a7c1f3be
title: Windows Device Portal 概述
description: 了解 Windows Device Portal 如何支持你通过网络或 USB 连接远程配置和管理你的设备。
ms.author: pafarley
ms.date: 12/12/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp，设备门户
ms.localizationpriority: medium
ms.openlocfilehash: 08e7d8fcfbab0d0b22fffa3e3e0aecc38d5b095c
ms.sourcegitcommit: f2f4820dd2026f1b47a2b1bf2bc89d7220a79c1a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/22/2018
ms.locfileid: "2799654"
---
# <a name="windows-device-portal-overview"></a>Windows Device Portal 概述

Windows Device Portal 可使你通过网络或 USB 连接远程配置和管理你的设备。 它还提供高级诊断工具可以帮助您解决并查看 Windows 设备的实时性能。

Windows 设备门户是您可以从 web 浏览器在 PC 上连接到的设备上的 web 服务器。 如果您的设备具有 web 浏览器，您还可以连接本地与该设备上的浏览器。

Windows 设备门户位于上每个设备系列，但功能和设置因每个设备的要求。 本文提供了 Device Portal 的常规说明以及指向包含每个设备系列的更具体信息的文章链接。

使用您可以使用直接来访问数据并以编程方式控制您的设备的[REST Api](device-portal-api-core.md)实现 Windows 设备门户的功能。

## <a name="setup"></a>设置

每台设备都有有关连接到 Device Portal 的特定说明，但每台设备都需要以下常规步骤：
1. 启用开发人员模式和设备门户 （配置设置应用程序中） 设备上。
2. 通过本地网络或 USB 连接设备和 PC。
3. 在浏览器中导航到 Device Portal 页面。 此表显示的端口和协议使用的每个设备系列。

设备系列 | 默认启用？ | HTTP | HTTPS | USB
--------------|----------------|------|-------|----
HoloLens | 是，处于开发人员模式下 | 80（默认值） | 443（默认值） | http://127.0.0.1:10080
IoT | 是，处于开发人员模式下 | 8080 | 通过注册表项启用 | 不适用
Xbox | 在开发人员模式内启用 | 已禁用 | 11443 | 不适用
桌面设备| 在开发人员模式内启用 | 50080\* | 50043\* | 不适用
电话 | 在开发人员模式内启用 | 80| 443 | http://127.0.0.1:10080

\ * 并非始终是这种情况，桌面上的 Device Portal 声明短暂范围 (&gt; 50000) 内的端口以防止与设备上的现有端口声明冲突。 若要了解详细信息，请参阅适用于桌面的[端口设置](device-portal-desktop.md#registry-based-configuration-for-device-portal)部分。  

有关特定于设备的设置说明，请参阅：
- [适用于 HoloLens 的 Device Portal](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-hololens)
- [适用于 IoT 的 Device Portal](https://go.microsoft.com/fwlink/?LinkID=616499)
- [适用于移动设备的 Device Portal](device-portal-mobile.md)
- [适用于 Xbox 的 Device Portal](device-portal-xbox.md)
- [适用于桌面设备的 Device Portal](device-portal-desktop.md#set-up-device-portal-on-windows-desktop)

## <a name="features"></a>功能

### <a name="toolbar-and-navigation"></a>工具栏和导航

页面顶部的工具栏提供访问常用功能。
- **电源**： 访问电源选项。
  - **关机**：关闭设备。
  - **重启**：重新接通设备的电源。
- **帮助**：打开帮助页面。

使用沿页面左侧的导航窗格中的链接导航到适用于你的设备的可用管理和监视工具。

以下所述的设备系列共有的工具。 根据设备的不同可能提供其他选项。 有关详细信息，请参阅您的设备类型的特定页面。

### <a name="apps-manager"></a>应用管理器

应用程序管理器提供安装/卸载和应用程序的管理功能包，并将捆绑主机设备上。

![设备门户应用程序管理器页](images/device-portal/wdp-apps.png)

- **已安装应用程序**： 使用下拉菜单中删除或启动的设备上安装应用程序。 通过单击**添加**安装新的应用程序。 这将启动安装部署来自本地打包应用程序的 UX、 网络或 web 承载和注册松散从网络共享的文件。
- **运行应用程序**： 获取有关当前正在运行，并根据需要关闭它们的应用程序的信息。

#### <a name="install-an-app"></a>安装应用

1.  创建应用包后，可以将其远程安装到设备上。 在 Visual Studio 中生成它后，将生成一个输出文件夹。
  ![应用安装](images/device-portal/iot-installapp0.png)
2.  设备门户的应用程序管理器部分中，单击**添加**并选择**安装从本地存储的应用程序包**。
3.  单击**浏览**并查找您的应用程序包。
3.  单击**浏览**并查找证书 (_.cer_) 文件 （不需要在所有设备上）
4.  检查的相应框中，如果要安装可选或以及应用程序安装的框架包。 如果你有多个依赖项，请分别添加每一个。     
5.  单击**下一步**将移动到下一步步骤并**安装**，以启动安装。 

#### <a name="uninstall-an-app"></a>卸载应用
1.  确保应用未在运行。 
2.  如果是，转到**运行应用程序**，并将其关闭。 如果您尝试卸载应用程序运行时，它将导致问题，当您尝试重新安装应用程序。 
3.  从下拉列表中选择的应用程序，单击**删除**。

### <a name="running-processes"></a>正在运行的进程

此页显示有关当前主机设备上运行的进程的详细信息。 这包括应用和系统进程。 在某些平台 （桌面、 IoT 和 HoloLens） 上，可以终止的进程。

![设备门户运行处理页](images/device-portal/mob-device-portal-processes.png)

### <a name="file-explorer"></a>文件资源管理器

此页允许您查看和处理文件存储的任何 sideloaded 应用程序。 请参阅[使用应用程序文件资源管理器](https://blogs.windows.com/buildingapps/2016/06/08/using-the-app-file-explorer-to-see-your-app-data/)博客文章以了解有关文件资源管理器和如何使用它。 

![设备门户文件资源管理器页](images/device-portal/mob-device-portal-AppFileExplorer.png)

### <a name="performance"></a>性能

性能页上显示的电源利用率，帧速率，如系统诊断信息的实时图形和 CPU 负载。

可用的指标如下所示：
- **CPU**： 的总的可用 CPU 利用率百分比
- **内存**： 中使用，可用、 提交、 分页，和非分页总计
- **I/O**： 读取和写入数据数量
- **网络**： 接收和发送数据
- **GPU**: %的总可用 GPU 引擎利用率


![设备门户性能页](images/device-portal/mob-device-portal-perf.png)

### <a name="event-tracing-for-windows-etw-logging"></a>事件跟踪 Windows (ETW) 日志记录

ETW 日志记录页上管理设备上的实时事件跟踪 Windows (ETW) 信息。

![设备门户 ETW 日志记录页](images/device-portal/mob-device-portal-etw.png)

选中**隐藏提供程序**以仅显示“事件”列表。
- **已注册提供程序**： 选择事件提供程序和跟踪级别。 跟踪级别是以下值之一：
  1. 异常退出或终止
  2. 严重错误
  3. 警告
  4. 非错误警告
  5. 详细的跟踪

  单击或点击**启用**以开始跟踪。 提供程序将添加到**已启用的提供程序**下拉列表。
- **自定义提供程序**：选择自定义 ETW 提供程序和跟踪级别。 根据其 GUDI 标识提供程序。 Guid 不包括括号。
- **启用提供程序**： 此列出了已启用的提供程序。 从下拉列表中选择一个提供程序，然后单击或点击**禁用**来停止跟踪。 单击或点击**全部停止**来暂停所有跟踪。
- **提供程序历史记录**： 此时将显示在当前会话中已启用的 ETW 提供程序。 单击或点击**启用**来激活已禁用的提供程序。 单击或点击**清除**来清除历史记录。
- **筛选器 / 事件**: **Events**内容列表 ETW 事件中以表格格式的所选提供程序。 实时更新表。 使用**筛选器**菜单设置将为其显示事件的自定义筛选器。 单击**清除**按钮以从表中删除所有 ETW 事件。 这不会禁用任何提供程序。 您可以单击**保存到文件**以将当前收集的 ETW 事件导出到本地的 CSV 文件。

使用 ETW 日志记录的详细信息，请参阅[使用设备门户以查看调试日志](https://blogs.windows.com/buildingapps/2016/06/10/using-device-portal-to-view-debug-logs-for-uwp/)博客文章。 

### <a name="performance-tracing"></a>性能跟踪

性能跟踪页允许您视图从主机设备的[Windows 性能录制 (WPR)](https://msdn.microsoft.com/library/hh448205.aspx)跟踪。

![设备门户性能跟踪页](images/device-portal/mob-device-portal-perf-tracing.png)

- **可用配置文件**：从下拉列表中选择 WPR 配置文件，然后单击或点击**开始**以开始跟踪。
- **自定义配置文件**：单击或点击**浏览**以从电脑中选择 WPR 配置文件。 单击或点击**上载并启动**以开始跟踪。

若要停止跟踪，请单击**停止**。 保持此页上，直到跟踪文件 (。ETL) 已完成下载。

捕获。[Windows 性能分析器](https://msdn.microsoft.com/library/windows/desktop/hh448170.aspx)中的分析，可以打开 ETL 文件。

### <a name="device-manager"></a>设备管理器

设备管理器页上枚举所有外围设备连接到设备。 您可以单击要查看的每个属性的设置图标。

![设备门户设备管理器页](images/device-portal/mob-device-portal-devices.png)

### <a name="networking"></a>网络

网络页上管理设备上的网络连接。 除非您连接到通过 USB 设备门户，更改这些设置将会可能断开您从设备门户。
- **可用网络**： 说明 WiFi 网络对设备可用。 单击或点击某个网络将允许你连接到该网络，并提供密钥（如果需要）。 设备门户还不支持 Enterprise 身份验证。 您还可以使用**配置文件**下拉列表尝试连接到任何已知的设备的 WiFi 配置文件。
- **IP 配置**： 显示设备的网络端口的每个主机地址信息。

![设备门户网络页](images/device-portal/mob-device-portal-network.png)

## <a name="service-features-and-notes"></a>服务功能和注释

### <a name="dns-sd"></a>DNS-SD

Device Portal 使用 DNS-SD 公布其存在于本地网络上。 所有 Device Portal 实例都将在“WDP._wdp._tcp.local”下公布，无论其设备类型如何。 服务实例的 TXT 记录提供以下信息：

项 | 类型 | 描述 
----|------|-------------
S | int | Device Portal 的安全端口。 如果为 0（零），则表示 Device Portal 不侦听 HTTPS 连接。 
D | string | 设备类型。 这将采用“Windows.*”格式，例如 Windows.Xbox 或 Windows.Desktop
A | string | 设备体系结构。 这将为 ARM、x86 或 AMD64。  
T | 字符串的 null 字符分隔列表 | 用户应用的设备标记。 有关其用法的信息，请参阅标记 REST API。 列表以双 null 结尾。  

推荐 HTTPS 端口上的连接，因为并非所有设备都侦听 DNS-SD 记录所公布的 HTTP 端口。 

### <a name="csrf-protection-and-scripting"></a>CSRF 保护和脚本

为了防止受到 [CSRF 攻击](https://wikipedia.org/wiki/Cross-site_request_forgery)，所有非 GET 请求上都需要唯一的令牌。 此令牌 X-CSRF-Token 请求标头派生自会话 Cookie CSRF-Token。 在 Device Portal Web UI 中，CSRF-Token Cookie 将复制到每个请求的 X-CSRF-Token 标头中。

> [!IMPORTANT]
> 此保护阻止的 REST Api 从独立客户端 （如命令行实用程序） 的用法。 这可以通过 3 种方法解决： 
> - 使用"自动-"用户名。 将“auto-”置于其用户名前面的客户端将绕过 CSRF 保护。 此用户名不能用于通过浏览器登录到 Device Portal，这一点很重要，因为它将针对 CSRF 攻击打开服务。 示例：如果 Device Portal 的用户名为“admin”，则 ```curl -u auto-admin:password <args>``` 应该用于绕过 CSRF 保护。 
> - 在客户端中实现 Cookie 到标头的方案。 这需要 GET 请求来建立会话 Cookie，并包含所有后续请求的标头和 Cookie。 
> - 禁用身份验证并使用 HTTP。 CSRF 保护仅适用于 HTTPS 终结点，因此 HTTP 终结点上的连接无需执行上述任一操作。 

#### <a name="cross-site-websocket-hijacking-cswsh-protection"></a>跨站点 WebSocket 劫持 (CSWSH) 保护

若要防止受到 [CSWSH 攻击](https://www.christian-schneider.net/CrossSiteWebSocketHijacking.html)，用于打开对 Device Portal 的 WebSocket 连接的所有客户端还必须提供与主机头匹配的 Origin 标头。 这向 Device Portal 证明了请求来自 Device Portal UI 或有效的客户端应用程序。 如果没有 Origin 标头，将拒绝你的请求。 
