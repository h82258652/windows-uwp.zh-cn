---
description: author: martinekuan ms.assetid: 54973C62-9669-4988-934E-9273FB0425FD title: 启用设备进行开发 可使用不同的方法针对 Windows 10 设备进行开发。
keywords: 开始使用 keywords: 开发人员许可证 keywords: Visual Studio、开发人员许可证 keywords: 启用设备
---
# 启用设备进行开发

可使用不同的方法针对 Windows 10 设备进行开发。 对于要用于开发、安装或测试应用的每台设备，不再需要开发人员许可证。 你只需从设备的设置中为这些任务启用设备一次。 就这么简单。 无需每 30 或 90 天续订开发人员许可证一次！

如果你仍然使用 Windows 8.1 设备通过 Microsoft Visual Studio 2013 或 Microsoft Visual Studio 2015 开发或测试应用，则仍然需要[获取开发人员许可证](https://msdn.microsoft.com/library/windows/apps/Hh974578)或[注册 Windows Phone](https://msdn.microsoft.com/library/windows/apps/Dn614128)。

## 使用开发人员功能

### 使用 Microsoft Visual Studio 开发应用

如果你在 Windows 10 设备上使用 Microsoft Visual Studio 并打开适用于 Windows 8.1 或 Windows 10 应用的解决方案，系统将通过此对话框提示你启用设备。 你需要启用设备才能使用设计器和调试应用。

![启用在 Visual Studio 中显示的开发人员模式对话框](images/latestenabledialog.png)

当你看到此对话框时，请单击“开发人员设置”以直接转到“更新和安全”页面，如下所示。 或者，单击“确定”，然后按照以下步骤启用 Windows 10 设备以进行开发。

### 启用 Windows 10 设备

对于 Windows 10，由你选择要在设备上启用的开发人员功能。 这包括任何设备：Windows 10 台式机、平板电脑和手机。 你可以启用设备以进行开发，或仅进行旁加载。

-   *旁加载*是指安装未经 Windows 应用商店认证的应用，然后对其进行运行或测试的过程。 例如，仅限公司内部使用的应用。
-   借助*开发人员模式*，你不仅可以旁加载应用，还可以在调试模式下从 Visual Studio 运行应用。

**注意** 如果旁加载应用，你仍然应该仅从受信任的源安装应用。 安装未经 Windows 应用商店认证的旁加载应用时，即表明你同意已获取旁加载应用所需的所有权限，并且你对任何由安装和运行应用引发的损害负全责。 请参阅 Windows &gt; 此[隐私声明](http://go.microsoft.com/fwlink/?LinkId=521839)的 Windows 应用商店部分。

**使用开发人员功能**

1.  在要启用的设备上，转到“设置”。 依次选择“更新和安全”和“对于开发人员”。
2.  选择所需的访问级别。 有关选项的更多详细信息，请参阅[应选择哪些设置：旁加载应用还是开发人员模式？](#WhichSettings)
3.  阅读所选设置的免责声明，然后单击“是”以接受更改。

下面是桌面设备系列的设置页。

![转到“设置”、选择“更新和安全”，然后选择“面向开发人员”来查看你的选项](images/devmode-pc-options.png)

下面是移动设备系列的设置页。

![在手机上的“设置”中，选择“更新和安全”](images/devmode-mob.png)

### 应选择哪些设置：旁加载应用还是开发人员模式？

默认情况下，你只能从 Windows 应用商店安装通用 Windows 平台 (UWP) 应用。 将这些设置更改为使用开发人员功能时可能需更改设备的安全级别。 不应从未经验证的源安装应用。

**旁加载应用**

旁加载应用设置通常由需要在未通过 Windows 应用商店认证的托管设备上安装自定义应用的公司或学校使用。 在此情况下，组织通常会强制执行禁用 *Windows 应用商店应用*设置的策略，如之前的手机设置页图像中所示。 组织还会提供旁加载应用所需的证书和安装位置。 有关详细信息，请参阅 TechNet 文章[在 Windows 10 中旁加载应用](https://technet.microsoft.com/library/mt269549.aspx)和 [Microsoft Intune 中的应用部署入门](https://technet.microsoft.com/library/dn646955.aspx)。

设备系列特定信息

-   对于桌面设备系列：你可以通过运行使用程序包（“Add-AppDevPackage.ps1”）创建的 Windows PowerShell 脚本，来安装应用包 (.appx) 和运行应用所需的任何证书。

-   对于移动设备系列：如果已安装了必需的证书，你可以点击文件以安装任何通过电子邮件收到的或 SD 卡上的 .appx。

**旁加载应用**是比开发人员模式更安全的选项，因为你无法在缺少可信任证书的设备上安装应用。

**开发人员模式**

除了旁加载外，开发人员模式设置还支持调试和其他部署选项。 它将替换 Windows 8.1 对于开发人员许可证的要求。

设备系列特定信息

-   对于桌面设备系列：

    启用开发人员模式以在 Visual Studio 开发和调试应用。 正如前面所述，如果未启用开发人员模式，你将在 Visual Studio 中收到提示。

-   对于移动设备系列：

    启用开发人员模式以从 Visual Studio 部署应用并在设备上调试它们。

    你可以点击文件以安装通过电子邮件收到的或 SD 卡上的任何 .appx。 请不要从未经验证的源安装应用。

**提示**  
有多个工具可用来将应用从 Windows 10 电脑部署到 Windows 10 移动设备。 两台设备均必须通过有线或无线的连接方式连接到网络的同一子网，或者它们必须通过 USB 进行连接。 所列的任意一种方法仅会安装应用包 (.appx)，不安装证书。

-   使用 Windows 10 应用程序部署 (WinAppDeployCmd) 工具。 了解有关 [WinAppDeployCmd 工具](http://msdn.microsoft.com/library/windows/apps/mt203806.aspx)的详细信息。
-   从版本 1511 的 Windows 10 开始，你可以使用 [Device Portal](#device_portal) 从浏览器部署到运行版本 1511 或更高版本的 Windows 10 的移动设备。 在 Device Portal (&lt;IP&gt;/appmanager.md) 中使用“应用”页上载应用包 (.appx) 并在设备上安装它。

 

### 设置组策略或注册表项

你还可以使用组策略或注册表项作为启用用于开发的 Windows 10 桌面设备的备用方法。

**对于桌面设备系列**

使用 gpedit.msc 设置组策略来启用设备，除非你拥有 Windows 10 家庭版。 如果你有 Windows 10 家庭版，则需要使用 regedit 或 PowerShell 命令直接设置注册表项以启用设备。

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

## 开发人员模式功能

对于每个设备系列，可能会提供其他开发人员功能。 仅当在设备上启用了“开发人员模式”时这些功能才可用，并且可能会因操作系统版本的不同而有所不同。

此图显示了适用于版本 1511 Windows 10 移动设备系列的开发人员功能。

![适用于移动设备的开发人员模式选项](images/devmode-mob-options.png)

### <span id="device-discovery-and-pairing"></span>设备发现和 Device Portal

若要了解有关设备发现和 Device Portal 的详细信息，请参阅 [Windows Device Portal 概述](../debug-test-perf/device-portal.md)。

有关特定于设备的设置说明，请参阅：
- [适用于 HoloLens 的 Device Portal](https://dev.windows.com/holographic/using_the_windows_device_portal)
- [适用于 IoT 的 Device Portal](http://ms-iot.github.io/content/en-US/win10/tools/DevicePortal.htm)
- [适用于移动设备的 Device Portal](../debug-test-perf/device-portal-mobile.md)
- [适用于 Xbox 的 Device Portal](../debug-test-perf/device-portal-xbox.md)

### 错误报告

设置此值以指定在手机上保存的故障转储量。

通过收集手机上的故障转储，你可以在故障发生后直接快速访问重要的故障信息。 仅为开发人员签名的应用收集转储。 你可以在“Documents\\Debug folder”文件夹的手机存储中查找转储。 有关转储文件的详细信息，请参阅[使用转储文件](https://msdn.microsoft.com/library/d5zhxt22.aspx)。

## 将设备从 Windows 8.1 升级到 Windows 10

当你在 Windows 8.1 设备上创建或旁加载应用时，必须安装开发人员许可证。 如果你将设备从 Windows 8.1 升级到 Windows 10，将保留此信息。 运行以下命令以从已升级的 Windows 10 设备中删除此信息。 如果你直接从 Windows 8.1 升级到版本为 1511 或更高版本的 Windows 10，则无需执行此步骤。

**注销开发人员许可证**

1.  使用管理员权限运行 PowerShell。
2.  运行此命令：**unregister-windowsdeveloperlicense**。

在这之后你需要启用设备用于开发（如本题中所述），以便可以继续在此设备上进行开发。 如果你不执行此操作，则可能在调试应用或为其创建程序包时遇到错误。 以下是此错误的示例：

错误：DEP0700：应用注册失败。




<!--HONumber=May16_HO2-->


