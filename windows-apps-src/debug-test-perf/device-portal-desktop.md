---
ms.assetid: 5c34c78e-9ff7-477b-87f6-a31367cd3f8b
title: 适用于 Windows 桌面的设备门户
description: 了解 Windows 设备门户如何在 Windows 桌面上打开诊断和自动化。
ms.date: 2/6/2019
ms.topic: article
keywords: windows 10，uwp，设备门户
ms.localizationpriority: medium
ms.openlocfilehash: 3dcf35a1bd43930e616edc6d1e7180c9cea31560
ms.sourcegitcommit: b79cc7e0eac414ac2275517a7f56d1f9a817d112
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/06/2019
ms.locfileid: "9060041"
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

## <a name="common-errors-and-issues"></a>常见错误和问题

下面是设置 Device Portal 时可能遇到的一些常见错误。

### <a name="windowsupdatesearch-returns-invalid-number-of-updates-0x800f0950-cbseinvalidwindowsupdatecount"></a>WindowsUpdateSearch 返回无效的更新数量 (0x800f0950 CBS_E_INVALID_WINDOWS_UPDATE_COUNT)

尝试在 Windows 10 的预发布版本上安装的开发人员程序包时，你可能会收到此错误。 Windows 更新上承载这些功能按需 (FoD) 包和下载这些预发行版本上需要你选择到外部测试版。 如果你安装参与到外部测试版的正确版本和圈组合，有效负载不会可下载。 仔细检查以下各项：

1. 导航到**设置 > 更新 & 安全 > Windows 预览体验计划**，并确认**Windows 预览体验成员帐户**部分具有正确的帐户信息。 如果你没有看到该部分中，选择**Windows 预览体验成员帐户链接**，添加你的电子邮件帐户，并确认，它将显示的标题下的**Windows 预览体验成员帐户**（你可能需要选择**Windows 预览体验成员帐户链接**到第二次实际新添加的帐户链接）。
 
2. 在**要接收的内容类型？**，请确保选择**活动开发状态的 Windows** 。
 
3. 在**你想要获取新版本的节奏？**，请确保**Windows Insider Fast**处于选中状态。
 
4. 你现在应该能够安装 Fod。 如果你已确认你是在 Windows 预览体验成员快速并仍无法安装 Fod，请提供反馈，并附加**C:\Windows\Logs\CBS**下的日志文件。

### <a name="sc-startservice-openservice-failed-1060-the-specified-service-does-not-exist-as-an-installed-service"></a>[SC]StartService: OpenService 失败 1060年： 指定的服务不存在作为安装服务

如果未安装开发人员程序包，你可能会收到此错误。 而无需开发人员程序包中，没有任何 web 管理服务。 请尝试再次安装开发人员程序包。

### <a name="cbs-cannot-start-download-because-the-system-is-on-metered-network-cbsemeterednetwork"></a>Cb 获取服务无法启动下载，因为系统位于按流量计费的网络 (CBS_E_METERED_NETWORK)

如果你使用的是按流量计费的 internet 连接，你可能会收到此错误。 你将无法下载按流量计费的连接上的开发人员程序包。

## <a name="see-also"></a>另请参阅

* [Windows 设备门户概述](device-portal.md)
* [设备门户核心 API 参考](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-api-core)
