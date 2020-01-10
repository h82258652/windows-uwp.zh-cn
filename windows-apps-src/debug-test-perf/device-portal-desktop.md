---
ms.assetid: 5c34c78e-9ff7-477b-87f6-a31367cd3f8b
title: 适用于 Windows 桌面的设备门户
description: 了解 Windows 设备门户如何在 Windows 桌面上打开诊断和自动化。
ms.date: 02/06/2019
ms.topic: article
keywords: windows 10、uwp、设备门户
ms.localizationpriority: medium
ms.openlocfilehash: 73f7e827c0ec8ca289d3523da06601de978a91d2
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/06/2020
ms.locfileid: "75681968"
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

在启用设备门户后，你将在该部分的底部看到 Web 链接。 记下追加到所列 URL 末尾的端口号：此端口号在启用设备门户时随机生成，但应在桌面重启后保持一致。 

这些链接提供了连接到设备门户的两种方法：通过本地网络（包括 VPN）或通过本地主机。

### <a name="connect-to-device-portal"></a>连接到设备门户

要通过本地主机进行连接，打开浏览器窗口，然后输入你正在使用的连接类型在此处显示的地址。

* Localhost： `http://127.0.0.1:<PORT>` 或 `http://localhost:<PORT>`
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
- “设备管理器”，
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
    - `UseDynamicPorts`：必填的 DWORD。 将其设置为 0，以便保留你已选择的端口号。
    - `HttpPort`：必填的 DWORD。 包含设备门户将在其上侦听 HTTP 连接的端口号。    
    - `HttpsPort`：必填的 DWORD。 包含设备门户将在其上侦听 HTTPS 连接的端口号。
    
在相同的 RegKey 路径下，你也可以关闭身份验证要求：
- `UseDefaultAuthorizer`已禁用的  - `0`，`1` 启用。  
    - 这可控制每个连接的基本身份验证要求以及从 HTTP 到 HTTPS 的重定向。  
    
### <a name="command-line-options-for-device-portal"></a>设备门户的命令行选项
通过管理命令提示符，你可以启用和配置设备门户的部件。 若要查看生成上支持的最新命令集，可以运行 `webmanagement /?`

- `sc start webmanagement` 或 `sc stop webmanagement` 
    - 打开或关闭该服务。 这仍需要启用开发人员模式。 
- `-Credentials <username> <password>` 
    - 设置设备门户的用户名和密码。 用户名必须符合基本身份验证标准，因此不能包含冒号（:)和应使用标准 ASCII 字符（例如 [A-za-z0-9]）构建，因为浏览器不会以标准方式分析完整字符集。  
- `-DeleteSSL` 
    - 这将重置用于 HTTPS 连接的 SSL 证书缓存。 如果你遇到无法避免的 TLS 连接错误（而不是预期的证书警告），此选项可为你解决该问题。 
- `-SetCert <pfxPath> <pfxPassword>`
    - 有关详细信息，请参阅[使用自定义的 SSL 证书预配设备门户](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-ssl)。  
    - 这将允许你安装自己的 SSL 证书来修复通常显示在设备门户中的 SSL 警告页面。 
- `-Debug <various options for authentication, port selection, and tracing level>`
    - 运行独立版本的具有特定配置和可见调试消息的设备门户。 这对于生成[打包的插件](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-plugin)最为有用。 
    - 有关如何作为系统运行此项以完全测试你的打包插件的详细信息，请参阅 [MSDN 杂志文章](https://msdn.microsoft.com/magazine/mt826332.aspx)。

## <a name="common-errors-and-issues"></a>常见错误和问题

下面是设置设备门户时可能会遇到的一些常见错误。

### <a name="windowsupdatesearch-returns-invalid-number-of-updates-0x800f0950-cbs_e_invalid_windows_update_count"></a>WindowsUpdateSearch 返回的更新数无效（0x800f0950 CBS_E_INVALID_WINDOWS_UPDATE_COUNT）

尝试将开发人员包安装到 Windows 10 的预发行版本时，可能会收到此错误。 这些按需功能（FoD）包承载 Windows 更新上，并在预发布版本上下载它们要求你选择加入试验。 如果未选择将安装用于正确的生成和环形组合，则不会下载有效负载。 仔细检查以下各项：

1. 导航到 "**设置" > 更新 Windows 预览体验计划 & 安全 >** ，并确认 " **windows 预览体验帐户**" 部分包含正确的帐户信息。 如果看不到此部分，请选择 "**链接 Windows 预览体验帐户**"、"添加电子邮件帐户"，并确认该帐户显示在 " **Windows 预览体验帐户**" 标题下（可能需要选择 "第二次将**windows 预览体验帐户链接**到实际链接新添加的帐户"）。
 
2. 在 "**你想要接收何种类型的内容？** " 下，确保已选择 " **Windows 的活动开发**"。
 
3. 在 "**想要获取新版本的步调"** 下，确保已选择 " **Windows 有问必答 Fast** "。
 
4. 现在应能够安装 FoDs。 如果已确认你已在 Windows 有问必答上快速，但仍无法安装 FoDs，请提供反馈，并将日志文件附加到**C:\Windows\Logs\CBS**下。

### <a name="sc-startservice-openservice-failed-1060-the-specified-service-does-not-exist-as-an-installed-service"></a>SCStartService： OpenService 失败1060：指定的服务不是已安装的服务

如果开发人员包未安装，则可能会收到此错误。 如果没有开发人员包，则没有 web 管理服务。 请尝试再次安装开发人员包。

### <a name="cbs-cannot-start-download-because-the-system-is-on-metered-network-cbs_e_metered_network"></a>由于系统位于计量网络（CBS_E_METERED_NETWORK）上，因此无法开始下载

如果使用按流量计费的 internet 连接，则可能会收到此错误。 你将无法通过按流量计费的连接下载开发人员包。

## <a name="see-also"></a>另请参阅

* [Windows 设备门户概述](device-portal.md)
* [设备门户核心 API 参考](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-api-core)
