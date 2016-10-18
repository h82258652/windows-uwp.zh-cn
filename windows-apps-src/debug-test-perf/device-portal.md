---
author: mcleblanc
ms.assetid: 60fc48dd-91a9-4dd6-a116-9292a7c1f3be
title: "Windows Device Portal 概述"
description: "了解 Windows Device Portal 如何支持你通过网络或 USB 连接远程配置和管理你的设备。"
translationtype: Human Translation
ms.sourcegitcommit: 7f6aba331ba27d2c0c2ca7925c452da58e155cb8
ms.openlocfilehash: b316eab1f269dadbe65b7e93b5a33a8e4c4924d7

---
# Windows Device Portal 概述

Windows Device Portal 可使你通过网络或 USB 连接远程配置和管理你的设备。 它还提供高级诊断工具，用于帮助你查看 Windows 设备的实时性能并对其进行疑难解答。

Device Portal 是设备上的 Web 服务器，你可以从电脑上的 Web 浏览器连接到它。 如果你的设备具有 Web 浏览器，你还可以与设备上的浏览器本地连接。

Windows Device Portal 在每个设备系列上都可用，但功能和设置可能因设备的要求而异。 本文提供了 Device Portal 的常规说明以及指向包含每个设备系列的更具体信息的文章链接。

Windows Device Portal 中的所有内容都基于 [REST API](device-portal-api-core.md) 生成，该 API 可用于访问数据和以编程方式控制设备。

## 设置

每台设备都有有关连接到 Device Portal 的特定说明，但每台设备都需要以下常规步骤：
1. 在你的设备上启用开发人员模式和 Device Portal。
2. 通过本地网络或 USB 连接你的设备和电脑。
3. 在浏览器中导航到 Device Portal 页面。 此表显示了每个设备系列所使用的端口和协议。

设备系列 | 默认启用？ | HTTP | HTTPS | USB
--------------|----------------|------|-------|----
HoloLens | 是，处于开发人员模式下 | 80（默认值） | 443（默认值） | localhost:10080
IoT | 是，处于开发人员模式下 | 8080 | 通过注册表项启用 | 不适用
Xbox | 在开发人员模式内启用 | 已禁用 | 11443 | 不适用
桌面设备| 在开发人员模式内启用 | 随机 &gt; 50,000 \(xx080\) | 随机 &gt; 50,000 \(xx443\) | 不适用
电话 | 在开发人员模式内启用 | 80| 443 | localhost:10080

有关特定于设备的设置说明，请参阅：
- [适用于 HoloLens 的 Device Portal](https://dev.windows.com/holographic/using_the_windows_device_portal)
- [适用于 IoT 的 Device Portal](https://go.microsoft.com/fwlink/?LinkID=616499)
- [适用于移动设备的 Device Portal](device-portal-mobile.md#set-up-device-portal-on-window-phone)
- [适用于 Xbox 的 Device Portal](device-portal-xbox.md)
- [适用于桌面设备的 Device Portal](device-portal-desktop.md#set-up-device-portal-on-windows-desktop)

## 功能

### 工具栏和导航

页面顶部的工具栏提供了对常用状态和功能的访问权限。
- **关机**：关闭设备。
- **重启**：重新接通设备的电源。
- **帮助**：打开帮助页面。

使用沿页面左侧的导航窗格中的链接导航到适用于你的设备的可用管理和监视工具。

下面介绍了在设备上常用的工具。 根据设备的不同可能提供其他选项。 有关详细信息，请参阅你的设备的特定页面。

### 主页

Device Portal 会话从主页开始。 主页通常具有设备的相关信息（如名称和 OS 版本）和可为该设备设置的首选项。

### 应用

为设备上的 AppX 程序包和捆绑包提供安装/卸载和管理功能。

![适用于移动设备的 Device Portal](images/device-portal/mob-device-portal-apps.png)

- **已安装的应用**：删除和启动应用。
- **正在运行的应用**：列出当前正在运行的应用。
- **安装应用**：从计算机或网络上的文件夹中选择应用包进行安装。
- **依赖项**：为要安装的应用添加依赖项。
- **部署**：将选定的应用和依赖项部署到设备。

**安装应用**

1.  [创建应用包](https://msdn.microsoft.com/library/windows/apps/xaml/hh454036(v=vs.140).aspx)后，可以将其远程安装到设备上。 在 Visual Studio 中生成它后，将生成一个输出文件夹。

    ![应用安装](images/device-portal/iot-installapp0.png)
2.  单击“浏览”并找到你的应用包 \(.appx\)。
3.  单击“浏览”并找到证书文件 \(.cer\)。 （并非在所有设备上都需要。）
4.  添加依赖项。 如果你有多个依赖项，请分别添加每一个。     
5.  在“部署”****下，单击“转到”****。 
6.  若要安装另一个应用，请单击“重置”****按钮来清除字段。


**卸载应用**

1.  确保应用未在运行。 
2.  如果正在运行，请转到“正在运行的应用”并关闭它。 如果你尝试在应用正在运行时卸载，它将在尝试重新安装应用时导致问题。 
3.  准备就绪后，单击“卸载”****。

### 进程

显示有关当前正在运行的进程的详细信息。 这包括应用和系统进程。

与电脑上的任务管理器非常相似，此页面可使你查看当前正在运行的进程及其内存使用情况。  在某些平台（桌面设备、IoT 和 HoloLens），你可以终止进程。

![适用于移动设备的 Device Portal](images/device-portal/mob-device-portal-processes.png)

### 性能

显示系统诊断信息的实时图形，如电源使用情况、帧速率和 CPU 负载。

以下是可用指标：
- **CPU**：总可用量的百分比
- **内存**：总量、正在使用、可用提交、页面缓冲和非页面缓冲
- **GPU**：GPU 引擎使用率、总可用量的百分比
- **I/O**：读取和写入
- **网络**：已接收和已发送

![适用于移动设备的 Device Portal](images/device-portal/mob-device-portal-perf.png)

### Windows 事件跟踪 \(ETW\)

管理设备上的实时 Windows 事件跟踪 \(ETW\)。

![适用于移动设备的 Device Portal](images/device-portal/mob-device-portal-etw.png)

选中“隐藏提供程序”****以仅显示“事件”列表。
- **注册的提供程序**：选择 ETW 提供程序和跟踪级别。 跟踪级别是以下值之一：
    1. 异常退出或终止
    2. 严重错误
    3. 警告
    4. 非错误警告
    5. 详细跟踪 \(*\)

单击或点击“启用”****以开始跟踪。 提供程序将添加到“已启用的提供程序”****下拉列表。
- **自定义提供程序**：选择自定义 ETW 提供程序和跟踪级别。 根据其 GUDI 标识提供程序。 不要在 GUID 中包含括号。
- **已启用的提供程序**：列出已启用的提供程序。 从下拉列表中选择一个提供程序，然后单击或点击“禁用”****来停止跟踪。 单击或点击“全部停止”****来暂停所有跟踪。
- **提供程序历史记录**：显示已在当前会话期间启用的 ETW 提供程序。 单击或点击“启用”****来激活已禁用的提供程序。 单击或点击“清除”****来清除历史记录。
- **事件**：以表格的形式列出来自选定提供程序的 ETW 事件。 此表将实时更新。 在该表下方，单击“清除”****按钮可删除表中的所有 ETW 事件。 这不会禁用任何提供程序。 你可以单击“保存到文件”****来将当前收集的 ETW 事件本地导出到 CSV 文件。

有关使用 ETW 跟踪的更多详细信息，请参阅关于将其用于从你的应用收集实时日志的[博客文章](https://blogs.windows.com/buildingapps/2016/06/10/using-device-portal-to-view-debug-logs-for-uwp/)。 

### 性能跟踪

从设备中捕获 [Windows Performance Recorder](https://msdn.microsoft.com/library/windows/hardware/hh448205.aspx) \(WPR\) 跟踪。

![适用于移动设备的 Device Portal](images/device-portal/mob-device-portal-perf-tracing.png)

- **可用配置文件**：从下拉列表中选择 WPR 配置文件，然后单击或点击“开始”****以开始跟踪。
- **自定义配置文件**：单击或点击“浏览”****以从电脑中选择 WPR 配置文件。 单击或点击“上载并启动”****以开始跟踪。

若要停止跟踪，请单击“停止”****。 停留在此页面上，直到跟踪文件 \(.ETL\) 完成下载。

可以打开捕获的 ETL 文件以供在 [Windows Performance Analyzer](https://msdn.microsoft.com/library/windows/hardware/hh448170.aspx) 中进行分析。

### 设备

枚举连接到你的设备的所有外围设备。

![适用于移动设备的 Device Portal](images/device-portal/mob-device-portal-devices.png)

### 网络

管理设备上的网络连接。  除非你通过 USB 连接到 Device Portal，否则更改这些设置很可能使你与 Device Portal 断开连接。
- **配置文件**：选择其他 WLAN 配置文件以供使用。  
- **可用网络**：可用于该设备的 WLAN 网络。 单击或点击某个网络将允许你连接到该网络，并提供密钥（如果需要）。 注意：Device Portal 尚不支持企业身份验证。 

![适用于移动设备的 Device Portal](images/device-portal/mob-device-portal-network.png)

### 应用文件资源管理器

允许查看和操纵由旁加载的应用存储的文件。  这是 Windows Phone 8.1 中[独立存储资源管理器](https://msdn.microsoft.com/library/windows/apps/hh286408(v=vs.105).aspx)的新跨平台版本；若要了解有关应用文件资源管理器以及使用方法的详细信息，请参阅[这篇博客文章](https://blogs.windows.com/buildingapps/2016/06/08/using-the-app-file-explorer-to-see-your-app-data/)。 

![适用于移动设备的 Device Portal](images/device-portal/mob-device-portal-AppFileExplorer.png)

## 服务功能和说明

### DNS-SD

Device Portal 使用 DNS-SD 公布其存在于本地网络上。  所有 Device Portal 实例都将在“WDP._wdp._tcp.local”下公布，无论其设备类型如何。 服务实例的 TXT 记录提供以下信息：

项 | 类型 | 描述 
----|------|-------------
S | int | Device Portal 的安全端口。  如果为 0（零），则表示 Device Portal 不侦听 HTTPS 连接。 
D | string | 设备类型。  这将采用“Windows.*”格式，例如 Windows.Xbox 或 Windows.Desktop
A | string | 设备体系结构。  这将为 ARM、x86 或 AMD64。  
T | 字符串的 null 字符分隔列表 | 用户应用的设备标记。 有关其用法的信息，请参阅标记 REST API。 列表以双 null 结尾。  

推荐 HTTPS 端口上的连接，因为并非所有设备都侦听 DNS-SD 记录所公布的 HTTP 端口。 

### CSRF 保护和脚本

为了防止受到 [CSRF 攻击](https://wikipedia.org/wiki/Cross-site_request_forgery)，所有非 GET 请求上都需要唯一的令牌。 此令牌 X-CSRF-Token 请求标头派生自会话 Cookie CSRF-Token。 在 Device Portal Web UI 中，CSRF-Token Cookie 将复制到每个请求的 X-CSRF-Token 标头中。

**重要提示** 此保护可防止从独立客户端（例如命令行程序）使用 REST API。 这可以通过 3 种方法解决： 

1. 使用“auto-”用户名。 将“auto-”置于其用户名前面的客户端将绕过 CSRF 保护。 此用户名不能用于通过浏览器登录到 Device Portal，这一点很重要，因为它将针对 CSRF 攻击打开服务。 示例：如果 Device Portal 的用户名为“admin”，则 ```curl -u auto-admin:password <args>``` 应该用于绕过 CSRF 保护。 

2. 在客户端中实现 Cookie 到标头的方案。 这需要 GET 请求来建立会话 Cookie，并包含所有后续请求的标头和 Cookie。 
 
3. 禁用身份验证并使用 HTTP。 CSRF 保护仅适用于 HTTPS 终结点，因此 HTTP 终结点上的连接无需执行上述任一操作。 

**注意**：以“auto-”开头的用户名将无法通过浏览器登录到 Device Portal。  

#### 跨站点 WebSocket 劫持 (CSWSH) 保护

若要防止受到 [CSWSH 攻击](https://www.christian-schneider.net/CrossSiteWebSocketHijacking.html)，用于打开对 Device Portal 的 WebSocket 连接的所有客户端还必须提供与主机头匹配的 Origin 标头。  这向 Device Portal 证明了请求来自 Device Portal UI 或有效的客户端应用程序。  如果没有 Origin 标头，将拒绝你的请求。 



<!--HONumber=Aug16_HO5-->


