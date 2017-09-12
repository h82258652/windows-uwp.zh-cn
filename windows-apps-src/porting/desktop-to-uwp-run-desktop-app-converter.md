---
author: normesta
Description: "运行 Desktop App Converter 以将 Windows 桌面应用程序（如 Win32、WPF 和 Windows 窗体）打包。"
Search.Product: eADQiWindows 10XVcnh
title: "使用 Desktop App Converter 将应用打包（桌面桥）"
ms.author: normesta
ms.date: 05/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 74c84eb6-4714-4e12-a658-09cb92b576e3
ms.openlocfilehash: 6aa469d0080412f48de511315684d365d4e90c99
ms.sourcegitcommit: 6c6f3c265498d7651fcc4081c04c41fafcbaa5e7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/09/2017
---
# <a name="package-an-app-using-the-desktop-app-converter-desktop-bridge"></a>使用 Desktop App Converter 将应用打包（桌面桥）

[获取 Desktop App Converter](https://aka.ms/converter)

可以使用 Desktop App Converter (DAC) 将桌面应用引入通用 Windows 平台 (UWP)。 这包括 Win32 应用和使用 .NET 4.6.1 创建的应用。

<div style="float: left; padding: 10px">
    ![DAC 图标](images/desktop-to-uwp/dac.png)
</div>

尽管此工具名称中出现了“Converter”一词，但它实际上并不转换你的应用。 你的应用将保持不变。 但是，此工具会生成一个带有应用包标识并能够调用各种 WinRT API 的 Windows 应用包。

可以通过使用 Add-AppxPackage PowerShell cmdlet 在开发计算机上安装该程序包。

转换器使用作为转换器下载的一部分提供的干净的基础映像在隔离的 Windows 环境中运行桌面安装程序。 它捕获桌面安装程序进行的任何注册表和文件系统 I/O，并将其作为输出的一部分打包。

> [!NOTE]
> 请观看 Microsoft Virtual Academy 发布的<a href="https://mva.microsoft.com/en-US/training-courses/developers-guide-to-the-desktop-bridge-17373?l=oZG0B1WhD_8406218965/">这一系列</a>的视频短片。 这些视频演示了使用 Desktop App Converter 的一些常见方法。

## <a name="the-dac-does-more-than-just-generate-a-package-for-you"></a>DAC 不仅能为你生成一个包

以下是它可以为你执行的其他一些操作。

**Windows 10 创意者更新**

<div style="float: left; ">
    ![选中](images/desktop-to-uwp/check.png)
</div>

自动注册预览控件、缩略图处理程序、属性处理程序、防火墙规则、URL 标志。

<div style="float: left; ">
    ![选中](images/desktop-to-uwp/check.png)
</div>

自动注册使用户能够使用“文件资源管理器”中的**种类**列对文件进行分组的文件类型映射。

<div style="float: left; ">
    ![选中](images/desktop-to-uwp/check.png)
</div>

注册你的公共 COM 服务器。

**Windows 10 周年更新或更高版本**

<div style="float: left; ">
    ![选中](images/desktop-to-uwp/check.png)
</div>

自动对程序包进行签名，使你能够测试你的应用。

<div style="float: left; ">
    ![选中](images/desktop-to-uwp/check.png)
</div>

根据桌面桥和 Windows 应用商店要求验证你的应用。

若要查找完整的选项列表，请参阅本指南的[参数](#command-reference)部分。

如果你已准备好创建应用包，那我们开始吧。

## <a name="first-consider-how-youll-distribute-your-app"></a>首先，请考虑你将如何分发应用
如果你打算将应用发布到 [Windows 应用商店](https://www.microsoft.com/store/apps)，请先填写[此表单](https://developer.microsoft.com/windows/projects/campaigns/desktop-bridge)。 Microsoft 将联系你以开始加入过程。 在此过程中，你需要在应用商店中预留一个名称，然后获取对应用打包所需的信息。

## <a name="make-sure-that-your-system-can-run-the-converter"></a>请确保你的系统可以运行转换器

请确保你的系统满足以下要求：

* Windows10 周年更新（10.0.14393.0 和更高版本）专业版或企业版。
* 64 位 (x64) 处理器
* 硬件辅助虚拟化
* 二级地址转换 (SLAT)
* [适用于 Windows10 的 Windows 软件开发工具包 (SDK)](https://go.microsoft.com/fwlink/?linkid=821375)。


## <a name="start-the-desktop-app-converter"></a>启动 Desktop App Converter

1. 下载并安装 [Desktop App Converter](https://aka.ms/converter)。

2. 以管理员身份运行 Desktop App Converter。

    ![以管理员身份运行 DAC](images/desktop-to-uwp/run-converter.png)

   会出现一个控制台窗口。 你需要使用该控制台窗口运行命令。

## <a name="set-a-few-things-up-apps-with-installers-only"></a>进行一些设置（仅限具有安装程序的应用）

如果你的应用没有安装程序，则可以跳到下一部分。

1. 识别操作系统的版本号。

   为此，可以在计算机上打开**系统信息应用**，然后查找操作系统的版本号。

    ![系统信息应用中的操作系统版本](images/desktop-to-uwp/os-version.png)

2. 下载相应的 [Desktop App Converter 基础映像](https://aka.ms/converterimages)。

   确保文件名中显示的版本号与 Windows 内部版本的版本号相匹配。

   ![匹配的基础映像版本](images/desktop-to-uwp/base-image-version.png)

   将下载的文件放置在你稍后可在计算机上找到它的任意位置。

3. 在启动 Desktop App Converter 时出现的控制台窗口中，运行此命令：```Set-ExecutionPolicy bypass```。
4.  通过运行此命令设置转换器：```DesktopAppConverter.exe -Setup -BaseImage .\BaseImage-1XXXX.wim -Verbose```。
5.  如果系统提示你重启计算机，请执行此操作。

    当转换器展开基础映像时，控制台窗口中会显示状态消息。 如果未看到任何状态消息，请按任意键。 此操作可以刷新控制台窗口的内容。

    ![控制台窗口中的状态消息](images/desktop-to-uwp/bas-image-setup.png)

    当基础映像完全展开后，请转至下一部分。

## <a name="package-an-app"></a>将应用打包

若要将应用打包，请在启动 Desktop App Converter 时打开的控制台窗口中运行 ``DesktopAppConverter.exe`` 命令。  

你将通过使用参数指定应用的包名称、发布者和版本号。

> [!NOTE]
> 如果你在 Windows 应用商店中预留了应用名称，则可以通过使用 Windows 开发人员中心仪表板获取包名称和发布者名称。 如果你打算将应用旁加载到其他系统上，只要选择的发布者名称与用于对应用进行签名的证书上的名称相匹配，就可以提供自己的名称。

### <a name="a-quick-look-at-command-parameters"></a>命令参数概览

以下是所需参数。

```CMD
DesktopAppConverter.exe
-Installer <String>
-Destination <String>
-PackageName <String>
-Publisher <String>
-Version <Version>
```
可以在[此处](#command-reference)阅读有关每个参数的信息。

### <a name="examples"></a>示例

以下是将应用打包的一些常见方法。

* [将具有安装程序 (.msi) 文件的应用打包](#installer-conversion)
* [将具有安装程序可执行文件的应用打包](#setup-conversion)
* [将没有安装程序的应用打包](#no-installer-conversion)
* [将应用打包、对应用进行签名并为应用商店提交做好准备](#optional-parameters)

<span id="installer-conversion" />
#### <a name="package-an-app-that-has-an-installer-msi-file"></a>将具有安装程序 (.msi) 文件的应用打包

使用 ``Installer`` 参数指向安装程序文件。

```cmd
DesktopAppConverter.exe -Installer C:\Installer\MyAppSetup.msi -Destination C:\Output\MyApp -PackageName "MyApp" -Publisher "CN=MyPublisher" -Version 0.0.0.1
```

> [!NOTE]
> 请确保安装程序位于独立文件夹中，并确保只有与该安装程序相关的文件位于同一文件夹中。 转换器将该文件夹的所有内容复制到隔离的 Windows 环境中。

**视频**

<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Convert-an-Application-That-Has-an-MSI-Installer-Kh1UU2WhD_7106218965" width="636" height="480" allowFullScreen frameBorder="0"></iframe>

<span id="setup-conversion" />
#### 将具有安装程序可执行文件的应用打包

使用 ``Installer`` 参数指向安装程序可执行文件。

```cmd
DesktopAppConverter.exe -Installer C:\Installer\MyAppSetup.exe -InstallerArguments "/S" -Destination C:\Output\MyApp -PackageName "MyApp" -Publisher "CN=MyPublisher" -Version 0.0.0.1
```

``InstallerArguments`` 参数是可选参数。 但是，因为 Desktop App Converter 需要安装程序在无人参与模式下运行，因此如果你的应用需要无提示标志以静默方式运行，则可能需要使用该参数。 ``/S`` 标志是十分常见的无提示标志，但你使用的标志可能有所不同，具体取决于用于创建安装程序文件的安装程序技术。

**视频**

<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Convert-an-Application-That-Has-a-Setup-exe-Installer-amWit2WhD_5306218965" width="636" height="480" allowFullScreen frameBorder="0"></iframe>

<span id="no-installer-conversion" />
#### 将没有安装程序的应用打包

在此示例中，使用 ``Installer`` 参数指向应用文件的根文件夹。

使用 `AppExecutable` 参数指向应用可执行文件。

```cmd
DesktopAppConverter.exe -Installer C:\Installer\MyApp\ -AppExecutable MyApp.exe -Destination C:\Output\MyApp -PackageName "MyApp" -Publisher "CN=MyPublisher" -Version 0.0.0.1
```

**视频**

<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Convert-a-No-Installer-Application-agAXF2WhD_3506218965" width="636" height="480" allowFullScreen frameBorder="0"></iframe>

<span id="optional-parameters" />
#### 将应用打包、对应用进行签名并对应用包运行验证检查

此示例类似于第一个示例，不同之处在于它显示了如何对应用进行签名以进行本地测试，然后根据桌面桥和 Windows 应用商店的要求验证你的应用。

```cmd
DesktopAppConverter.exe -Installer C:\Installer\MyAppSetup.exe -InstallerArguments "/S" -Destination C:\Output\MyApp -PackageName "MyApp" -Publisher "CN=MyPublisher" -Version 0.0.0.1 -MakeAppx -Sign -Verbose -Verify
```

``Sign`` 参数生成一个证书，然后使用该证书对应用进行签名。 若要运行应用，你需要安装生成的证书。 若要了解如何操作，请参阅本指南的[运行已打包的应用](#run-app)部分。

可以使用 ``Verify`` 参数验证你的应用。

### <a name="a-quick-look-at-optional-parameters"></a>可选参数概览

``Sign`` 和 ``Verify`` 参数是可选参数。 还有更多的可选参数。  以下是一些更常用的可选参数。

```CMD
[-ExpandedBaseImage <String>]
[-AppExecutable <String>]
[-AppFileTypes <String>]
[-AppId <String>]
[-AppDisplayName <String>]
[-AppDescription <String>]
[-PackageDisplayName <String>]
[-PackagePublisherDisplayName <String>]
[-MakeAppx]
[-LogFile <String>]
[<CommonParameters>]
```
可以在下一部分中阅读有关所有参数的信息。

<span id="command-reference" />
### <a name="parameter-reference"></a>参数引用

以下是运行 Desktop App Converter 时可使用的参数的完整列表（按类别列出）。

* [安装](#setup-params)
* [转换](#conversion-params)
* [程序包标识符](#identity-params)
* [程序包清单](#manifest-params)
* [清除](#cleanup-params)
* [体系结构](#architecture-params)
* [杂项](#other-params)

还可以通过在应用控制台窗口中运行 ``Get-Help`` 命令来查看整个列表。   

||||
|-------------|-----------|-------------|
|<span id="setup-params" /> <strong>设置参数</strong>  ||
|-Setup [&lt;SwitchParameter&gt;] |必需 |在安装模式下运行 DesktopAppConverter。 安装模式支持扩展所提供的基础映像。|
|-BaseImage &lt;String&gt; | 必需 |未展开的基础映像的完整路径。 如果指定了 -Setup，则需要此参数。|
| -LogFile &lt;String&gt; |可选 |指定日志文件。 如果省略此参数，将创建一个日志文件临时位置。|
|-NatSubnetPrefix &lt;String&gt; |可选 |用于 Nat 实例的前缀值。 通常，仅在主机连接到与转换器的 NetNat 相同的子网范围时，你会希望更改此值。 可以通过使用 **Get-NetNat** cmdlet 查询当前的转换器 NetNat 配置。 |
|-NoRestart [&lt;SwitchParameter&gt;] |必需 |在运行安装程序时不提示重启（需要进行重启才能启用容器功能）。 |
|<span id="conversion-params" /> <strong>转换参数</strong>|||
|-AppInstallPath &lt;String&gt;  |可选 |包含已安装文件的应用程序（如果已安装）的根文件夹的完整路径（例如“C:\Program Files (x86)\MyApp”）。|
|-Destination &lt;String&gt; |必需 |转换器的 appx 输出的所需目标位置 - 如果此位置尚不存在，DesktopAppConverter 则可以创建此位置。|
|-Installer &lt;String&gt; |必需 |适用于应用程序的安装程序的路径 - 必须能够在无人参与/静默的情况下运行。 无安装程序的转换，这是应用文件的根目录的路径。 |
|-InstallerArguments &lt;String&gt; |可选 |用于强制安装程序在无人参与/静默的情况下运行的参数的以逗号分隔的列表或字符串。 如果安装程序是 msi，则此参数为可选参数。 若要从安装程序中获取日志，请在此处为安装程序提供日志记录参数，并使用路径 &lt;log_folder&gt;，该路径是转换器使用相应路径所替换的标记。 <br><br>**注意**：无人参与/无提示标志和日志参数将因安装程序技术而异。 <br><br>此参数的用法示例：-InstallerArguments "/silent /log &lt;log_folder&gt;\install.log" 另一个不生成日志文件的示例可能如下所示：```-InstallerArguments "/quiet", "/norestart"``` 同样，如果你希望转换器捕获日志并将其放置在最终的日志文件夹中，则必须逐字节地将任何日志直接指向标记路径 &lt;log_folder&gt;。|
|-InstallerValidExitCodes &lt;Int32&gt; |可选 |指示安装程序成功运行的退出代码的以逗号分隔的列表（例如 0，1234，5678）。  默认情况下，对于非 msi，它为 0，对于 msi，它为 0，1641，3010。|
|<span id="identity-params" /><strong>程序包标识符参数</strong>||
|-PackageName &lt;String&gt; |必需 |通用 Windows 应用包的名称 |
|-Publisher &lt;String&gt; |必需 |通用 Windows 应用包的发布者 |
|-Version &lt;Version&gt; |必需 |通用 Windows 应用包的版本号 |
|<span id="manifest-params" /><strong>程序包清单参数</strong>||
|-AppExecutable &lt;String&gt; |可选 |应用程序的主要可执行文件的名称（例如“MyApp.exe”）。 无安装程序的转换需要使用此参数。 |
|-AppFileTypes &lt;String&gt;|可选 |应用程序将与其关联的文件类型的以逗号分隔的列表。 用法示例：-AppFileTypes "'.md', '.markdown'"。|
|-AppId &lt;String&gt; |可选 |在 Windows 应用程序包清单中指定要将应用程序 ID 设置为的值。 如果未指定，则将其设置为为 *PackageName* 传入的值。|
|-AppDisplayName &lt;String&gt;  |可选 |在 Windows 应用程序包清单中指定要将应用程序显示名称设置为的值。 如果未指定，则将其设置为为 *PackageName* 传入的值。 |
|-AppDescription &lt;String&gt; |可选 |在 Windows 应用程序包清单中指定要将应用程序描述设置为的值。 如果未指定，则将其设置为为 *PackageName* 传入的值。|
|-PackageDisplayName &lt;String&gt; |可选 |在 Windows 应用程序包清单中指定要将程序包显示名称设置为的值。 如果未指定，则将其设置为为 *PackageName* 传入的值。 |
|-PackagePublisherDisplayName &lt;String&gt; |可选 |在 Windows 应用程序包清单中指定要将程序包发布者显示名称设置为的值。 如果未指定，则将其设置为为 *Publisher* 传入的值。 |
|<span id="cleanup-params" /><strong>清除参数</strong>|||
|-Cleanup [&lt;Option&gt;] |必需 |为 DesktopAppConverter 项目运行清除。 清除模式有 3 个有效的选项。 |
|-Cleanup All | |删除所有已展开的基础映像、删除任何临时转换器文件、删除容器网络并禁用可选的 Windows 功能，即容器。 |
|-Cleanup WorkDirectory |必需 |删除所有临时转换器文件。 |
|-Cleanup ExpandedImage |必需 |删除安装在主机上的所有已展开的基础映像。 |
|<span id="architecture-params" /><strong>程序包体系结构参数</strong>|||
|-PackageArch &lt;String&gt; |必需 |生成具有指定体系结构的程序包。 有效选项为“x86”或“x64”，例如 -PackageArch x86。 该参数为可选参数。 如果未指定，DesktopAppConverter 将尝试自动检测程序包体系结构。 如果自动检测失败，它将默认为 x64 程序包。 |
|<span id="other-params" /><strong>其他参数</strong>|||
|-ExpandedBaseImage &lt;String&gt;  |可选 |已展开的基础映像的完整路径。|
|-MakeAppx [&lt;SwitchParameter&gt;]  |可选 |一个告知此脚本对输出调用 MakeAppx 的开关（如果有）。 |
|-LogFile &lt;String&gt;  |可选 |指定日志文件。 如果省略此参数，将创建一个日志文件临时位置。 |
| -Sign [&lt;SwitchParameter&gt;] |可选 |出于测试目的，告知此脚本使用生成的证书对输出 Windows 应用包进行签名。 此开关应该位于开关 ```-MakeAppx``` 的旁边。 |
|&lt;通用参数&gt; |必需 |此 cmdlet 支持通用参数：*Verbose*、*Debug*、*ErrorAction*、*ErrorVariable*、*WarningAction*、*WarningVariable*、*OutBuffer*、*PipelineVariable* 和 *OutVariable*。 有关详细信息，请参阅 [about_CommonParameters](http://go.microsoft.com/fwlink/?LinkID=113216)。 |
| -Verify [&lt;SwitchParameter&gt;] |可选 |一个告知 DAC 根据桌面桥和 Windows 应用商店的要求验证应用包的开关（如果有）。 结果是一个“VerifyReport.xml”验证报告，该报告在浏览器中能够以最佳方式显示。 此开关应该位于开关 `-MakeAppx` 的旁边。 |
|-PublishComRegistrations| 可选| 扫描你的安装程序进行的所有公共 COM 注册，并发布你的清单中有效的注册。 仅当想使这些注册可供其他应用程序使用时才使用此标志。 如果这些注册将仅由你的应用程序使用，则无需使用此标志。 <br><br>请查看[本文](https://blogs.windows.com/buildingapps/2017/04/13/com-server-ole-document-support-desktop-bridge/#lDg5gSFxJ2TDlpC6.97)以确保在你将应用打包后，COM 注册会按预期工作。

<span id="run-app" />
## <a name="run-the-packaged-app"></a>运行打包的应用

可通过两种方法运行应用。

一种方法是打开 PowerShell 命令提示符，然后键入此命令：```Add-AppxPackage –Register AppxManifest.xml```。 这可能是运行应用最简单的方法，因为你无需对其进行签名。

另一种方法是使用证书对应用进行签名。 如果使用 ```sign``` 参数，Desktop App Converter 将为你生成一个证书，然后使用该证书对应用进行签名。 该文件名为 **auto-generated.cer**，你可以在已打包应用的根文件夹中找到它。

请按照以下步骤安装生成的证书，然后运行应用。

1. 双击 **auto-generated.cer** 文件安装证书。

   ![生成的证书文件](images/desktop-to-uwp/generated-cert-file.png)

   > [!NOTE]
   > 如果系统提示你输入密码，请使用默认密码“123456”。

2. 在**证书**对话框中，选择**安装证书**按钮。
3. 在**证书导入向导**中，将证书安装到**本地计算机**上，并将证书放在**受信任人**证书存储区中。

   ![“受信任人”存储区](images/desktop-to-uwp/trusted-people-store.png)

5. 在已打包应用的根文件夹中，双击 Windows 应用包文件。

   ![Windows 应用包文件](images/desktop-to-uwp/windows-app-package.png)

6. 选择**安装**按钮以安装应用。

   ![“安装”按钮](images/desktop-to-uwp/install.png)


## <a name="modify-the-packaged-app"></a>修改已打包的应用

你可能需要对已打包的应用进行更改，以解决 Bug、添加视觉资源或使用动态磁贴等现代体验增强你的应用。

进行更改之后，无需再次运行转换器。 可以使用 MakeAppx 工具以及 DAC 为你的应用生成的 appxmanifest.xml 文件将应用重新打包。 请参阅[生成 Windows 应用包](desktop-to-uwp-manual-conversion.md#make-appx)。

> [!NOTE]
> 如果你对安装程序进行的注册表设置做出了更改，则需要再次运行 Desktop App Converter 以应用这些更改。

**视频**

|修改并重新打包输出 |演示：修改并重新打包输出|
|---|---|
|<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Video-Modifying-and-Repackaging-Output-from-Desktop-App-Converter-OwpAJ3WhD_6706218965" width="426" height="472" allowFullScreen frameBorder="0"></iframe>|<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Modify-Output-from-Desktop-App-Converter-gEnsa3WhD_8606218965" width="426" height="472" allowFullScreen frameBorder="0"></iframe>|

以下两部分介绍了针对已打包的应用可考虑使用的一些可选修复。

### <a name="delete-unnecessary-files-and-registry-keys"></a>删除不必要的文件和注册表项

Desktop App Converter 采用非常保守的方法来筛选掉容器中的文件和系统干扰。

如有需要，可以查看 VFS 文件夹并删除安装程序不需要的任何文件。  还可以查看 Reg.dat 的内容并删除应用未安装/不需要的任何项。

### <a name="fix-corrupted-pe-headers"></a>修复损坏的 PE 头

在转换过程中，DesktopAppConverter 将自动运行 PEHeaderCertFixTool 以修复任何损坏的 PE 头。 但是，还可以在 UWP Windows 应用包、松散文件或特定的二进制文件上运行 PEHeaderCertFixTool。 以下提供了一个示例。

```CMD
PEHeaderCertFixTool.exe <binary file>|<.appx package>|&lt;folder> [/c] [/v]
 /c   -- check for corrupted certificate but do not fix (optional)
 /v   -- verbose (optional)
example1: PEHeaderCertFixTool app.exe
example2: PEHeaderCertFixTool c:\package.appx /c
example3: PEHeaderCertFixTool c:\myapp /c /v
```

## <a name="known-issues-and-disclosures"></a>已知问题和披露

### <a name="known-issues-and-workarounds"></a>已知问题和解决方法

以下是一些已知问题以及可以尝试解决这些问题的一些方法。

#### <a name="ecreatingisolatedenvfailed-an-estartingisolatedenvfailed-errors"></a>E_CREATING_ISOLATED_ENV_FAILED an E_STARTING_ISOLATED_ENV_FAILED 错误    

如果收到任一错误，请确保你正在使用的是从[下载中心](https://aka.ms/converterimages)下载的有效基础映像。
如果使用的是有效的基础映像，请尝试在命令中使用 ``-Cleanup All``。
如果不起作用，请将日志发送至 converter@microsoft.com，以帮助我们进行调查。

#### <a name="new-containernetwork-the-object-already-exists-error"></a>New-ContainerNetwork：对象已存在错误

设置新的基础映像时，可能会收到此错误。 如果你在以前安装了 Desktop App Converter 的开发人员计算机上安装有 Windows 预览体验计划外部测试版，则可能会出现这种情况。

若要解决此问题，请尝试从提升的命令提示符中运行命令 `Netsh int ipv4 reset`，然后重启计算机。

#### <a name="your-net-app-is-compiled-with-the-anycpu-build-option-and-fails-to-install"></a>你的 .NET 应用是使用“AnyCPU”生成选项编译的，无法安装

如果将主要可执行文件或任何依赖项放置在 **Program Files** 或 **Windows\System32** 文件夹层次结构中的任意位置，则可能会出现这种情况。

若要解决此问题，请尝试使用你体系结构特定的桌面安装程序（32 位或 64 位）生成 Windows 应用包。

#### <a name="publishing-public-side-by-side-fusion-assemblies-wont-work"></a>发布公共并行的 Fusion 程序集不起作用。

 在安装期间，应用程序可以发布公共并行的 Fusion 程序集，这些程序集可供任何其他进程访问。 在进程激活上下文创建期间，这些程序集将由名为 CSRSS.exe 的系统进程检索。 当为已转换的进程执行此操作后，这些程序集的激活上下文创建和模块加载将失败。 并行的 Fusion 程序集注册在以下位置中：
  + 注册表： `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\SideBySide\Winners`
  + 文件系统：%windir%\\SideBySide

这是一个已知限制，目前尚无解决方法。 即，内置程序集（如 ComCtl）随操作系统附带，因此依赖它们是安全的行为。

#### <a name="error-found-in-xml-the-executable-attribute-is-invalid---the-value-myappexe-is-invalid-according-to-its-datatype"></a>XML 中发现错误。 “Executable”特性无效 - 根据其数据类型，值“MyApp.EXE”无效

如果应用程序中的可执行文件具有大写的 **.EXE** 扩展名，则可能会出现这种情况。 虽然此扩展名的大小写不应影响应用是否能够运行，但这可能导致 DAC 生成此错误。

若要解决此问题，请尝试在打包时指定 **-AppExecutable** 标志，并使用小写的“.exe”作为主要可执行文件的扩展名（例如 MYAPP.exe）。    或者，可以将应用中的所有可执行文件的大小写从大写更改为小写（例如从 .EXE 更改为 .exe）。


### <a name="telemetry-from-desktop-app-converter"></a>来自 Desktop App Converter 的遥测

Desktop App Converter 可以收集关于你和你使用该软件的情况的信息，并将此信息发送给 Microsoft。 你可以在产品文档和 [Microsoft 隐私声明](http://go.microsoft.com/fwlink/?LinkId=521839)中了解有关 Microsoft 的数据收集和使用的详细信息。 你同意遵守 Microsoft 隐私声明的所有适用条款。

默认情况下，将为 Desktop App Converter 启用遥测。 添加以下注册表项以将遥测配置为所需设置：  

```cmd
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\DesktopAppConverter
```
+ 通过使用设置为 1 的 DWORD 来添加或编辑 *DisableTelemetry* 值。
+ 若要启用遥测，请删除该项或将值设置为 0。

### <a name="language-support"></a>语言支持

Desktop App Converter 不支持 Unicode；因此，没有可用于该工具的任何中文字符或非 ASCII 字符。

## <a name="next-steps"></a>后续步骤

**运行应用/查找和修复问题**

请参阅[运行、调试和测试打包的桌面应用（桌面桥）](desktop-to-uwp-debug.md)

**分发应用**

请参阅[分发打包的桌面应用（桌面桥）](desktop-to-uwp-distribute.md)

**查找特定问题的答案**

我们的团队会监控这些 [StackOverflow 标记](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。

**提供关于本文的反馈**

请使用下面的评论区。
