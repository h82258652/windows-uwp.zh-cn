---
ms.assetid: 60fc48dd-91a9-4dd6-a116-9292a7c1f3be
title: Windows 设备门户概述
description: 了解 Windows Device Portal 如何支持你通过网络或 USB 连接远程配置和管理你的设备。
ms.date: 04/09/2019
ms.topic: article
keywords: windows 10, uwp, device portal
ms.localizationpriority: medium
ms.openlocfilehash: 2292d97166d34905bb895aa3f53f864510a21f46
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2019
ms.locfileid: "74254765"
---
# <a name="windows-device-portal-overview"></a>Windows 设备门户概述

Windows Device Portal 可使你通过网络或 USB 连接远程配置和管理你的设备。 It also provides advanced diagnostic tools to help you troubleshoot and view the real-time performance of your Windows device.

Windows Device Portal is a web server on your device that you can connect to from a web browser on a PC. If your device has a web browser, you can also connect locally with the browser on that device.

Windows Device Portal is available on each device family, but features and setup vary based on each device's requirements. 本文提供了 Device Portal 的常规说明以及指向包含每个设备系列的更具体信息的文章链接。

The functionality of the Windows Device Portal is implemented with [REST APIs](device-portal-api-core.md) that you can use directly to access data and control your device programmatically.

## <a name="setup"></a>“安装程序”

每台设备都有有关连接到 Device Portal 的特定说明，但每台设备都需要以下常规步骤：

1. Enable Developer Mode and Device Portal on your device (configured in the Settings app).

2. Connect your device and PC through a local network or with USB.

3. 在浏览器中导航到 Device Portal 页面。 This table shows the ports and protocols used by each device family.

设备系列 | 默认启用？ | HTTP | HTTPS | “USB”
--------------|----------------|------|-------|----
HoloLens | 是，处于开发人员模式下 | 80（默认值） | 443（默认值） | http://127.0.0.1:10080
IoT | 是，处于开发人员模式下 | 8080 | 通过注册表项启用 | N/A
Xbox | 在开发人员模式内启用 | 禁用 | 11443 | N/A
桌面| 在开发人员模式内启用 | 50080\* | 50043\* | N/A
Phone | 在开发人员模式内启用 | 80| 443 | http://127.0.0.1:10080

\* This is not always the case, as Device Portal on desktop claims ports in the ephemeral range (>50,000) to prevent collisions with existing port claims on the device. 若要了解详细信息，请参阅适用于桌面的[端口设置](device-portal-desktop.md#registry-based-configuration-for-device-portal)部分。  

有关特定于设备的设置说明，请参阅：

- [适用于 HoloLens 的设备门户](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-hololens)
- [适用于 IoT 的设备门户](https://docs.microsoft.com/windows/iot-core/manage-your-device/DevicePortal)
- [适用于移动设备的设备门户](device-portal-mobile.md)
- [适用于 Xbox 的设备门户](../xbox-apps/device-portal-xbox.md)
- [适用于台式机的设备门户](device-portal-desktop.md#set-up-device-portal-on-windows-desktop)

## <a name="features"></a>功能

### <a name="toolbar-and-navigation"></a>工具栏和导航

The toolbar at the top of the page provides access to commonly used features.

- **Power**: Access power options.
  - **关机**：关闭设备。
  - **重启**：重新接通设备的电源。
- **帮助**：打开帮助页面。

使用沿页面左侧的导航窗格中的链接导航到适用于你的设备的可用管理和监视工具。

Tools that are common across device families are described here. 根据设备的不同可能提供其他选项。 For more info, see the specific page for your device type.

### <a name="apps-manager"></a>应用管理器

The Apps manager provides install/uninstall and management functionality for app packages and bundles on the host device.

![Device Portal Apps manager page](images/device-portal/WDP_AppsManager2.png)

* **Deploy apps**: Deploy packaged apps from local, network, or web hosts and register loose files from network shares.
* **Installed apps**: Use the dropdown menu to remove or start apps that are installed on the device.
* **Running apps**: Get information about the apps that are currently running and close them as necessary.

#### <a name="install-sideload-an-app"></a>Install (sideload) an app

You can sideload apps during development using Windows Device Portal:

1. When you've created an app package, you can remotely install it onto your device. 在 Visual Studio 中生成它后，将生成一个输出文件夹。

    ![应用安装](images/device-portal/iot-installapp0.png)

2. In Windows Device Portal, navigate to the **Apps manager** page.

3. In the **Deploy apps** section, select **Local Storage**.

4. Under **Select the application package**, select **Choose File** and browse to the app package that you want to sideload.

5. Under **Select certificate file (.cer) used to sign app package**, select **Choose File** and browse to the certificate associated with that app package.

6. Check the respective boxes if you want to install optional or framework packages along with the app installation, and select **Next** to choose them.

7. Select **Install** to initiate the installation.

8. If the device is running Windows 10 in S mode, and it is the first time that the given certificate has been installed on the device, restart the device.

#### <a name="install-a-certificate"></a>Install a certificate

Alternatively, you can install the certificate via Windows Device Portal, and install the app through other means:

1. In Windows Device Portal, navigate to the **Apps manager** page.

2. In the **Deploy apps** section, select **Install Certificate**.

3. Under **Select certificate file (.cer) used to sign an app package**, select **Choose File** and browse to the certificate associated with the app package that you want to sideload.

4. Select **Install** to initiate the installation.

5. If the device is running Windows 10 in S mode, and it is the first time that the given certificate has been installed on the device, restart the device.

#### <a name="uninstall-an-app"></a>卸载应用

1. 确保应用未在运行。
2. If it is, go to **Running apps** and close it. If you attempt to uninstall while the app is running, it will cause issues when you attempt to reinstall the app.
3. Select the app from the dropdown and click **Remove**.

### <a name="running-processes"></a>Running processes

This page shows details about processes currently running on the host device. 这包括应用和系统进程。 On some platforms (Desktop, IoT, and HoloLens), you can terminate processes.

![Device Portal Running processes page](images/device-portal/mob-device-portal-processes.png)

### <a name="file-explorer"></a>文件资源管理器

This page allows you to view and manipulate files stored by any sideloaded apps. See the [Using the App File Explorer](https://blogs.windows.com/buildingapps/2016/06/08/using-the-app-file-explorer-to-see-your-app-data/) blog post to learn more about the File explorer and how to use it.

![Device Portal File explorer page](images/device-portal/mob-device-portal-AppFileExplorer.png)

### <a name="performance"></a>性能

The Performance page shows real-time graphs of system diagnostic info like power usage, frame rate, and CPU load.

可用的指标如下所示：

- **CPU**: Percent of total available CPU utilization
- **Memory**: Total, in use, available, committed, paged, and non-paged
- **I/O**: Read and write data quantities
- **Network**: Received and sent data
- **GPU**: Percent of total available GPU engine utilization

![Device Portal Performance page](images/device-portal/mob-device-portal-perf.png)

### <a name="event-tracing-for-windows-etw-logging"></a>Event Tracing for Windows (ETW) logging

The ETW logging page manages real-time Event Tracing for Windows (ETW) information on the device.

![Device Portal ETW logging page](images/device-portal/mob-device-portal-etw.png)

选中**隐藏提供程序**以仅显示“事件”列表。

- **Registered providers**: Select the event provider and the tracing level. The tracing level is one of these values:
  1. 异常退出或终止
  2. 严重错误
  3. 警告
  4. 非错误警告
  5. Detailed trace

  单击或点击**启用**以开始跟踪。 提供程序将添加到**已启用的提供程序**下拉列表。
- **自定义提供程序**：选择自定义 ETW 提供程序和跟踪级别。 根据其 GUDI 标识提供程序。 Do not include brackets in the GUID.
- **Enabled providers**: This lists the enabled providers. 从下拉列表中选择一个提供程序，然后单击或点击**禁用**来停止跟踪。 单击或点击**全部停止**来暂停所有跟踪。
- **Providers history**: This shows the ETW providers that were enabled during the current session. 单击或点击**启用**来激活已禁用的提供程序。 单击或点击**清除**来清除历史记录。
- **Filters / Events**: The **Events** section lists ETW events from the selected providers in table format. The table is updated in real time. Use the **Filters** menu to set up custom filters for which events will be displayed. Click the **Clear** button to delete all ETW events from the table. 这不会禁用任何提供程序。 You can click **Save to file** to export the currently collected ETW events to a local CSV file.

For more details on using ETW logging, see the [Use Device Portal to view debug logs](https://blogs.windows.com/buildingapps/2016/06/10/using-device-portal-to-view-debug-logs-for-uwp/) blog post.

### <a name="performance-tracing"></a>性能跟踪

The Performance tracing page allows you for view the [Windows Performance Recorder (WPR)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh448205(v=win.10)) traces from the host device.

![Device Portal performance tracing page](images/device-portal/mob-device-portal-perf-tracing.png)

- **可用配置文件**：从下拉列表中选择 WPR 配置文件，然后单击或点击**开始**以开始跟踪。
- **自定义配置文件**：单击或点击**浏览**以从电脑中选择 WPR 配置文件。 单击或点击**上载并启动**以开始跟踪。

若要停止跟踪，请单击**停止**。 Stay on this page until the trace file (.ETL) has finished downloading.

Captured .ETL files can be opened for analysis in the [Windows Performance Analyzer](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh448170(v=win.10)).

### <a name="device-manager"></a>“设备管理器”，

The Device manager page enumerates all peripherals attached to your device. You can click the settings icons to view the properties of each.

![Device Portal Device manager page](images/device-portal/mob-device-portal-devices.png)

### <a name="networking"></a>网络

The Networking page manages network connections on the device. Unless you are connected to Device Portal through USB, changing these settings will likely disconnect you from Device Portal.

- **Available networks**: Shows the WiFi networks available to the device. 单击或点击某个网络将允许你连接到该网络，并提供密钥（如果需要）。 Device Portal does not yet support Enterprise Authentication. You can also use the **Profiles** dropdown to attempt to connect to any of the WiFi profiles known to the device.
- **IP configuration**: Shows address information about each of the host device's network ports.

![Device Portal Networking page](images/device-portal/mob-device-portal-network.png)

## <a name="service-features-and-notes"></a>Service features and notes

### <a name="dns-sd"></a>DNS-SD

Device Portal 使用 DNS-SD 公布其存在于本地网络上。 所有 Device Portal 实例都将在“WDP._wdp._tcp.local”下公布，无论其设备类型如何。 服务实例的 TXT 记录提供以下信息：

密钥 | 在任务栏的搜索框中键入 | 描述
----|------|-------------
S | int | Device Portal 的安全端口。 如果为 0（零），则表示 Device Portal 不侦听 HTTPS 连接。
D | 字符串 | 设备类型。 This will be in the format "Windows.*", for example, Windows.Xbox or Windows.Desktop
一个 | 字符串 | 设备体系结构。 这将为 ARM、x86 或 AMD64。  
T | 字符串的 null 字符分隔列表 | 用户应用的设备标记。 有关其用法的信息，请参阅标记 REST API。 列表以双 null 结尾。  

推荐 HTTPS 端口上的连接，因为并非所有设备都侦听 DNS-SD 记录所公布的 HTTP 端口。

### <a name="csrf-protection-and-scripting"></a>CSRF 保护和脚本

为了防止受到 [CSRF 攻击](https://en.wikipedia.org/wiki/Cross-site_request_forgery)，所有非 GET 请求上都需要唯一的令牌。 此令牌 X-CSRF-Token 请求标头派生自会话 Cookie CSRF-Token。 在 Device Portal Web UI 中，CSRF-Token Cookie 将复制到每个请求的 X-CSRF-Token 标头中。

> [!IMPORTANT]
> This protection prevents usages of the REST APIs from a standalone client (such as command-line utilities). 这可以通过 3 种方法解决：
> - Use an "auto-" username. 将“auto-”置于其用户名前面的客户端将绕过 CSRF 保护。 此用户名不能用于通过浏览器登录到 Device Portal，这一点很重要，因为它将针对 CSRF 攻击打开服务。 示例：如果 Device Portal 的用户名为“admin”，则 ```curl -u auto-admin:password <args>``` 应该用于绕过 CSRF 保护。
> - 在客户端中实现 Cookie 到标头的方案。 这需要 GET 请求来建立会话 Cookie，并包含所有后续请求的标头和 Cookie。
> - 禁用身份验证并使用 HTTP。 CSRF 保护仅适用于 HTTPS 终结点，因此 HTTP 终结点上的连接无需执行上述任一操作。

#### <a name="cross-site-websocket-hijacking-cswsh-protection"></a>跨站点 WebSocket 劫持 (CSWSH) 保护

若要防止受到 [CSWSH 攻击](https://www.christian-schneider.net/CrossSiteWebSocketHijacking.html)，用于打开对 Device Portal 的 WebSocket 连接的所有客户端还必须提供与主机头匹配的 Origin 标头。 这向 Device Portal 证明了请求来自 Device Portal UI 或有效的客户端应用程序。 如果没有 Origin 标头，将拒绝你的请求。
