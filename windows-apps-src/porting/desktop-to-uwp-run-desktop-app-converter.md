---
author: awkoren
Description: "运行桌面转换器应用以将 Windows 桌面应用程序（如 Win32、WPF 和 Windows 窗体）转换为通用 Windows 平台 (UWP) 应用。"
Search.Product: eADQiWindows 10XVcnh
title: "桌面应用转换器预览 (Project Centennial)"
ms.sourcegitcommit: 07016fabb8b49e57dd0ae4ef68447451d31aa2dc
ms.openlocfilehash: bc28197cccc0559f57abc8cb81e23bf241ca3716

---

# 桌面应用转换器预览 (Project Centennial)

\[有些信息与可能在商业发行之前就经过实质性修改的预发布产品相关。 Microsoft 不对此处提供的信息作任何明示或默示的担保。\]

[获取桌面应用转换器。](http://go.microsoft.com/fwlink/?LinkId=785437)

桌面应用转换器是一款预发布工具，可使你将为 .NET 4.6.1 或 Win32 编写的现有桌面应用发布到通用 Windows 平台 (UWP)。 你可以在无人参与（无提示）模式下通过转换器运行桌面安装程序，并获取可在开发计算机上使用 Add-AppxPackage PowerShell cmdlet 安装的 AppX 程序包。

转换器使用作为转换器下载的一部分提供的干净基本映像在隔离的 Windows 环境中运行桌面安装程序。 它捕获桌面安装程序进行的任何注册表和文件系统 I/O，并将其作为输出的一部分打包。 转换器输出一个带有程序包标识和调用各种 WinRT API 的功能的 AppX。

## 新增功能

本部分概述桌面应用转换器的各版本之间的更改。 

### 6/16/2016

* 桌面应用转换器 (v0.1.20) 可以解决最新 Windows 10 Insider Preview 版本上任何阻碍成功转换的问题。 
* 使用 ```–PackageArch``` 替换了 ```–CreateX86Package```，这允许你指定生成的程序包的体系结构。 

### 6/8/2016

* 添加了对在运行转换器的 AMD64 主机上生成 x86 appx 程序包的支持。
* 通过删除所有以前扩展的基本映像降低了磁盘空间使用率。
* 添加了对清除临时文件和所有不必要的基本映像的支持。
* 改进了对检测文件类型和协议关联的支持。
* 改进了用于检测大量应用的 AppExecutable 属性的逻辑。
* 添加了对为基于 MSI 的安装程序提供其他 –InstallerArguments 的支持。
* 针对转换过程中所有 PathTooLongException 错误的 bug 修复。

### 5/12/2016

- 还原了对 Windows 专业版的支持。 
- 转换器 ```-Setup``` 标志现在启用 Windows 容器功能，并处理基本映像扩展。 从提升的 PowerShell 提示符运行以下命令来执行一次性设置： ```PS C:\> .\DesktopAppConverter.ps1 -Setup -BaseImage BaseImage-12345.wim -Verbose```
- 添加了应用安装路径的自动检测并将应用程序根移到 VFS 之外，以在运行时减少任何不必要的文件系统重定向。
- 添加了作为转换过程的一部分的扩展基本映像的自动检测。
- 添加了文件类型关联和协议的自动检测。
- 改进了检测“开始”菜单快捷方式的逻辑。
- 改进了文件系统筛选以保留应用安装的 MUI 文件。
- 在清单中更新了 Project Centennial 的最低受支持桌面版本 (10.0.14342.0)。

## 系统要求

### 支持的操作系统
+ Windows 10 周年更新企业版预览（内部版本 10.0.14342.0 和更高版本）

### 所需的硬件配置

你的计算机必须具有以下最低功能：
+ 64 位 (x64) 处理器
+ 硬件辅助虚拟化
+ 二级地址转换 (SLAT)

### 推荐资源
+ [适用于 Windows 10 的 Windows 软件开发工具包 (SDK)](https://developer.microsoft.com/en-us/windows/downloads/windows-10-sdk)

## 设置桌面应用转换器   
桌面应用转换器依赖于作为 Windows Insider Preview 版本进行外部测试的 Windows 10 功能。 确保你使用最新版本来利用转换器。

1. 确保你具有最新的 Windows 10 Insider Preview 操作系统 - 企业版或专业版 (http://insider.windows.com)。 
2. 下载 DesktopAppConverter.zip 和与你的 Insider Preview 版本匹配的基本映像 .wim 文件 (http://aka.ms/converter)。 
3. 将 DesktopAppConverter.zip 解压缩到本地文件夹。
4. 从管理员 PowerShell 窗口：  
```CMD
PS C:\> Set-ExecutionPolicy bypass
```
5. 从管理员 PowerShell 窗口运行以下命令来设置转换器：
```CMD
PS C:\> .\DesktopAppConverter.ps1 -Setup -BaseImage .\BaseImage-1XXXX.wim -Verbose
```
6. 如果运行以前的命令后提示你重启，请重启计算机并再次运行该命令。

## 运行桌面应用转换器
桌面应用转换器具有两个入口点：PowerShell 和 Command Shell。 你可以使用这些入口点之一启动转换过程。

### 用法
```CMD
DesktopAppConverter.ps1
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

### 示例
以下示例介绍如何将 *&lt;publisher_name&gt;* 发布的名为 *MyApp* 的桌面应用转换为 UWP 程序包 (AppX)。

+ 从管理员 PowerShell 窗口，运行以下命令：
```CMD
PS C:\>.\DesktopAppConverter.ps1 -Installer C:\Installer\MyApp.exe 
-InstallerArguments "/S" -Destination C:\Output\MyApp -PackageName "MyApp" 
-Publisher "CN=<publisher_name>" -Version 0.0.0.1 -MakeAppx -Verbose
```

## 部署转换的 AppX
在 PowerShell 中使用 [Add-AppxPackage](https://technet.microsoft.com/en-us/library/hh856048.aspx) cmdlet 将已签名的应用包 (.appx) 部署到用户帐户。 若要对 .appx 程序包进行签名，请参考以下部分“对 .Appx 程序包进行签名”。 此外，你可以包含该 cmdlet 的 *Register* 参数，以在开发过程中从已解压缩文件的文件夹中进行安装。 有关详细信息，请参阅[部署和调试转换的 UWP 应用](desktop-to-uwp-deploy-and-debug.md)。

## 对 .Appx 程序包进行签名

Add-AppxPackage cmdlet 要求必须对要部署的应用程序包 (.appx) 进行签名。 使用 Microsoft Windows 10 SDK 附带的 SignTool.exe 对 .appx 程序包进行签名。

### 示例
```CMD
C:\> MakeCert.exe -r -h 0 -n "CN=<publisher_name>" -eku 1.3.6.1.5.5.7.3.3 -pe -sv <my.pvk> <my.cer>
C:\> pvk2pfx.exe -pvk <my.pvk> -spc <my.cer> -pfx <my.pfx>
C:\> signtool.exe sign -f <my.pfx> -fd SHA256 -v .\<outputAppX>.appx
```
**注意：**当你运行 MakeCert.exe 并且系统要求你输入密码时，请选择“无”****。

有关证书和签名的详细信息，请参阅：

+ [操作方法：创建在部署期间使用的临时证书](https://msdn.microsoft.com/library/ms733813.aspx)
+ [SignTool](https://msdn.microsoft.com/library/windows/desktop/aa387764.aspx)
+ [SignTool.exe（签名工具）](https://msdn.microsoft.com/library/8s9b9yaz.aspx)

### 警告
1. 主机上的 Windows 10 版本必须与作为桌面应用转换器下载的一部分获取的基本映像匹配。  
2. 确保桌面安装程序是独立的目录，因为转换器会将目录的所有内容复制到隔离的 Windows 环境中。  
3. 当前，桌面应用转换器仅支持在 64 位操作系统上运行转换过程。 你仅可以将转换的.appx 程序包部署到 64 位 (x64) 操作系统。  
4. 桌面应用转换器需要桌面安装程序在无人参与模式下运行。 确保使用 *-InstallerArguments* 参数将安装程序的无提示标志传递到转换器。
5. 发布公共 SxS Fusion 程序集不起作用。 在安装期间，应用程序可以发布公共并行 Fusion 程序集，这些程序集可供任何其他进程访问。 在进程激活上下文创建期间，这些程序集将由名为 CSRSS.exe 的系统进程检索。 当 Centennial 进程执行此操作时，这些程序集的激活上下文创建和模块加载将失败。 内置程序集（如 ComCtl）随操作系统附带，因此从 Centennial 进程获取它们的依赖项很安全。 SxS Fusion 程序集在以下位置注册：
  + 注册表： `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\SideBySide\Winners`
  + 文件系统：%windir%\\SideBySide

## 已知问题

+ 如果你在之前已安装桌面应用转换器预览的开发人员计算机上收到 Windows 预览体验成员外部测试版，你可能会在设置新的基本映像时收到错误 `New-ContainerNetwork: The object already exists`。 作为解决方法，请从提升的命令提示符运行命令 `Netsh int ipv4 reset`，然后重启你的计算机。 
+ 如果主可执行文件或任何依赖项放置在“Program Files”或“Windows\System32”下，使用“AnyCPU”版本选项编译的 .NET 应用将无法安装。 作为解决方法，请使用特定于体系结构的桌面安装程序（32 位或 64 位）成功生成 AppX 程序包。

## 来自桌面应用转换器的遥测  
桌面应用转换器可以收集关于你和你使用该软件的情况的信息，并将此信息发送给 Microsoft。 你可以在产品文档和 [Microsoft 隐私声明](http://go.microsoft.com/fwlink/?LinkId=521839)中了解有关 Microsoft 的数据收集和使用的详细信息。 你同意遵守 Microsoft 隐私声明的所有适用条款。

默认情况下，将为桌面应用转换器启用遥测。 添加以下注册表项以将遥测配置为所需设置：  
```CMD
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\DesktopAppConverter
```
+ 通过使用设置为 1 的 DWORD 来添加或编辑 *DisableTelemetry* 值。
+ 若要启用遥测，请删除该项或将值设置为 0。

## 桌面应用转换器用法
下面是桌面应用转换器的参数列表。 你还可以通过运行以下命令在 Windows Powershell 窗口中查看此列表：  
```CMD
get-help .\DesktopAppConverter.ps1 -detailed
```

### Setup 参数  
|参数|说明|
|---------|-----------|
|```-Setup [<SwitchParameter>]``` | 在设置模式下运行 DesktopAppConverter。 设置模式支持扩展所提供的基本映像。|
|```-BaseImage <String>``` | 未扩展的基本映像的完整路径。 如果指定 -Setup，则需要此参数。|
|```-LogFile <String>``` [可选] | 指定日志文件。 如果省略，将创建一个日志文件临时位置。|
|```-NatSubnetPrefix <String>``` [可选] | 用于 Nat 实例的前缀值。 通常，仅在主机连接到与转换器的 NetNat 相同的子网范围时，你会希望更改此值。 你可以通过使用 **Get-NetNat** cmdlet 查询当前转换器 NetNat 配置。 |
|```-NoRestart [<SwitchParameter>]``` | 在运行设置时不要提示重启（需要重启才能启用容器功能）。 |

### Conversion 参数  
|参数|说明|
|---------|-----------|
|```-Installer <String>``` | 应用程序的安装程序的路径 - 必须能够在无人参与/无提示的情况下运行。|
|```-InstallerArguments <String>``` [可选] | 用于强制安装程序在无人参与/无提示的情况下运行的参数的逗号分隔列表或字符串。 如果安装程序是 msi，则此参数为可选参数。 若要从安装程序中获取日志，请在此处为安装程序提供日志记录参数，并使用路径 ```<log_folder>```，该路径是转换器替换为相应路径的标记。 <br><br>**注意：无人参与/无提示标志和日志参数将因安装程序技术而有所不同。** <br><br>此参数的用法示例：```-InstallerArguments "/silent /log <log_folder>\install.log"```另一个不产生日志文件的示例可能如下所示：```-InstallerArguments "/quiet", "/norestart"```同样，如果你希望转换器捕获日志并将其放置在最终日志文件夹中，你必须逐字节地将任何日志直接指向标记路径 ```<log_folder>```。|
|```-InstallerValidExitCodes <Int32>``` [可选] | 指示安装程序成功运行的退出代码的逗号分隔列表（例如，0, 1234, 5678）。  默认情况下，对于非 msi，它为 0，对于 msi，它为 0, 1641, 3010。|
|```-Destination <String>``` | 如果转换器的 appx 输出的所需目标尚未存在，DesktopAppConverter 可以创建此位置。|

### Appx Identity 参数  
|参数|描述|
|---------|-----------|
|```-PackageName <String>``` | 通用 Windows 应用程序包的名称
|```-Publisher <String>``` | 通用 Windows 应用程序包的发布者
|```-Version <Version>``` | 通用 Windows 应用程序包的版本号

### 可选 Appx Manifest 参数  
|参数|描述|
|---------|-----------|
|```-AppExecutable <String>``` [可选] | 应用程序的主可执行文件的完整路径（如果要安装，但并非必须安装），例如“C:\Program Files (x86)\MyApp\MyApp.exe”。|
|```-AppFileTypes <String>``` [可选] | 应用程序将与其关联的文件类型的逗号分隔列表（例如 “.txt, .doc”，不包含引号）。|
|```-AppId <String>``` [可选] | 在 appx 清单中指定要将 Application Id 设置为的值。 如果未指定，则将其设置为 *PackageName* 传入的值。|
|```-AppDisplayName <String>``` [可选] | 在 appx 清单中指定要将应用程序显示名称设置为的值。 如果未指定，则将其设置为 *PackageName* 传入的值。 |
|```-AppDescription <String>``` [可选] | 在 appx 清单中指定要将应用程序描述设置为的值。 如果未指定，则将其设置为 *PackageName* 传入的值。|
|```-PackageDisplayName <String>``` [可选] | 在 appx 清单中指定要将程序包显示名称设置为的值。 如果未指定，则将其设置为 *PackageName* 传入的值。 |
|```-PackagePublisherDisplayName <String>``` [可选] | 在 appx 清单中指定要将程序包发布者显示名称设置为的值。 如果未指定，则将其设置为 *Publisher* 传入的值。 |

### 其他 Conversion 参数  
|参数|描述|
|---------|-----------|
|```-ExpandedBaseImage <String>``` [可选] | 已扩展的基本映像的完整路径。|
|```-MakeAppx [<SwitchParameter>]``` [可选] | 一个告知此脚本对输出调用 MakeAppx 的开关（当存在时）。 |
|```-LogFile <String>``` [可选] | 指定日志文件。 如果省略，将创建一个日志文件临时位置。 |
|```<Common parameters>``` | 此 cmdlet 支持通用参数：*Verbose*、*Debug*、*ErrorAction*、*ErrorVariable*、*WarningAction*、*WarningVariable*、*OutBuffer*、*PipelineVariable* 和 *OutVariable*。 有关详细信息，请参阅 [about_CommonParameters](http://go.microsoft.com/fwlink/?LinkID=113216)。 |

### 清除参数
|参数|说明|
|---------|-----------|
|```Cleanup [<Option>]``` | 为 DesktopAppConverter 项目运行清除。 清除模式有 3 个有效的选项。 |
|```Cleanup All``` | 删除所有已扩展的基本映像、删除任何临时转换器文件、删除容器网络并禁用可选的 Windows 功能，即容器。 |
|```Cleanup WorkDirectory``` | 删除所有临时转换器文件。 |
|```Cleanup ExpandedImages``` | 删除安装在主机上的所有已扩展的基本映像。 |

### 程序包体系结构
桌面应用转换器预览现在支持创建可在 x86 和 amd64 计算机上安装和运行的 x86 和 x64 应用包。 请注意，桌面应用转换器仍然需要在 AMD64 计算机上运行才能执行成功的转换。

|参数|描述|
|---------|-----------|
|```-PackageArch <String>``` | 生成指定了体系结构的程序包。 有效选项为“x86”或“x64”，例如 -PackageArch x86。 该参数为可选参数。 如果未指定，DesktopAppConverter 将尝试自动检测程序包体系结构。 如果自动检测失败，它将默认为 x64 程序包。 |

## 另请参阅
+ [获取桌面应用转换器](http://go.microsoft.com/fwlink/?LinkId=785437)
+ [将你的桌面应用发布到通用 Windows 平台](https://developer.microsoft.com/en-us/windows/bridges/desktop)
+ [使用桌面应用转换器将桌面应用发布到 UWP](https://channel9.msdn.com/events/Build/2016/P504)
+ [Project Centennial：将现有桌面应用程序发布到通用 Windows 平台](https://channel9.msdn.com/events/Build/2016/B829)  
+ [适用于 Desktop Bridge 的 UserVoice (Project Centennial)](http://aka.ms/UserVoiceDesktopToUwp)
+ [GitHub 上的 UWP 代码示例的桌面应用桥](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)



<!--HONumber=Jun16_HO4-->


