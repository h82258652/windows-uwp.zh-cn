---
author: awkoren
Description: "运行桌面转换器应用以将 Windows 桌面应用程序（如 Win32、WPF 和 Windows 窗体）转换为通用 Windows 平台 (UWP) 应用。"
Search.Product: eADQiWindows 10XVcnh
title: Desktop App Converter
translationtype: Human Translation
ms.sourcegitcommit: bf6da2f4d780774819fe7a4abf6367345304767c
ms.openlocfilehash: 3ffd664892fe5ee589d3bf5704e2eeed178bf5f3

---

# <a name="desktop-app-converter"></a>Desktop App Converter

[获取 Desktop App Converter](https://aka.ms/converter)

Desktop App Converter 是一款工具，允许你将为 .NET 4.6.1 或 Win32 编写的现有桌面应用发布到通用 Windows 平台 (UWP)。 你可以在无人参与（无提示）模式下通过转换器运行桌面安装程序，并获取可在开发计算机上使用 Add-AppxPackage PowerShell cmdlet 安装的 AppX 程序包。

Desktop App Converter 现已在 [Windows 应用商店](https://aka.ms/converter) 中提供。

转换器使用作为转换器下载的一部分提供的干净基本映像在隔离的 Windows 环境中运行桌面安装程序。 它捕获桌面安装程序进行的任何注册表和文件系统 I/O，并将其作为输出的一部分打包。 转换器输出一个带有程序包标识和调用各种 WinRT API 的功能的 AppX。

## <a name="whats-new"></a>新增功能

本部分概述 Desktop App Converter 的各版本之间的更改。 

### <a name="12142016-v104"></a>12/14/2016 (v1.0.4)

* 改进了基本映像验证，可检查无效的 .wim 文件。 
* 针对 ```-Publisher``` 参数中特殊字符的 bug 修复。 
* 更新的资源。

### <a name="1122016-v101"></a>11/2/2016 (v1.0.1)

* 改进了清单架构验证。 
* 改进了错误消息。 
* 添加了受支持 Windows 版本的验证。 
* 注册表筛选器测试的 bug 修复。

### <a name="9142016-v10"></a>9/14/2016 (v1.0)

* Desktop App Converter 现可在 [Windows 应用商店](https://aka.ms/converter) 中下载！ 
* 在[下载中心](https://aka.ms/converterimages)获取最新的 Windows 10 基本映像 (.wim) 以供与 DAC 一起使用。
* 通过应用商店应用，你现在可以使用新入口点 *DesktopAppConverter.exe <arguments>* 在提升的命令提示符下或 PowerShell 窗口中从任意位置运行转换器。  


### <a name="922016-v0125"></a>9/2/2016 (v0.1.25)

* 集成了最新的 dotnet-computervirtualization NuGet 包。
* 在 common.dll 上添加了新引入的依赖项。
* 多个 Bug 修复。

### <a name="842016-v0124"></a>8/4/2016 (v0.1.24)

* 添加了对自动签署 DAC 生成的已转换应用以供测试的支持。 查看 ```–Sign``` 标识，试一试。 
* 添加了虚拟注册表配置单元中的 COM 注册在打包的 AppX 内不受支持时的警告。  
* 添加了对在 VC++ 库上自动检测应用依赖项并随后将其转换为 AppX 清单依赖项的支持。 请注意，为了使用 VC++ 运行时旁加载和测试应用，你需要下载 VCLib 框架包，如博客文章[在 Centennial 项目中使用 Visual C++ 运行时](https://blogs.msdn.microsoft.com/vcblog/2016/07/07/using-visual-c-runtime-in-centennial-project)所述。 在你的计算机上的文件夹 ```Program Files (x86)\Microsoft SDKs\Windows Kits\10\ExtensionSDKs\Microsoft.VCLibs.Desktop``` 下查找程序包、导航到你所依赖的版本（例如 11.0、12.0、14.0），然后双击相应的体系结构程序包（x64、x86）进行安装。
* 更新了清单框架以与 Windows 10 周年更新 (10.0.14393.0) 一致。 
* 多个 Bug 修复和改进的输出布局。 

### <a name="772016-v0122"></a>7/7/2016 (v0.1.22)

* 添加了对从桌面应用程序自动检测外壳扩展以及在 UWP 程序包的 AppXManifest 中声明这些扩展的支持。 若要了解有关桌面扩展的详细信息，请参阅[**已转换的桌面应用扩展**](desktop-to-uwp-extensions.md)。 
* 改进了大量应用的 AppExecutable 检测。 

### <a name="6162016-v0120"></a>6/16/2016 (v0.1.20)

* 解决了最新 Windows 10 Insider Preview 版本上阻碍成功转换的问题。 
* 使用 ```–PackageArch``` 替换了 ```–CreateX86Package```，这允许你指定生成的程序包的体系结构。 

### <a name="682016"></a>6/8/2016

* 添加了对在运行转换器的 AMD64 主机上生成 x86 appx 程序包的支持。
* 通过删除所有以前扩展的基本映像降低了磁盘空间使用率。
* 添加了对清除临时文件和所有不必要的基本映像的支持。
* 改进了对检测文件类型和协议关联的支持。
* 改进了用于检测大量应用的 AppExecutable 属性的逻辑。
* 添加了对为基于 MSI 的安装程序提供其他 –InstallerArguments 的支持。
* 针对转换过程中所有 PathTooLongException 错误的 bug 修复。

### <a name="5122016"></a>5/12/2016

- 还原了对 Windows 专业版的支持。 
- 转换器 ```-Setup``` 标志现在启用 Windows 容器功能，并处理基本映像扩展。 从提升的 PowerShell 提示符运行以下命令来执行一次性设置： ```PS C:\> .\DesktopAppConverter.ps1 -Setup -BaseImage BaseImage-12345.wim -Verbose```
- 添加了应用安装路径的自动检测并将应用程序根移到 VFS 之外，以在运行时减少任何不必要的文件系统重定向。
- 添加了作为转换过程的一部分的扩展基本映像的自动检测。
- 添加了文件类型关联和协议的自动检测。
- 改进了检测“开始”菜单快捷方式的逻辑。
- 改进了文件系统筛选以保留应用安装的 MUI 文件。
- 在清单中更新了最低受支持桌面版本 (10.0.14342.0)。

## <a name="system-requirements"></a>系统要求

### <a name="operating-system"></a>操作系统

+ Windows 10 周年更新（10.0.14393.0 和更高版本）专业版或企业版。

### <a name="hardware-configuration"></a>硬件配置

+ 64 位 (x64) 处理器
+ 硬件辅助虚拟化
+ 二级地址转换 (SLAT)

### <a name="required-resources"></a>所需资源

+ [适用于 Windows 10 的 Windows 软件开发工具包 (SDK)](https://go.microsoft.com/fwlink/?linkid=821375)

## <a name="set-up-the-desktop-app-converter"></a>设置 Desktop App Converter

Desktop App Converter 依靠最新的 Windows 10 功能。 请确保你使用的是 Windows 10 周年更新 (14393.0) 或更高版本。

1.  [从 Windows 应用商店下载 StoreDesktopAppConverter](https://aka.ms/converter) 和[与你的版本匹配的基本映像 .wim 文件](https://aka.ms/converterimages)。  
2.  以管理员身份运行 DesktopAppConverter。 可从“开始”菜单执行此操作，方法是右键单击磁贴并从“更多”下选择“以管理员身份运行”，或者从任务栏执行此操作，方法是右键单击磁贴、第二次右键单击弹出的应用名称，然后选择“以管理员身份运行”。
3.  从应用控制台窗口中，运行 ```Set-ExecutionPolicy bypass```。
4.  通过从控制台窗口运行 ```DesktopAppConverter.exe -Setup -BaseImage .\BaseImage-1XXXX.wim -Verbose``` 来设置转换器。
5.  如果运行以前的命令后提示你重启，请重新启动计算机。

## <a name="run-the-desktop-app-converter"></a>运行 Desktop App Converter

+ **应用商店下载**：使用 ```DesktopAppConverter.exe``` 运行转换器。

### <a name="usage"></a>用法

```CMD
DesktopAppConverter.exe
-Installer <String> [-InstallerArguments <String>] [-InstallerValidExitCodes <Int32>]
-Destination <String>
-PackageName <String>
-Publisher <String>
-Version <Version>
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

### <a name="example"></a>示例

以下示例介绍如何将 *&lt;publisher_name&gt;* 发布的名为 *MyApp* 的桌面应用转换为 UWP 软件包 (AppX)。

```CMD
DesktopAppConverter.exe -Installer C:\Installer\MyApp.exe 
-InstallerArguments "/S" -Destination C:\Output\MyApp -PackageName "MyApp" 
-Publisher "CN=<publisher_name>" -Version 0.0.0.1 -MakeAppx -Verbose
```

## <a name="deploy-your-converted-appx"></a>部署已转换的 AppX

在 PowerShell 中使用 [Add-AppxPackage](https://technet.microsoft.com/library/hh856048.aspx) cmdlet 将已签名的应用包 (.appx) 部署到用户帐户。 

可以使用 Desktop App Converter (v0.1.24) 中的 ```-Sign``` 标志对已转换应用进行自动签名。 此外，若要了解如何对 AppX 程序包进行自动签名，请参阅[对已转换的桌面应用进行签名](desktop-to-uwp-signing.md)。

你还可以利用 Add-AppXPackage PowerShell cmdlet 的 ```-Register``` 参数，在开发过程中从已解压缩文件的文件夹中进行安装。 

有关部署和调试已转换应用的详细信息，请参阅[部署和调试转换的 UWP 应用](desktop-to-uwp-deploy-and-debug.md)。 

## <a name="sign-your-appx-package"></a>对 .Appx 程序包进行签名

Add-AppxPackage cmdlet 要求必须对要部署的应用程序包 (.appx) 进行签名。 使用作为转换器命令行的一部分的 ```-Sign``` 标志或 Microsoft Windows 10 SDK 附带的 SignTool.exe 对 .appx 程序包进行签名。

有关如何对.appx 程序包进行签名的其他详细信息，请参阅 [对已转换的桌面应用进行签名](desktop-to-uwp-signing.md)。 

注意：如果你尝试使用自动生成的证书对程序包进行签名，你将需要使用默认密码“123456”。

## <a name="modify-vfs-folder-and-registry-hive-optional"></a>修改 VFS 文件夹和注册表实体文件（可选）

Desktop App Converter 采用非常保守的方法来筛选掉容器中的文件和系统干扰。  对此不作要求，但是转换后，你可以：

1. 查看 VFS 文件夹并删除安装程序不需要的任何文件。
2. 查看 Reg.dat 的内容并删除你的应用未安装/不需要的任何密钥。

如果你对转换的应用（包括上述应用）进行任何更改，不需要再次运行转换器，你可以使用 MakeAppx 工具和 DAC 为你的应用生成的 appxmanifest.xml 文件手动重新打包应用。 有关帮助，请参阅 [使用桌面桥手动将你的应用转换为 UWP](desktop-to-uwp-manual-conversion.md)。

## <a name="caveats"></a>警告

1. 主机上的 Windows 10 版本必须与作为 Desktop App Converter 下载的一部分获取的基本映像匹配。  
2. 确保桌面安装程序是独立的目录，因为转换器会将目录的所有内容复制到隔离的 Windows 环境中。  
3. 当前，Desktop App Converter 仅支持在 64 位操作系统上运行转换过程。 你仅可以将转换的.appx 程序包部署到 64 位 (x64) 操作系统。  
4. Desktop App Converter 需要桌面安装程序在无人参与模式下运行。 确保使用 *-InstallerArguments* 参数将安装程序的无提示标志传递到转换器。
5. 发布公共 SxS Fusion 程序集不起作用。 在安装期间，应用程序可以发布公共并行 Fusion 程序集，这些程序集可供任何其他进程访问。 在进程激活上下文创建期间，这些程序集将由名为 CSRSS.exe 的系统进程检索。 当已转换的进程执行此操作时，这些程序集的激活上下文创建和模块加载将失败。 内置程序集（如 ComCtl）随操作系统附带，因此获取它们的依赖项很安全。 SxS Fusion 程序集在以下位置注册：
  + 注册表： `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\SideBySide\Winners`
  + 文件系统：%windir%\\SideBySide

## <a name="known-issues"></a>已知问题

* 我们目前正在调查某些操作系统版本上出现的以下错误：
    
    * ```E_CREATTING_ISOLATED_ENV_FAILED```
    * ```E_STARTING_ISOLATED_ENV_FAILED```
    
    如果遇到任一错误，请确保使用的是从[下载中心](https://aka.ms/converterimages)下载的有效基本映像。 如果你使用的是有效的 .wim，请向我们的以下地址发送你的日志来帮助我们进行调查：converter@microsoft.com。 

* 如果你在之前已安装 Desktop App Converter 的开发人员计算机上收到 Windows 预览体验成员外部测试版，你可能会在设置新的基本映像时收到错误 `New-ContainerNetwork: The object already exists`。 作为解决方法，请从提升的命令提示符运行命令 `Netsh int ipv4 reset`，然后重启你的计算机。 

* 如果主可执行文件或任何依赖项放置在“Program Files”或“Windows\System32”下，使用“AnyCPU”版本选项编译的 .NET 应用将无法安装。 作为解决方法，请使用特定于体系结构的桌面安装程序（32 位或 64 位）成功生成 AppX 程序包。

## <a name="telemetry-from-desktop-app-converter"></a>来自 Desktop App Converter 的遥测

Desktop App Converter 可以收集关于你和你使用该软件的情况的信息，并将此信息发送给 Microsoft。 你可以在产品文档和 [Microsoft 隐私声明](http://go.microsoft.com/fwlink/?LinkId=521839)中了解有关 Microsoft 的数据收集和使用的详细信息。 你同意遵守 Microsoft 隐私声明的所有适用条款。

默认情况下，将为 Desktop App Converter 启用遥测。 添加以下注册表项以将遥测配置为所需设置：  

```cmd
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\DesktopAppConverter
```
+ 通过使用设置为 1 的 DWORD 来添加或编辑 *DisableTelemetry* 值。
+ 若要启用遥测，请删除该项或将值设置为 0。

## <a name="desktop-app-converter-usage"></a>Desktop App Converter 用法

下面是 Desktop App Converter 的参数列表。 还可通过运行以下内容查看此列表：   

```CMD
Get-Help DesktopAppConverter.exe -detailed
```

### <a name="setup-parameters"></a>Setup 参数  

|参数|说明|
|---------|-----------|
|```-Setup [<SwitchParameter>]``` | 在设置模式下运行 DesktopAppConverter。 设置模式支持扩展所提供的基本映像。|
|```-BaseImage <String>``` | 未扩展的基本映像的完整路径。 如果指定 -Setup，则需要此参数。|
|```-LogFile <String>``` [可选] | 指定日志文件。 如果省略，将创建一个日志文件临时位置。|
|```-NatSubnetPrefix <String>``` [可选] | 用于 Nat 实例的前缀值。 通常，仅在主机连接到与转换器的 NetNat 相同的子网范围时，你会希望更改此值。 你可以通过使用 **Get-NetNat** cmdlet 查询当前转换器 NetNat 配置。 |
|```-NoRestart [<SwitchParameter>]``` | 在运行设置时不要提示重启（需要重启才能启用容器功能）。 |

### <a name="conversion-parameters"></a>Conversion 参数  

|参数|说明|
|---------|-----------|
|```-AppInstallPath <String> [optional]``` | 应用程序针对已安装文件（如果已安装）的根文件夹的完整路径（例如“C:\Program Files (x86)\MyApp”）。| 
|```-Destination <String>``` | 如果转换器的 appx 输出的所需目标尚未存在，DesktopAppConverter 可以创建此位置。|
|```-Installer <String>``` | 应用程序的安装程序的路径 - 必须能够在无人参与/无提示的情况下运行。|
|```-InstallerArguments <String>``` [可选] | 用于强制安装程序在无人参与/无提示的情况下运行的参数的逗号分隔列表或字符串。 如果安装程序是 msi，则此参数为可选参数。 若要从安装程序中获取日志，请在此处为安装程序提供日志记录参数，并使用路径 ```<log_folder>```，该路径是转换器替换为相应路径的标记。 <br><br>**注意：无人参与/无提示标志和日志参数将因安装程序技术而有所不同。** <br><br>此参数的用法示例：```-InstallerArguments "/silent /log <log_folder>\install.log"```另一个不产生日志文件的示例可能如下所示：```-InstallerArguments "/quiet", "/norestart"```同样，如果你希望转换器捕获日志并将其放置在最终日志文件夹中，你必须逐字节地将任何日志直接指向标记路径 ```<log_folder>```。|
|```-InstallerValidExitCodes <Int32>``` [可选] | 指示安装程序成功运行的退出代码的逗号分隔列表（例如，0, 1234, 5678）。  默认情况下，对于非 msi，它为 0，对于 msi，它为 0, 1641, 3010。|

### <a name="appx-identity-parameters"></a>Appx Identity 参数  

|参数|说明|
|---------|-----------|
|```-PackageName <String>``` | 通用 Windows 应用程序包的名称
|```-Publisher <String>``` | 通用 Windows 应用程序包的发布者
|```-Version <Version>``` | 通用 Windows 应用程序包的版本号

### <a name="optional-appx-manifest-parameters"></a>可选 Appx Manifest 参数  

|参数|说明|
|---------|-----------|
|```-AppExecutable <String> [optional]``` [可选] | 你的应用程序的主可执行文件的名称（“MyApp.exe”）。 |
|```-AppFileTypes <String>``` [可选] | 应用程序将与其关联的文件类型的逗号分隔列表（例如 “.txt, .doc”，不包含引号）。|
|```-AppId <String>``` [可选] | 在 appx 清单中指定要将 Application Id 设置为的值。 如果未指定，则将其设置为 *PackageName* 传入的值。|
|```-AppDisplayName <String>``` [可选] | 在 appx 清单中指定要将应用程序显示名称设置为的值。 如果未指定，则将其设置为 *PackageName* 传入的值。 |
|```-AppDescription <String>``` [可选] | 在 appx 清单中指定要将应用程序描述设置为的值。 如果未指定，则将其设置为 *PackageName* 传入的值。|
|```-PackageDisplayName <String>``` [可选] | 在 appx 清单中指定要将程序包显示名称设置为的值。 如果未指定，则将其设置为 *PackageName* 传入的值。 |
|```-PackagePublisherDisplayName <String>``` [可选] | 在 appx 清单中指定要将程序包发布者显示名称设置为的值。 如果未指定，则将其设置为 *Publisher* 传入的值。 |

### <a name="other-conversion-parameters"></a>其他 Conversion 参数  

|参数|说明|
|---------|-----------|
|```-ExpandedBaseImage <String>``` [可选] | 已扩展的基本映像的完整路径。|
|```-MakeAppx [<SwitchParameter>]``` [可选] | 一个告知此脚本对输出调用 MakeAppx 的开关（当存在时）。 |
|```-LogFile <String>``` [可选] | 指定日志文件。 如果省略，将创建一个日志文件临时位置。 |
| ```Sign [<SwitchParameter>] [optional]``` | 指示此脚本对输出 appx 进行签名。 此开关应该位于开关 ```-MakeAppx``` 旁边。 
|```<Common parameters>``` | 此 cmdlet 支持通用参数：*Verbose*、*Debug*、*ErrorAction*、*ErrorVariable*、*WarningAction*、*WarningVariable*、*OutBuffer*、*PipelineVariable* 和 *OutVariable*。 有关详细信息，请参阅 [about_CommonParameters](http://go.microsoft.com/fwlink/?LinkID=113216)。 |

### <a name="cleanup-parameters"></a>清除参数

|参数|说明|
|---------|-----------|
|```Cleanup [<Option>]``` | 为 DesktopAppConverter 项目运行清除。 清除模式有 3 个有效的选项。 |
|```Cleanup All``` | 删除所有已扩展的基本映像、删除任何临时转换器文件、删除容器网络并禁用可选的 Windows 功能，即容器。 |
|```Cleanup WorkDirectory``` | 删除所有临时转换器文件。 |
|```Cleanup ExpandedImage``` | 删除安装在主机上的所有已扩展的基本映像。 |

### <a name="package-architecture"></a>程序包体系结构

Desktop App Converter 现在支持创建可在 x86 和 amd64 计算机上安装和运行的 x86 和 x64 应用包。 请注意，Desktop App Converter 仍然需要在 AMD64 计算机上运行才能执行成功的转换。

|参数|说明|
|---------|-----------|
|```-PackageArch <String>``` | 生成指定了体系结构的程序包。 有效选项为“x86”或“x64”，例如 -PackageArch x86。 该参数为可选参数。 如果未指定，DesktopAppConverter 将尝试自动检测程序包体系结构。 如果自动检测失败，它将默认为 x64 程序包。 

### <a name="running-the-peheadercertfixtool"></a>运行 PEHeaderCertFixTool

在转换过程中，DesktopAppConverter 将自动运行 PEHeaderCertFixTool，以便修复任何损坏的 PE 标头。 但是，还可以在 UWP appx、松散文件或指定二进制文件上运行 PEHeaderCertFixTool。 示例用法： 

```CMD
PEHeaderCertFixTool.exe <binary file>|<.appx package>|<folder> [/c] [/v]
 /c   -- check for corrupted certificate but do not fix (optional)
 /v   -- verbose (optional)
example1: PEHeaderCertFixTool app.exe
example2: PEHeaderCertFixTool c:\package.appx /c
example3: PEHeaderCertFixTool c:\myapp /c /v
```

## <a name="language-support"></a>语言支持

Desktop App Converter 不支持 Unicode；因此，没有用于该工具的任何中文字符或非 ASCII 字符。

## <a name="see-also"></a>另请参阅

+ [使用 Desktop App Converter 将桌面应用发布到 UWP](https://channel9.msdn.com/events/Build/2016/P504)
+ [Project Centennial：将现有桌面应用程序发布到通用 Windows 平台](https://channel9.msdn.com/events/Build/2016/B829)  


<!--HONumber=Dec16_HO3-->


