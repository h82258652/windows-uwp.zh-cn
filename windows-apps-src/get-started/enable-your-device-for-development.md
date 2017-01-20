---
author: GrantMeStrength
ms.assetid: 54973C62-9669-4988-934E-9273FB0425FD
title: "启用设备进行开发"
description: "配置 Windows 10 设备进行开发和调试。"
keywords: "入门 开发人员许可证 Visual Studio，开发人员许可证 启用设备"
ms.author: jken
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: 7a6f0be15105bc70e580eaaf581152338c56bed7
ms.openlocfilehash: 3641fdd1f7eb74a11233115d9dfb809ea959926e

---
# <a name="enable-your-device-for-development"></a>启用设备进行开发

在可以编写应用之前，需要在你的开发电脑上以及要用于测试代码的任何设备上启用开发人员模式。

## <a name="use-developer-features"></a>使用开发人员功能

### <a name="develop-your-app-with-microsoft-visual-studio"></a>使用 Microsoft Visual Studio 开发应用

必须先在你的电脑上启用开发人员模式，然后才可以在 Visual Studio 中打开 UWP 应用项目。 如果打开 UWP 项目，但未启用开发人员模式，“面向开发人员”设置页会自动打开。 按照下一部分中的说明操作，启用开发人员模式。

当在 Windows 10 版本 1511 或更早版本中的 Visual Studio 中打开 UWP 应用项目时，会在 Visual Studio 中看到此对话框。 

![启用在 Visual Studio 中显示的开发人员模式对话框](images/latestenabledialog.png)

当看到此对话框时，请单击“开发人员设置”打开“面向开发人员”设置页，然后启用开发人员模式。

> 可以随时转到“面向开发人员”页面启用或禁用开发人员模式：只需在任务栏中的 Cortana 搜索框中输入“开发人员设置”。

### <a name="enable-your-windows-10-devices"></a>启用 Windows 10 设备

你可以启用设备以进行开发，或仅进行旁加载。

-   *旁加载*是指安装未经 Windows 应用商店认证的应用，然后对其进行运行或测试的过程。 例如，仅限公司内部使用的应用。
-   借助*开发人员模式*，你不仅可以旁加载应用，还可以在调试模式下从 Visual Studio 运行应用。 

    启用开发人员模式时，会安装选项包，其中包括：
    - 安装 Windows Device Portal。 仅当“启用 Device Portal”选项打开时，才会启用 Device Portal，并为它配置防火墙规则。
    - 安装、启用允许远程安装应用的 SSH 服务，并为其配置防火墙规则。
    - （仅限桌面设备）允许启用适用于 Linux 的 Windows 子系统。 有关详细信息，请参阅[关于装有 Ubuntu 的 Windows 上的 Bash](https://msdn.microsoft.com/commandline/wsl/about)。

有关选项的更多详细信息，请参阅[应选择哪些设置：旁加载应用还是开发人员模式？](https://msdn.microsoft.com/en-us/windows/uwp/get-started/enable-your-device-for-development#which-settings-should-i-choose-sideload-apps-or-developer-mode)

**使用开发人员功能**

1.  在要启用的设备上，转到“设置”。 依次选择“更新和安全”和“对于开发人员”。
2.  选择所需的访问级别 - 若要开发 UWP 应用，请选择“开发人员模式”。 
3.  阅读所选设置的免责声明，然后单击“是”以接受更改。

> [!NOTE]
> 如果你的设备为组织所有，你的组织可能会禁用某些选项，如以下所示。

下面是桌面设备系列的设置页。

![转到“设置”、选择“更新和安全”，然后选择“面向开发人员”来查看你的选项](images/devmode-pc-options.png)

下面是移动设备系列的设置页。

![在手机上的“设置”中，选择“更新和安全”](images/devmode-mob.png)

## <a name="developer-mode-features"></a>开发人员模式功能

对于每个设备系列，可能会提供其他开发人员功能。 仅当在设备上启用了开发人员模式时这些功能才可用，并且可能会因操作系统版本的不同而有所不同。

此图显示了适用于版本 1511 Windows 10 移动设备系列的开发人员功能。

![适用于移动设备的开发人员模式选项](images/devmode-mob-options.png) 

### <a name="span-iddevice-discovery-and-pairingspandevice-portal"></a><span id="device-discovery-and-pairing"></span>Device Portal

若要了解有关设备发现和 Device Portal 的详细信息，请参阅 [Windows Device Portal 概述](../debug-test-perf/device-portal.md)。

有关特定于设备的设置说明，请参阅：
- [适用于桌面设备的 Device Portal](https://msdn.microsoft.com/windows/uwp/debug-test-perf/device-portal-desktop)
- [适用于 HoloLens 的 Device Portal](https://dev.windows.com/holographic/using_the_windows_device_portal)
- [适用于 IoT 的 Device Portal](https://developer.microsoft.com/windows/iot/docs/DevicePortal)
- [适用于移动设备的 Device Portal](../debug-test-perf/device-portal-mobile.md)
- [适用于 Xbox 的 Device Portal](../debug-test-perf/device-portal-xbox.md)

如果启用开发人员模式或 Device Portal 时遇到问题，请参阅[已知问题](https://social.msdn.microsoft.com/Forums/en-US/home?forum=Win10SDKToolsIssues&sort=relevancedesc&brandIgnore=True&searchTerm=%22device+portal%22)论坛查找这些问题的解决方法。 

###<a name="ssh"></a>SSH

在你的设备上启用开发人员模式时，会启用 SSH 服务。  当你的设备成为 UWP 应用程序的部署目标时，会使用该服务。   服务名称为“SSH Server Broker”和“SSH Server Proxy”。

> [!NOTE]
> 这不是 Microsoft 的 OpenSSH 实现，可以在 [GitHub](https://github.com/PowerShell/Win32-OpenSSH) 上找到该实现。

为了充分利用 SSH 服务，可以启用设备发现来支持固定配对。 如果想要运行另一个 SSH 服务，可以在其他端口上设置该服务或关闭开发人员模式 SSH 服务。 若要关闭 SSH 服务，只需禁用开发人员模式即可。  

### <a name="device-discovery"></a>设备发现

启用设备发现时，将允许你的设备通过 mDNS 对于网络上的其他设备可见。  此功能还允许你获取与此设备配对的 SSH 固定。  

![固定配对](images/devmode-pc-pinpair.PNG)

仅当想要使设备成为部署目标时，才应该启用设备发现。 例如，如果使用 Device Portal 将应用部署到手机进行测试，只需要在手机上启用设备发现，并不需要在开发电脑上启用。

### <a name="error-reporting-mobile-only"></a>错误报告（仅限移动设备）

设置此值以指定在手机上保存的故障转储量。

通过收集手机上的故障转储，你可以在故障发生后直接快速访问重要的故障信息。 仅为开发人员签名的应用收集转储。 你可以在“Documents\\Debug folder”文件夹的手机存储中查找转储。 有关转储文件的详细信息，请参阅[使用转储文件](https://msdn.microsoft.com/library/d5zhxt22.aspx)。

### <a name="optimizations-for-windows-explorer-remote-desktop-and-powershell-desktop-only"></a>适用于 Windows 资源管理器、远程桌面和 PowerShell 的优化（仅限桌面设备）

 在桌面设备系列上，“面向开发人员”设置页提供可用于针对开发任务对电脑进行优化的设置的快捷方式。 对于每个设置，可以选中相应复选框，然后单击“应用”或单击“显示设置”链接打开该选项的设置页。 

## <a name="which-settings-should-i-choose-sideload-apps-or-developer-mode"></a>应选择哪些设置：旁加载应用还是开发人员模式？

默认情况下，你只能从 Windows 应用商店安装通用 Windows 平台 (UWP) 应用。 将这些设置更改为使用开发人员功能时可能需更改设备的安全级别。 不应从未经验证的源安装应用。

### <a name="sideload-apps"></a>旁加载应用

旁加载应用设置通常由需要在未通过 Windows 应用商店认证的托管设备上安装自定义应用的公司或学校使用。 在此情况下，组织通常会强制执行禁用 *Windows 应用商店应用*设置的策略，如之前的设置页图像中所示。 组织还会提供旁加载应用所需的证书和安装位置。 有关详细信息，请参阅 TechNet 文章[在 Windows 10 中旁加载应用](https://technet.microsoft.com/library/mt269549.aspx)和 [Microsoft Intune 中的应用部署入门](https://technet.microsoft.com/library/dn646955.aspx)。

设备系列特定信息

-   对于桌面设备系列：你可以通过运行使用程序包（“Add-AppDevPackage.ps1”）创建的 Windows PowerShell 脚本，来安装应用包 (.appx) 和运行应用所需的任何证书。 有关详细信息，请参阅[打包 UWP 应用](../packaging/packaging-uwp-apps.md)。

-   对于移动设备系列：如果已安装了必需的证书，你可以点击文件以安装任何通过电子邮件收到的或 SD 卡上的 .appx。

**旁加载应用**是比开发人员模式更安全的选项，因为你无法在缺少可信任证书的设备上安装应用。

> [!NOTE]
> 如果旁加载应用，你仍然应该仅从受信任的源安装应用。 安装未经 Windows 应用商店认证的旁加载应用时，即表明你同意已获取旁加载应用所需的所有权限，并且你对任何由安装和运行应用引发的损害负全责。 请参阅 Windows &gt; 此[隐私声明](http://go.microsoft.com/fwlink/?LinkId=521839)的 Windows 应用商店部分。

### <a name="developer-mode"></a>开发人员模式

开发人员模式将替换 Windows 8.1 对于开发人员许可证的要求。  除了旁加载外，开发人员模式设置还支持调试和其他部署选项。 这包括启动 SSH 服务允许部署该设备。 为了停止运行此服务，必须禁用开发人员模式。

设备系列特定信息

-   对于桌面设备系列：

    启用开发人员模式以在 Visual Studio 开发和调试应用。 正如前面所述，如果未启用开发人员模式，你将在 Visual Studio 中收到提示。

    允许启用适用于 Linux 的 Windows 子系统。 有关详细信息，请参阅[关于装有 Ubuntu 的 Windows 上的 Bash](https://msdn.microsoft.com/commandline/wsl/about)。

-   对于移动设备系列：

    启用开发人员模式以从 Visual Studio 部署应用并在设备上调试它们。

    你可以点击文件以安装通过电子邮件收到的或 SD 卡上的任何 .appx。 请不要从未经验证的源安装应用。

**提示**  
有多个工具可用来将应用从 Windows 10 电脑部署到 Windows 10 移动设备。 两台设备均必须通过有线或无线的连接方式连接到网络的同一子网，或者它们必须通过 USB 进行连接。 所列的任意一种方法仅会安装应用包 (.appx)，不安装证书。

-   使用 Windows 10 应用程序部署 (WinAppDeployCmd) 工具。 了解有关 [WinAppDeployCmd 工具](http://msdn.microsoft.com/library/windows/apps/mt203806.aspx)的详细信息。
-   从 Windows 10 版本 1511 开始，你可以使用 [Device Portal](#device_portal) 从浏览器部署到运行 Windows 10 版本 1511 或更高版本的移动设备。 在 Device Portal 中使用**[应用](../debug-test-perf/device-portal.md#apps)**页上传应用包 (.appx) 并在设备上安装它。

## <a name="use-group-policies-or-registry-keys-to-enable-a-device"></a>使用组策略或注册表项启用设备

对于大多数开发人员而言，希望使用“设置”应用启用设备进行调试。 在诸如自动执行测试等方案中，可以使用其他方法启用 Windows 10 桌面设备进行开发。

可以使用 gpedit.msc 设置组策略来启用设备，除非你拥有 Windows 10 家庭版。 如果你有 Windows 10 家庭版，则需要使用 regedit 或 PowerShell 命令直接设置注册表项以启用设备。

**使用 gpedit 启用设备**

1.  运行 **Gpedit.msc**。
2.  转到“本地计算机策略”&gt;“计算机配置”&gt;“管理模板”&gt;“Windows 组件”&gt;“应用包部署”
3.  若要启用旁加载，请编辑策略以启用以下项：

    -   **允许安装所有受信任的应用**

    - 或：

    若要启用开发人员模式，请编辑策略以启用以下两项：

    -   **允许安装所有受信任的应用**
    -   **允许开发 Windows 应用商店应用并从集成开发环境 (IDE) 安装这些应用**

4.  重新启动计算机。

**使用 regedit 启用设备**

1.  运行 **regedit**。
2.  若要启用旁加载，请将此 DWORD 的值设置为 1：

    -   **HKLM\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\AppModelUnlock\\AllowAllTrustedApps**

    - 或：

    若要启用开发人员模式，请将此 DWORD 的值设置为 1：

    -   **HKLM\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\AppModelUnlock\\AllowDevelopmentWithoutDevLicense**

**使用 PowerShell 启用设备**

1.  使用管理员权限运行 PowerShell。
2.  若要启用旁加载，请运行此命令：

    -   **PS C:\\WINDOWS\\system32&gt; reg add "HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\AppModelUnlock" /t REG\_DWORD /f /v "AllowAllTrustedApps" /d "1"**

    - 或 -

    若要启用开发人员模式，请运行此命令：

    -   **PS C:\\WINDOWS\\system32&gt; reg add "HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\AppModelUnlock" /t REG\_DWORD /f /v "AllowDevelopmentWithoutDevLicense" /d "1"**

## <a name="upgrade-your-device-from-windows-81-to-windows-10"></a>将设备从 Windows 8.1 升级到 Windows 10

当你在 Windows 8.1 设备上创建或旁加载应用时，必须安装开发人员许可证。 如果你将设备从 Windows 8.1 升级到 Windows 10，将保留此信息。 运行以下命令以从已升级的 Windows 10 设备中删除此信息。 如果你直接从 Windows 8.1 升级到版本为 1511 或更高版本的 Windows 10，则无需执行此步骤。

**注销开发人员许可证**

1.  使用管理员权限运行 PowerShell。
2.  运行此命令：**unregister-windowsdeveloperlicense**。

在这之后你需要启用设备用于开发（如本题中所述），以便可以继续在此设备上进行开发。 如果你不执行此操作，则可能在调试应用或为其创建程序包时遇到错误。 以下是此错误的示例：

错误：DEP0700：应用注册失败。



<!--HONumber=Dec16_HO2-->


