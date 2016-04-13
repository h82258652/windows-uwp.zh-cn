---
ms.assetid: 60fc48dd-91a9-4dd6-a116-9292a7c1f3be
title: Windows Device Portal 概述
description: 了解 Windows Device Portal 如何支持你通过网络或 USB 连接远程配置和管理你的设备。
---
# Windows Device Portal 概述

Windows Device Portal 可使你通过网络或 USB 连接远程配置和管理你的设备。 它还提供高级诊断工具，用于帮助你查看 Windows 设备的实时性能并对其进行疑难解答。 

Device Portal 是设备上的 Web 服务器，你可以从电脑上的 Web 浏览器连接到它。 如果你的设备具有 Web 浏览器，你还可以与设备上的浏览器本地连接。

Windows Device Portal 在每个设备系列上都可用，但功能和设置可能因设备的要求而异。 本文提供了 Device Portal 的常规说明以及指向包含每个设备系列的更具体信息的文章链接。

Windows Device Portal 中的所有内容都基于 [REST API](device-portal-api-core.md) 生成，该 API 可用于访问数据和以编程方式控制设备。

## 设置

每台设备都有有关连接到 Device Portal 的特定说明，但每台设备都需要以下常规步骤。
1. 在你的设备上启用开发人员模式和 Device Portal。
2. 通过本地网络或 USB 连接你的设备和电脑。
3. 在浏览器中导航到 Device Portal 页面。 此表显示了每个设备系列所使用的端口和协议。

设备系列 | 默认启用？ | HTTP | HTTPS | USB
--------------|----------------|------|-------|----
HoloLens | 是，处于开发人员模式下 | 80（默认值） | 443（默认值） | localhost:10080
IoT | 是，处于开发人员模式下 | 8080 | 通过注册表项启用 | 不适用
Xbox | 在开发人员模式内启用 | 已禁用 | 11443 | 不适用
桌面设备| 在开发人员模式内启用 | 随机 > 50,000 (xx080) | 随机 > 50,000 (xx443) | 不适用
电话 | 在开发人员模式内启用 | 80| 443 | localhost:10080

有关特定于设备的设置说明，请参阅：
- [适用于 HoloLens 的 Device Portal](https://dev.windows.com/holographic/using_the_windows_device_portal)
- [适用于 IoT 的 Device Portal](http://ms-iot.github.io/content/en-US/win10/tools/DevicePortal.htm)
- [适用于移动设备的 Device Portal](device-portal-mobile.md#setup)
- [适用于 Xbox 的 Device Portal](device-portal-xbox.md)

## 功能

### 工具栏和导航

页面顶部的工具栏提供了对常用状态和功能的访问权限。
- **关机**：关闭设备。
- **重新启动**：重新接通设备的电源。
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
2.  单击“浏览”并找到你的应用包 (.appx)。
3.  单击“浏览”并找到证书文件 (.cer)。 （并非在所有设备上都需要。）
4.  添加依赖项。 如果你有多个依赖项，请分别添加每一个。     
5.  在“部署”下，单击“转到”。 
6.  若要安装另一个应用，请单击“重置”按钮来清除字段。


**卸载应用**

1.  确保应用未在运行。 
2.  如果正在运行，请转到“正在运行的应用”并关闭它。 如果你尝试在应用正在运行时卸载，它将在尝试重新安装应用时导致问题。 
3.  准备就绪后，单击“卸载”。

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

### Windows 事件跟踪 (ETW)

管理设备上的实时 Windows 事件跟踪 (ETW)。

![适用于移动设备的 Device Portal](images/device-portal/mob-device-portal-etw.png)

选中“隐藏提供程序”以仅显示“事件”列表。
- **注册的提供程序**：选择 ETW 提供程序和跟踪级别。 跟踪级别是以下值之一：
    1. 异常退出或终止
    2. 严重错误
    3. 警告
    4. 非错误警告
    5. 详细跟踪 (*)

单击或点击“启用”以开始跟踪。 提供程序将添加到“已启用的提供程序”下拉列表。
- **自定义提供程序**：选择自定义 ETW 提供程序和跟踪级别。 根据其 GUDI 标识提供程序。 不要在 GUID 中包含括号。
- **已启用的提供程序**：列出已启用的提供程序。 从下拉列表中选择一个提供程序，然后单击或点击“禁用”来停止跟踪。 单击或点击“全部停止”来暂停所有跟踪。
- **提供程序历史记录**：显示已在当前会话期间启用的 ETW 提供程序。 单击或点击“启用”来激活已禁用的提供程序。 单击或点击“清除”来清除历史记录。
- **事件**：以表格的形式列出来自选定提供程序的 ETW 事件。 此表将实时更新。 在该表下方，单击“清除”按钮可删除表中的所有 ETW 事件。 这不会禁用任何提供程序。 你可以单击“保存到文件”来将当前收集的 ETW 事件本地导出到 CSV 文件。

### 性能跟踪

从设备中捕获 [Windows Performance Recorder](https://msdn.microsoft.com/library/windows/hardware/hh448205.aspx) (WPR) 跟踪。

![适用于移动设备的 Device Portal](images/device-portal/mob-device-portal-perf-tracing.png)

- **可用配置文件**：从下拉列表中选择 WPR 配置文件，然后单击或点击“开始”以开始跟踪。
- **自定义配置文件**：单击或点击“浏览”以从电脑中选择 WPR 配置文件。 单击或点击“上载并启动”以开始跟踪。

若要停止跟踪，请单击停止链接。 停留在此页面上，直到跟踪文件 (.ETL) 完成下载。

可以打开捕获的 ETL文件以供在 [Windows Performance Analyzer](https://msdn.microsoft.com/library/windows/hardware/hh448170.aspx) 中进行分析。

### 设备

枚举连接到你的设备的所有外围设备。

![适用于移动设备的 Device Portal](images/device-portal/mob-device-portal-devices.png)

### 网络

管理设备上的网络连接。  除非你通过 USB 连接到 Device Portal，否则更改这些设置很可能使你与 Device Portal 断开连接。 
- **配置文件**：选择其他 WLAN 配置文件以供使用。  
- **可用网络**：可用于该设备的 WLAN 网络。  单击或点击某个网络将允许你连接到该网络，并提供密钥（如果需要）。  注意：Device Portal 尚不支持企业身份验证。 

![适用于移动设备的 Device Portal](images/device-portal/mob-device-portal-network.png)



<!--HONumber=Mar16_HO5-->


