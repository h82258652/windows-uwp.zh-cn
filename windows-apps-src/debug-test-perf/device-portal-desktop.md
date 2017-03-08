---
author: mcleblanc
ms.assetid: 5c34c78e-9ff7-477b-87f6-a31367cd3f8b
title: "适用于桌面设备的 Device Portal"
description: "了解 Windows Device Portal 如何在 Windows 桌面上打开诊断和自动化。"
ms.author: markl
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 7b8b396078d59cc2ab3180e9af8b6017fd5edbda
ms.lasthandoff: 02/07/2017

---
# <a name="device-portal-for-desktop"></a>适用于桌面设备的 Device Portal

从 Windows 10 版本 1607 开始，附加的开发人员功能可用于桌面设备。 这些功能仅在启用了“开发人员模式”时才可用。

有关如何启用“开发人员模式”的信息，请参阅[启用设备进行开发](../get-started/enable-your-device-for-development.md)。

Device Portal 允许你查看诊断信息，并通过 HTTP 从浏览器与你的桌面进行交互。 你可以使用 Device Portal 执行以下操作：
- 查看并操作正在运行的进程的列表
- 安装、删除、启动和终止应用
- 更改 WLAN 配置文件、查看信号强度 并查看 ipconfig
- 查看 CPU、内存、I/O、网络和 GPU 使用情况的实时图
- 收集进程转储
- 收集 ETW 跟踪 
- 操作旁加载应用的独立存储

## <a name="set-up-device-portal-on-windows-desktop"></a>在 Windows 桌面上设置 Device Portal

### <a name="turn-on-device-portal"></a>打开 Device Portal

在“开发人员设置”**** 菜单中，在启用开发人员模式的情况下，你可以启用 Device Portal。  

当你启用 Device Portal 时，你还必须为 Device Portal 创建用户名和密码。 不要使用你的 Microsoft 帐户或其他 Windows 凭据。  

在启用 Device Portal 后，你将在“设置”****部分底部看到指向它的链接。 记下应用于 URL末尾的端口号：此端口号在启用 Device Portal 时随机生成，但应在桌面重启后保持一致。 如果你希望手动设置端口号以使它们永久保留，请参阅[设置端口号](device-portal-desktop.md#setting-port-numbers)。

你可以从以下两种方法中进行选择来连接到 Device Portal：本地主机和通过本地网络（包括 VPN）。

**连接到 Device Portal**

1. 在浏览器中，针对使用的连接类型输入地址，如下所示。

    - Localhost：`http://127.0.0.1:PORT` 或 `http://localhost:PORT`

    使用此地址在本地查看 Device Portal。
    
    - 本地网络： `https://<The IP address of the desktop>:PORT`

    使用此地址通过本地网络进行连接。

身份验证和安全通信要求使用 HTTPS。

如果在信任本地网络上的所有用户、设备上没有任何私人信息并且具有独特要求的受保护环境（例如测试实验室）中使用 Device Portal，你可以禁用身份验证。 这支持未加密的通信，并允许任何拥有你的计算机 IP 地址的用户控制通信。

## <a name="device-portal-pages"></a>Device Portal 页面

桌面上的 Device Portal 提供了一组标准页面。 有关详细说明，请参阅 [Windows Device Portal 概述](device-portal.md)。

- 应用
- 进程
- 性能
- 调试
- Windows 事件跟踪 (ETW)
- 性能跟踪
- 设备
- 网络
- 应用文件资源管理器 

## <a name="setting-port-numbers"></a>设置端口号

如果你希望为 Device Portal 选择端口号（如 80 和 443），你可以设置以下 RegKey：

- 在 HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\WebManagement\Service 下
    - UseDynamicPorts：一个必需的 DWORD。 将其设置为 0，以便保留你已选择的端口号。
    - HttpsPort：一个必需的 DWORD。 包含 Device Portal 将在其上侦听 HTTP 连接的端口号。  
    - HttpsPort：一个必需的 DWORD。 包含 Device Portal 将在其上侦听 HTTPS 连接的端口号。

## <a name="failure-to-install-developer-mode-package-or-launch-device-portal"></a>无法安装开发人员程序包或启动 Device Portal
有时，由于网络或兼容性问题，开发人员模式无法正确安装。 开发人员模式程序包对**远程**部署是必需的（Device Portal 和 SSH），但对本地部署并非如此。  即使你遇到这些问题，你仍然可以使用 Visual Studio 本地部署应用。 

请参阅[已知问题](https://social.msdn.microsoft.com/Forums/en-US/home?forum=Win10SDKToolsIssues&sort=relevancedesc&brandIgnore=True&searchTerm=%22device+portal%22)论坛，查找这些问题的解决方法以及其他内容。 

### <a name="failed-to-locate-the-package"></a>无法找到该程序包

“无法在 Windows 更新中找到开发人员模式程序包。 错误代码 0x001234 了解详细信息”   

发生此错误可能是由于网络连接问题、企业设置，或者程序包可能丢失。 

若要解决此问题：

1. 确保你的计算机连接到 Internet。 
2. 如果你位于加入域的计算机上，请与网络管理员联系。 
3. 在“设置”&gt;“更新和安全”&gt;“Windows 更新”中检查 Windows 更新。
4. 在“设置”&gt;“系统”&gt;“应用和功能”&gt;“管理可选功能”&gt;“添加功能”中验证 Windows 开发人员模式是否存在。 如果缺少，Windows 无法为计算机找到正确的程序包。 

在执行上述任意步骤后，禁用并随后重新启用“开发人员模式”以验证是否解决该问题。 


### <a name="failed-to-install-the-package"></a>无法安装程序包

“开发人员模式程序包无法安装。 错误代码 0x001234 了解详细信息”

发生此错误可能是由于 Windows 的内部版本和开发人员模式程序包之间不兼容。 

若要解决此问题：

1. 在“设置”&gt;“更新和安全”&gt;“Windows 更新”中检查 Windows 更新。
2. 重启计算机以确保所有更新都已应用。

