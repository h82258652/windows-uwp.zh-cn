---
ms.assetid: 60fc48dd-91a9-4dd6-a116-9292a7c1f3be
title: Windows Device Portal 概述
description: 了解 Windows Device Portal 如何支持你通过网络或 USB 连接远程配置和管理你的设备。
ms.date: 04/09/2019
ms.topic: article
keywords: windows 10、uwp、设备门户
ms.localizationpriority: medium
ms.openlocfilehash: e2d149faaa967846244553ed53ed5e8255dae276
ms.sourcegitcommit: 445320ff0ee7323d823194d4ec9cfa6e710ed85d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/11/2019
ms.locfileid: "72282281"
---
# <a name="windows-device-portal-overview"></a>Windows Device Portal 概述

Windows Device Portal 可使你通过网络或 USB 连接远程配置和管理你的设备。 它还提供了高级诊断工具，可帮助你排查和查看 Windows 设备的实时性能。

Windows 设备门户是设备上的 web 服务器，你可以从电脑上的 web 浏览器连接到该服务器。 如果设备有 web 浏览器，还可以使用该设备上的浏览器进行本地连接。

Windows 设备门户在每个设备系列上都可用，但功能和设置因每个设备的要求而异。 本文提供了 Device Portal 的常规说明以及指向包含每个设备系列的更具体信息的文章链接。

Windows 设备门户的功能是通过[REST api](device-portal-api-core.md)实现的，你可以直接使用它来访问数据并以编程方式控制设备。

## <a name="setup"></a>安装

每台设备都有有关连接到 Device Portal 的特定说明，但每台设备都需要以下常规步骤：

1. 在设备上启用开发人员模式和设备门户（在设置应用中配置）。

2. 通过本地网络或使用 USB 连接设备和电脑。

3. 在浏览器中导航到 Device Portal 页面。 下表显示了每个设备系列使用的端口和协议。

设备系列 | 默认启用？ | HTTP | HTTPS | USB
--------------|----------------|------|-------|----
HoloLens | 是，处于开发人员模式下 | 80（默认值） | 443（默认值） | http://127.0.0.1:10080
IoT | 是，处于开发人员模式下 | 8080 | 通过注册表项启用 | 不可用
Xbox | 在开发人员模式内启用 | Disabled | 11443 | 不可用
桌面设备| 在开发人员模式内启用 | 50080 @ no__t-0 | 50043 @ no__t-0 | 不可用
Phone | 在开发人员模式内启用 | 80| 443 | http://127.0.0.1:10080

\* 这并不总是如此，因为桌面上的设备门户在暂时范围（> 50000）中声明端口会阻止设备上的现有端口声明发生冲突。 若要了解详细信息，请参阅适用于桌面的[端口设置](device-portal-desktop.md#registry-based-configuration-for-device-portal)部分。  

有关特定于设备的设置说明，请参阅：

- [适用于 HoloLens 的设备门户](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-hololens)
- [适用于 IoT 的设备门户](https://go.microsoft.com/fwlink/?LinkID=616499)
- [适用于移动设备的设备门户](device-portal-mobile.md)
- [适用于 Xbox 的设备门户](../xbox-apps/device-portal-xbox.md)
- [适用于台式机的设备门户](device-portal-desktop.md#set-up-device-portal-on-windows-desktop)

## <a name="features"></a>功能

### <a name="toolbar-and-navigation"></a>工具栏和导航

页面顶部的工具栏提供了对常用功能的访问权限。

- **电源**：访问电源选项。
  - **关闭**：关闭设备。
  - **重新启动**：在设备上循环电源。
- **帮助**：打开 "帮助" 页。

使用沿页面左侧的导航窗格中的链接导航到适用于你的设备的可用管理和监视工具。

此处介绍了跨设备家族公用的工具。 根据设备的不同可能提供其他选项。 有关详细信息，请参阅设备类型的特定页面。

### <a name="apps-manager"></a>应用管理器

应用程序管理器为主机设备上的应用包和捆绑包提供安装/卸载和管理功能。

![设备门户应用管理器页](images/device-portal/WDP_AppsManager2.png)

* **部署应用**：部署来自本地、网络或 web 主机的打包应用，并从网络共享注册松散文件。
* **已安装的应用**：使用下拉菜单来删除或启动安装在设备上的应用。
* **正在运行的应用**：获取有关当前正在运行的应用程序的信息，并根据需要将其关闭。

#### <a name="install-sideload-an-app"></a>安装（旁加载）应用

你可以在开发过程中使用 Windows 设备门户来旁加载应用程序：

1. 创建应用包后，可以将其远程安装到设备上。 在 Visual Studio 中生成它后，将生成一个输出文件夹。

    ![应用安装](images/device-portal/iot-installapp0.png)

2. 在 Windows 设备门户中，导航到 "**应用程序管理器**" 页。

3. 在 "**部署应用**" 部分，选择 "**本地存储**"。

4. 在 "**选择应用程序包**" 下，选择 "**选择文件**"，然后浏览到要旁加载的应用程序包。

5. 在 "**选择用于对应用程序包进行签名的证书文件（.cer）** " 下，选择 "**选择文件**"，并浏览到与该应用包关联的证书。

6. 如果要安装可选包或框架包以及应用安装，请选中相应的框，然后选择 "**下一步**" 以选择它们。

7. 选择 "**安装**" 以启动安装。

8. 如果设备在 S 模式下运行 Windows 10，并且是第一次在设备上安装了给定证书，请重新启动设备。

#### <a name="install-a-certificate"></a>安装证书

或者，你可以通过 Windows 设备门户安装证书，并通过其他方式安装应用：

1. 在 Windows 设备门户中，导航到 "**应用程序管理器**" 页。

2. 在 "**部署应用**" 部分中，选择 "**安装证书**"。

3. 在 "**选择用于对应用程序包进行签名的证书文件（.cer）** " 下，选择 "**选择文件**"，并浏览到与要旁加载的应用包关联的证书。

4. 选择 "**安装**" 以启动安装。

5. 如果设备在 S 模式下运行 Windows 10，并且是第一次在设备上安装了给定证书，请重新启动设备。

#### <a name="uninstall-an-app"></a>卸载应用

1. 确保应用未在运行。
2. 如果是，请前往**正在运行的应用程序**并将其关闭。 如果尝试在应用程序运行时卸载，则在尝试重新安装应用程序时，将会出现问题。
3. 从下拉列表中选择应用，然后单击 "**删除**"。

### <a name="running-processes"></a>正在运行的进程

此页显示有关主机设备上当前正在运行的进程的详细信息。 这包括应用和系统进程。 在某些平台（桌面、IoT 和 HoloLens）上，可以终止进程。

![设备门户运行进程页](images/device-portal/mob-device-portal-processes.png)

### <a name="file-explorer"></a>文件资源管理器

此页面允许你查看和操作由任何旁加载应用存储的文件。 请参阅[使用应用文件资源管理器](https://blogs.windows.com/buildingapps/2016/06/08/using-the-app-file-explorer-to-see-your-app-data/)博客文章，了解有关文件资源管理器以及如何使用它的详细信息。

![设备门户文件资源管理器页](images/device-portal/mob-device-portal-AppFileExplorer.png)

### <a name="performance"></a>性能

"性能" 页面显示系统诊断信息的实时图形，如电源使用情况、帧速率和 CPU 负载。

可用的指标如下所示：

- **CPU**：可用总 CPU 使用率百分比
- **内存**：总计、使用、可用、已提交、分页和非分页
- **I/O**：读取和写入数据量
- **网络**：接收和发送的数据
- **GPU**：可用 GPU 引擎使用率的总百分比

![设备门户性能页](images/device-portal/mob-device-portal-perf.png)

### <a name="event-tracing-for-windows-etw-logging"></a>Windows 事件跟踪（ETW）日志记录

ETW 日志记录页面管理设备上 Windows （ETW）的实时事件跟踪。

![设备门户 ETW 日志记录页](images/device-portal/mob-device-portal-etw.png)

选中**隐藏提供程序**以仅显示“事件”列表。

- **已注册的提供程序**：选择事件提供程序和跟踪级别。 跟踪级别为以下值之一：
  1. 异常退出或终止
  2. 严重错误
  3. 警告
  4. 非错误警告
  5. 详细跟踪

  单击或点击**启用**以开始跟踪。 提供程序将添加到**已启用的提供程序**下拉列表。
- **自定义提供程序**：选择一个自定义 ETW 提供程序和跟踪级别。 根据其 GUDI 标识提供程序。 不要在 GUID 中包含方括号。
- **已启用的提供程序**：这会列出已启用的提供程序。 从下拉列表中选择一个提供程序，然后单击或点击**禁用**来停止跟踪。 单击或点击**全部停止**来暂停所有跟踪。
- **提供程序历史记录**：这将显示在当前会话期间启用的 ETW 提供程序。 单击或点击**启用**来激活已禁用的提供程序。 单击或点击**清除**来清除历史记录。
- **筛选器/事件**：**Events**节按表格式列出选定提供程序中的 ETW 事件。 表将实时更新。 使用 "**筛选器**" 菜单设置将显示其事件的自定义筛选器。 单击 "**清除**" 按钮可从表中删除所有 ETW 事件。 这不会禁用任何提供程序。 可以单击 "**保存到文件**" 将当前收集的 ETW 事件导出到本地 CSV 文件。

有关使用 ETW 日志记录的更多详细信息，请参阅[使用设备门户查看调试日志](https://blogs.windows.com/buildingapps/2016/06/10/using-device-portal-to-view-debug-logs-for-uwp/)博客文章。

### <a name="performance-tracing"></a>性能跟踪

"性能跟踪" 页允许您从主机设备查看[Windows 性能记录器（"）](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh448205(v=win.10))跟踪。

![设备门户性能跟踪页面](images/device-portal/mob-device-portal-perf-tracing.png)

- **可用配置文件**：从下拉列表中选择 "" "配置文件，然后单击或点击"**启动**"以启动跟踪。
- **自定义配置文件**：单击或点击 "**浏览**"，从电脑选择 "配置文件。 单击或点击**上载并启动**以开始跟踪。

若要停止跟踪，请单击**停止**。 一直停留在此页上，直到跟踪文件（。ETL）已完成下载。

总结.可以在[Windows 性能分析器](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh448170(v=win.10))中打开 ETL 文件以进行分析。

### <a name="device-manager"></a>设备管理器

"设备管理器" 页枚举连接到设备的所有外设。 您可以单击 "设置" 图标以查看每个的属性。

![设备门户 "设备管理器" 页](images/device-portal/mob-device-portal-devices.png)

### <a name="networking"></a>网络

"网络" 页管理设备上的网络连接。 如果通过 USB 连接到设备门户，则更改这些设置可能会断开与设备门户的连接。

- **可用网络**：显示设备可用的 WiFi 网络。 单击或点击某个网络将允许你连接到该网络，并提供密钥（如果需要）。 设备门户尚不支持企业身份验证。 你还可以使用 "**配置文件**" 下拉列表来尝试连接到设备已知的任何 WiFi 配置文件。
- **IP 配置**：显示有关每个主机设备的网络端口的地址信息。

![设备门户的 "网络" 页](images/device-portal/mob-device-portal-network.png)

## <a name="service-features-and-notes"></a>服务功能和说明

### <a name="dns-sd"></a>DNS-SD

Device Portal 使用 DNS-SD 公布其存在于本地网络上。 所有 Device Portal 实例都将在“WDP._wdp._tcp.local”下公布，无论其设备类型如何。 服务实例的 TXT 记录提供以下信息：

Key | 类型 | 描述
----|------|-------------
S | int | Device Portal 的安全端口。 如果为 0（零），则表示 Device Portal 不侦听 HTTPS 连接。
D | string | 设备类型。 这将采用 "Windows. *" 格式，例如，Windows Xbox 或 Windows Desktop
A | string | 设备体系结构。 这将为 ARM、x86 或 AMD64。  
T | 字符串的 null 字符分隔列表 | 用户应用的设备标记。 有关其用法的信息，请参阅标记 REST API。 列表以双 null 结尾。  

推荐 HTTPS 端口上的连接，因为并非所有设备都侦听 DNS-SD 记录所公布的 HTTP 端口。

### <a name="csrf-protection-and-scripting"></a>CSRF 保护和脚本

为了防止受到 [CSRF 攻击](https://en.wikipedia.org/wiki/Cross-site_request_forgery)，所有非 GET 请求上都需要唯一的令牌。 此令牌 X-CSRF-Token 请求标头派生自会话 Cookie CSRF-Token。 在 Device Portal Web UI 中，CSRF-Token Cookie 将复制到每个请求的 X-CSRF-Token 标头中。

> [!IMPORTANT]
> 此保护可防止从独立客户端（例如命令行实用程序）使用 REST Api。 这可以通过 3 种方法解决：
> - 使用 "自动" 用户名。 将“auto-”置于其用户名前面的客户端将绕过 CSRF 保护。 此用户名不能用于通过浏览器登录到 Device Portal，这一点很重要，因为它将针对 CSRF 攻击打开服务。 例如：如果设备门户的用户名为 "admin"，则应使用 ```curl -u auto-admin:password <args>``` 来绕过 CSRF 保护。
> - 在客户端中实现 Cookie 到标头的方案。 这需要 GET 请求来建立会话 Cookie，并包含所有后续请求的标头和 Cookie。
> - 禁用身份验证并使用 HTTP。 CSRF 保护仅适用于 HTTPS 终结点，因此 HTTP 终结点上的连接无需执行上述任一操作。

#### <a name="cross-site-websocket-hijacking-cswsh-protection"></a>跨站点 WebSocket 劫持 (CSWSH) 保护

若要防止受到 [CSWSH 攻击](https://www.christian-schneider.net/CrossSiteWebSocketHijacking.html)，用于打开对 Device Portal 的 WebSocket 连接的所有客户端还必须提供与主机头匹配的 Origin 标头。 这向 Device Portal 证明了请求来自 Device Portal UI 或有效的客户端应用程序。 如果没有 Origin 标头，将拒绝你的请求。
