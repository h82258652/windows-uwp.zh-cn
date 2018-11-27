---
ms.assetid: 60fc48dd-91a9-4dd6-a116-9292a7c1f3be
title: Windows Device Portal 概述
description: 了解 Windows Device Portal 如何支持你通过网络或 USB 连接远程配置和管理你的设备。
ms.date: 12/12/2017
ms.topic: article
keywords: windows 10，uwp，设备门户
ms.localizationpriority: medium
ms.openlocfilehash: 2bffdb31e9001bd0b2abe873780ef507c2073b46
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2018
ms.locfileid: "7700455"
---
# <a name="windows-device-portal-overview"></a>Windows Device Portal 概述

Windows Device Portal 可使你通过网络或 USB 连接远程配置和管理你的设备。 它还提供高级诊断工具，可帮助你解决并查看 Windows 设备的实时性能。

Windows Device Portal 是你可以通过在电脑上的 web 浏览器连接到的设备上的 web 服务器。 如果你的设备具有 web 浏览器，你可以还与该设备上的浏览器本地连接。

Windows Device Portal 是适用于每个设备系列，但功能和设置因每个设备的要求。 本文提供了 Device Portal 的常规说明以及指向包含每个设备系列的更具体信息的文章链接。

使用[REST Api](device-portal-api-core.md) ，可用于直接访问数据和以编程方式控制设备已实现的 Windows Device Portal 的功能。

## <a name="setup"></a>设置

每台设备都有有关连接到 Device Portal 的特定说明，但每台设备都需要以下常规步骤：
1. 在你的设备 （已在设置应用配置） 上启用开发人员模式和 Device Portal。
2. 通过本地网络或 USB 连接你的设备和电脑。
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

在页面顶部的工具栏提供了对常用功能的访问。
- **电源**： 访问电源选项。
  - **关机**：关闭设备。
  - **重启**：重新接通设备的电源。
- **帮助**：打开帮助页面。

使用沿页面左侧的导航窗格中的链接导航到适用于你的设备的可用管理和监视工具。

此处所述的通用设备系列的工具。 根据设备的不同可能提供其他选项。 有关详细信息，请参阅你的设备类型的特定页面。

### <a name="apps-manager"></a>应用管理器

应用管理器提供安装/卸载和应用管理功能包并将捆绑在主机设备上。

![设备门户应用管理器页](images/device-portal/wdp-apps.png)

- **已安装应用**： 使用下拉菜单中删除或开始菜单在设备安装的应用。 通过单击**添加**安装新的应用。 这将启动安装 UX 部署已打包的应用从本地、 网络或 web 托管和注册 loose 文件从网络共享。
- **正在运行的应用**： 获取有关当前正在运行，并根据需要关闭它们的应用的信息。

#### <a name="install-an-app"></a>安装应用

1.  创建应用包后，可以将其远程安装到设备上。 在 Visual Studio 中生成它后，将生成一个输出文件夹。
  ![应用安装](images/device-portal/iot-installapp0.png)
2.  在设备门户的应用管理器部分中，单击**添加**，然后选择**从本地存储的应用包安装**。
3.  单击**浏览**并找到你的应用包。
3.  单击**浏览**并找到证书 (_.cer_) 文件 （不需要在所有设备上）
4.  如果你想要安装可选的各个框内或框架包以及应用安装检查。 如果你有多个依赖项，请分别添加每一个。     
5.  单击**下一步**移动到下一步和**安装**启动安装。 

#### <a name="uninstall-an-app"></a>卸载应用
1.  确保应用未在运行。 
2.  如果是，请转到**正在运行的应用**，并关闭它。 如果你尝试进行卸载应用运行时，它将尝试重新安装该应用时导致问题。 
3.  从下拉列表中选择的应用，然后单击**删除**。

### <a name="running-processes"></a>正在运行的进程

此页显示有关当前在主机设备上运行的进程的详细信息。 这包括应用和系统进程。 在某些平台 （Desktop、 IoT、 和 HoloLens） 上，你可以终止进程。

![设备门户运行处理页面](images/device-portal/mob-device-portal-processes.png)

### <a name="file-explorer"></a>文件资源管理器

此页面允许你查看和操纵由旁加载的任何应用存储的文件。 请参阅[使用应用文件资源管理器](https://blogs.windows.com/buildingapps/2016/06/08/using-the-app-file-explorer-to-see-your-app-data/)博客文章以了解有关文件资源管理器以及如何使用它的详细信息。 

![设备门户文件资源管理器页](images/device-portal/mob-device-portal-AppFileExplorer.png)

### <a name="performance"></a>性能

性能页显示系统诊断信息，如电源使用情况、 帧速率的实时图形和 CPU 负载。

可用的指标如下所示：
- **CPU**： 的总可用的 CPU 使用率百分比
- **内存**： 总数，在使用中，可用、 提交、 页面缓冲，和非页面缓冲
- **I/O**： 读取和写入数据数量
- **网络**： 接收和发送数据
- **GPU**: %的总可用 GPU 引擎使用率


![设备门户性能页面](images/device-portal/mob-device-portal-perf.png)

### <a name="event-tracing-for-windows-etw-logging"></a>事件 Windows 跟踪 (ETW) 日志记录

ETW 日志记录页面管理设备上的实时事件跟踪 Windows (ETW) 信息。

![设备门户 ETW 日志记录页面](images/device-portal/mob-device-portal-etw.png)

选中**隐藏提供程序**以仅显示“事件”列表。
- **已注册的提供程序**： 选择事件提供程序和跟踪级别。 跟踪级别是以下值之一：
  1. 异常退出或终止
  2. 严重错误
  3. 警告
  4. 非错误警告
  5. 详细跟踪 \

  单击或点击**启用**以开始跟踪。 提供程序将添加到**已启用的提供程序**下拉列表。
- **自定义提供程序**：选择自定义 ETW 提供程序和跟踪级别。 根据其 GUDI 标识提供程序。 不在 GUID 中包含括号。
- **已启用提供程序**： 这将列出已启用的提供程序。 从下拉列表中选择一个提供程序，然后单击或点击**禁用**来停止跟踪。 单击或点击**全部停止**来暂停所有跟踪。
- **提供程序历史记录**： 这将显示当前会话期间启用的 ETW 提供程序。 单击或点击**启用**来激活已禁用的提供程序。 单击或点击**清除**来清除历史记录。
- **筛选器 / 事件**:**事件**部分列出了来自选定提供程序以表格的 ETW 事件。 表将实时更新。 使用**筛选器**菜单设置自定义筛选器将为其显示事件。 单击**清除**按钮以从表中删除所有 ETW 事件。 这不会禁用任何提供程序。 你可以单击**保存到文件**将当前收集的 ETW 事件导出到本地 CSV 文件。

有关使用 ETW 日志记录的更多详细信息，请参阅[使用设备门户，以查看调试日志](https://blogs.windows.com/buildingapps/2016/06/10/using-device-portal-to-view-debug-logs-for-uwp/)博客文章。 

### <a name="performance-tracing"></a>性能跟踪

性能跟踪页面允许你为视图与主机设备的[Windows Performance Recorder (WPR)](https://msdn.microsoft.com/library/hh448205.aspx)跟踪。

![设备门户性能跟踪页面](images/device-portal/mob-device-portal-perf-tracing.png)

- **可用配置文件**：从下拉列表中选择 WPR 配置文件，然后单击或点击**开始**以开始跟踪。
- **自定义配置文件**：单击或点击**浏览**以从电脑中选择 WPR 配置文件。 单击或点击**上载并启动**以开始跟踪。

若要停止跟踪，请单击**停止**。 停留在此页面上，直到跟踪文件 (。ETL) 完成下载。

捕获。可用于在[Windows Performance Analyzer](https://msdn.microsoft.com/library/windows/desktop/hh448170.aspx)分析打开 ETL 文件。

### <a name="device-manager"></a>设备管理器

设备管理器页面枚举连接到你的设备的所有外围设备。 你可以单击要查看每个属性的设置图标。

![设备门户设备管理器页](images/device-portal/mob-device-portal-devices.png)

### <a name="networking"></a>网络

网络页面管理设备上的网络连接。 除非你连接到 Device Portal 通过 USB，更改这些设置很可能使断开连接你从 Device Portal。
- **可用的网络**： 显示适用于该设备的 WiFi 网络。 单击或点击某个网络将允许你连接到该网络，并提供密钥（如果需要）。 Device Portal 尚不支持企业身份验证。 你还可以使用**配置文件**下拉列表来尝试连接到任何已知到设备的 WiFi 配置文件。
- **IP 配置**： 显示设备的网络端口的有关其中每个主机的地址信息。

![设备门户网络页面](images/device-portal/mob-device-portal-network.png)

## <a name="service-features-and-notes"></a>服务功能和说明

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
> 此保护可防止从独立客户端 （如命令行实用程序） 使用 REST Api。 这可以通过 3 种方法解决： 
> - 使用"auto-"用户名。 将“auto-”置于其用户名前面的客户端将绕过 CSRF 保护。 此用户名不能用于通过浏览器登录到 Device Portal，这一点很重要，因为它将针对 CSRF 攻击打开服务。 示例：如果 Device Portal 的用户名为“admin”，则 ```curl -u auto-admin:password <args>``` 应该用于绕过 CSRF 保护。 
> - 在客户端中实现 Cookie 到标头的方案。 这需要 GET 请求来建立会话 Cookie，并包含所有后续请求的标头和 Cookie。 
> - 禁用身份验证并使用 HTTP。 CSRF 保护仅适用于 HTTPS 终结点，因此 HTTP 终结点上的连接无需执行上述任一操作。 

#### <a name="cross-site-websocket-hijacking-cswsh-protection"></a>跨站点 WebSocket 劫持 (CSWSH) 保护

若要防止受到 [CSWSH 攻击](https://www.christian-schneider.net/CrossSiteWebSocketHijacking.html)，用于打开对 Device Portal 的 WebSocket 连接的所有客户端还必须提供与主机头匹配的 Origin 标头。 这向 Device Portal 证明了请求来自 Device Portal UI 或有效的客户端应用程序。 如果没有 Origin 标头，将拒绝你的请求。 
