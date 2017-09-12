---
author: PatrickFarley
ms.assetid: 5c34c78e-9ff7-477b-87f6-a31367cd3f8b
title: "适用于桌面设备的 Device Portal"
description: "了解 Windows Device Portal 如何在 Windows 桌面上打开诊断和自动化。"
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 32155bfbb676a5f79dd4b1629f0a88368da36828
ms.sourcegitcommit: 0fa9ae00117e8e6b04ed38956e605bb74c1261c6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/07/2017
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

## <a name="failure-to-install-developer-mode-package"></a>无法安装开发人员模式程序包。
有时，由于网络或兼容性问题，开发人员模式无法正确安装。 开发人员模式程序包是**远程**部署到电脑的必需条件 - 使用来自浏览器的 Device Portal 或设备发现启用 SSH - 但不用于本地部署。  即使你遇到这些问题，你仍然可以使用 Visual Studio 本地部署应用。 

请参阅[已知问题](https://social.msdn.microsoft.com/Forums/en-US/home?forum=Win10SDKToolsIssues&sort=relevancedesc&brandIgnore=True&searchTerm=%22device+portal%22)论坛和[开发人员模式页](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#failure-to-install-developer-mode-package)，查找这些问题的解决方法以及其他内容。 

