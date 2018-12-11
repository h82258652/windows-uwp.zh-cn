---
ms.assetid: 5c34c78e-9ff7-477b-87f6-a31367cd3f8b
title: 适用于 Windows 桌面的设备门户
description: 了解 Windows 设备门户如何在 Windows 桌面上打开诊断和自动化。
ms.date: 03/15/2018
ms.topic: article
keywords: windows 10，uwp，设备门户
ms.localizationpriority: medium
ms.openlocfilehash: 1be8dfd11e68dc8e6382f98e08e6c23f2a4d6be6
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2018
ms.locfileid: "8889423"
---
# <a name="device-portal-for-windows-desktop"></a>适用于 Windows 桌面的设备门户



Windows 设备门户允许你查看诊断信息，并通过 HTTP 从浏览器窗口与你的桌面进行交互。 你可以使用设备门户执行以下操作：
- 查看并操作正在运行的进程的列表
- 安装、删除、启动和终止应用
- 更改 WLAN 配置文件、查看信号强度 并查看 ipconfig
- 查看 CPU、内存、I/O、网络和 GPU 使用情况的实时图
- 收集进程转储
- 收集 ETW 跟踪 
- 操作旁加载应用的独立存储

## <a name="set-up-device-portal-on-windows-desktop"></a>在 Windows 桌面上设置设备门户

### <a name="turn-on-developer-mode"></a>打开开发人员模式

从 Windows 10 版本 1607 开始，适用于桌面的一些较新功能仅在启用开发人员模式时可用。 有关如何启用开发人员模式的信息，请参阅[启用设备进行开发](../get-started/enable-your-device-for-development.md)。

> [!IMPORTANT]
> 有时，由于网络或兼容性问题，开发人员模式无法在你的设备上正确安装。 有关解决这些问题的帮助，请参阅[启用设备进行开发的相关部分](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#failure-to-install-developer-mode-package)。

### <a name="turn-on-device-portal"></a>打开设备门户

您可以在**设置**的**面向开发人员**部分中启用设备门户。 当你启用它时，你还必须创建相应的用户名和密码。 不要使用你的 Microsoft 帐户或其他 Windows 凭据。 

![“设置”应用的“设备门户”部分](images/device-portal/device-portal-desk-settings.png) 

在启用设备门户后，你将在该部分的底部看到 Web 链接。 记下追加到所列 URL 末尾的端口号：此端口号在启用设备门户时随机生成，但应在桌面重启后保持一致。 如果你希望手动设置端口号以使它们永久保留，请参阅[设置端口号](device-portal-desktop.md#setting-port-numbers)。

这些链接提供了连接到设备门户的两种方法：通过本地网络（包括 VPN）或通过本地主机。

### <a name="connect-to-device-portal"></a>连接到设备门户

要通过本地主机进行连接，打开浏览器窗口，然后输入你正在使用的连接类型在此处显示的地址。

* Localhost：`http://127.0.0.1:<PORT>` 或 `http://localhost:<PORT>`
* 本地网络： `https://<IP address of the desktop>:<PORT>`

身份验证和安全通信要求使用 HTTPS。

如果在信任本地网络上的所有用户、设备上没有任何私人信息并且具有独特要求的受保护环境（例如测试实验室）中使用设备门户，你可以禁用“身份验证”选项。 这支持未加密的通信，并允许任何拥有你的计算机 IP 地址的用户连接到通信并控制它。

## <a name="device-portal-content-on-windows-desktop"></a>Windows 桌面上的设备门户内容

Windows 桌面上的设备门户提供了一组标准页面。 有关这些页面的详细说明，请参阅 [Windows 设备门户概述](device-portal.md)。

- 应用管理器
- 文件资源管理器
- 正在运行的进程
- 性能
- 调试
- Windows 事件跟踪 (ETW)
- 性能跟踪
- 设备管理器
- 网络
- 故障数据
- 功能
- 混合现实
- 流式安装调试程序
- 位置
- Scratch

## <a name="more-device-portal-options"></a>更多设备门户选项
### <a name="registry-based-configuration-for-device-portal"></a>设备门户的基于注册表的配置

如果你希望为设备门户选择端口号（如 80 和 443），你可以设置以下 RegKey：

- 在 `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\WebManagement\Service`
    - `UseDynamicPorts`：一个必需的 DWORD。 将其设置为 0，以便保留你已选择的端口号。
    - `HttpPort`：一个必需的 DWORD。 包含设备门户将在其上侦听 HTTP 连接的端口号。    
    - `HttpsPort`：一个必需的 DWORD。 包含设备门户将在其上侦听 HTTPS 连接的端口号。
    
在相同的 RegKey 路径下，你也可以关闭身份验证要求：
- `UseDefaultAuthorizer` - `0` 为禁用，`1` 为启用。  
    - 这可控制每个连接的基本身份验证要求以及从 HTTP 到 HTTPS 的重定向。  
    
### <a name="command-line-options-for-device-portal"></a>设备门户的命令行选项
通过管理命令提示符，你可以启用和配置设备门户的部件。 要查看你的版本上支持的最新命令集，你可以运行 `webmanagement /?`

- `sc start webmanagement` 或 `sc stop webmanagement` 
    - 打开或关闭该服务。 这仍需要启用开发人员模式。 
- `-Credentials <username> <password>` 
    - 设置设备门户的用户名和密码。 用户名必须符合基本身份验证标准，因此不得包含冒号 (:) 且应从标准 ASCII 字符（如 [a-zA-Z0-9]）中构建，因为浏览器不会以标准方式解析全字符集。  
- `-DeleteSSL` 
    - 这将重置用于 HTTPS 连接的 SSL 证书缓存。 如果你遇到无法避免的 TLS 连接错误（而不是预期的证书警告），此选项可为你解决该问题。 
- `-SetCert <pfxPath> <pfxPassword>`
    - 有关详细信息，请参阅[使用自定义的 SSL 证书预配设备门户](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-ssl)。  
    - 这将允许你安装自己的 SSL 证书来修复通常显示在设备门户中的 SSL 警告页面。 
- `-Debug <various options for authentication, port selection, and tracing level>`
    - 运行独立版本的具有特定配置和可见调试消息的设备门户。 这对于生成[打包的插件](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-plugin)最为有用。 
    - 有关如何作为系统运行此项以完全测试你的打包插件的详细信息，请参阅 [MSDN 杂志文章](https://msdn.microsoft.com/en-us/magazine/mt826332.aspx)。

## <a name="see-also"></a>另请参阅

* [Windows 设备门户概述](device-portal.md)
* [设备门户核心 API 参考](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-api-core)
