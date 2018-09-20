---
author: normesta
Description: Fix issues that prevent your desktop application from running in an MSIX container
Search.Product: eADQiWindows 10XVcnh
title: 修复阻止 MSIX 容器中运行桌面应用程序的问题
ms.author: normesta
ms.date: 07/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7e5119696498156d36ec63b16b1d76c00b03f4df
ms.sourcegitcommit: 4f6dc806229a8226894c55ceb6d6eab391ec8ab6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/20/2018
ms.locfileid: "4091607"
---
# <a name="apply-runtime-fixes-to-an-msix-package-by-using-the-package-support-framework"></a>通过使用程序包支持框架到 MSIX 程序包应用运行时修复

包支持框架是可帮助你修复时应用到你现有的 win32 应用程序不能访问的源代码，以便它可以 MSIX 容器中运行的开源工具包。 包支持框架可帮助你遵循在现代的运行时环境的最佳实践的应用程序。

若要了解详细信息，请参阅[包支持框架](https://docs.microsoft.com/windows/msix/package-support-framework-overview)。

本指南将帮助你确定应用程序兼容性问题，并查找、 应用，并扩展运行时修复解决它们的。

<a id="identify" />

## <a name="identify-packaged-application-compatibility-issues"></a>确定已打包的应用程序兼容性问题

首先，创建用于你的应用程序的程序包。 然后，将它安装、 运行它，并观察其行为。 你可能收到错误消息，以帮助你识别兼容性问题。 你还可以使用[进程监视器](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon)来识别问题。  与工作目录和程序路径访问权限有关的应用程序假设相关的常见问题。

### <a name="using-process-monitor-to-identify-an-issue"></a>使用进程监视器来识别问题

[进程监视器](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon)是一个强大的工具，用于观察应用的文件和注册表操作以及它们的结果。  这可以帮助你了解应用程序兼容性问题。  打开进程监视器后, 添加筛选器 (筛选器 >...筛选器) 包含仅从应用程序可执行文件的事件。

![ProcMon 应用筛选器](images/desktop-to-uwp/procmon_app_filter.png)

将显示的事件列表。 对于其中许多事件，单词**成功**会显示在**结果**列中。

![ProcMon 事件](images/desktop-to-uwp/procmon_events.png)

（可选） 你可以筛选以仅显示仅失败的事件。

![ProcMon 排除成功](images/desktop-to-uwp/procmon_exclude_success.png)

如果你怀疑文件系统访问失败，搜索失败是在 System32/SysWOW64 或包文件路径下的事件。 在这里，太还有助于筛选器。 在此列表的底部的开始菜单和向上滚动。 最近发生故障的显示在此列表的底部。 大多数注意包含诸如拒绝访问，"的字符串的错误和"未找到路径/名称"，并忽略看起来不可疑的操作。 [PSFSample](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/samples/PSFSample/)具有两个问题。 你可以看到在下图中出现的列表中的这些问题。

![ProcMon Config.txt](images/desktop-to-uwp/procmon_config_txt.png)

在第一个问题出现在此图像中，应用程序无法从位于"C:\Windows\SysWOW64"路径"Config.txt"文件中读取。 不太可能在应用程序正在尝试直接引用该路径。 大多数情况下，在尝试从该文件中读取使用的相对路径，并且默认情况下，"System32/SysWOW64"应用程序的工作目录。 这要求应用程序应设置为某个位置封装其当前工作目录。 查找 appx 内，我们可以看到该文件存在可执行文件相同的目录中。

![应用 Config.txt](images/desktop-to-uwp/psfsampleapp_config_txt.png)

在下图中显示第二个问题。

![ProcMon 日志文件](images/desktop-to-uwp/procmon_logfile.png)

在此问题，应用程序无法.log 文件写入其程序包路径。 这会建议文件重定向填充程序可能会帮助。

<a id="find" />

## <a name="find-a-runtime-fix"></a>查找运行时修复

PSF 包含你可以使用，例如文件重定向大幅的运行时修补程序。

### <a name="file-redirection-shim"></a>文件重定向填充程序

你可以使用[文件重定向大幅](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/develop/FileRedirectionShim)重定向写入或读取不可以从 MSIX 容器中运行的应用程序访问的目录中的数据尝试。

例如，如果你的应用程序写入与你的应用程序可执行文件相同的目录中的日志文件，然后可以使用[文件重定向大幅](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/develop/FileRedirectionShim)在另一个位置，如本地应用数据存储中创建该日志文件。

### <a name="runtime-fixes-from-the-community"></a>从社区的运行时修复

请确保查看我们的[GitHub](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/develop)页面的社区贡献。 很可能在其他开发人员解决了类似的问题，并且具有共享的运行时修复程序。

## <a name="apply-a-runtime-fix"></a>应用运行时修复

从 Windows SDK 中，并按照以下步骤，你可以应用现有的运行时修复进行一些简单的工具。

> [!div class="checklist"]
> * 创建程序包布局文件夹
> * 获取的包支持框架文件
> * 将其添加到你的程序包
> * 修改包清单
> * 创建配置文件

我们看一下每个任务。

### <a name="create-the-package-layout-folder"></a>创建程序包布局文件夹

如果你已经有一个.appx 文件，你可以将其内容解包到布局文件夹将用作你的程序包暂存区域。  你可以从**x64 适用于 VS 2017 的本机工具命令提示符**，或手动使用的可执行的搜索路径中的 SDK bin 路径。

```
makeappx unpack /p PSFSamplePackage_1.0.60.0_AnyCPU_Debug.appx /d PackageContents

```

这将为你提供类似以下的。

![程序包布局](images/desktop-to-uwp/package_contents.png)

如果你开始没有.appx 文件，你可以从头开始创建的程序包文件夹和文件。

### <a name="get-the-package-support-framework-files"></a>获取的包支持框架文件

你可以通过使用 Visual Studio 中获取 PSF Nuget 程序包。 你还可以通过使用独立的 Nuget 命令行工具获取它。

#### <a name="get-the-package-by-using-visual-studio"></a>通过使用 Visual Studio 中获取程序包

在 Visual Studio 中，右键单击解决方案或项目节点并选择管理 Nuget 程序包命令之一。  搜索**Microsoft.PackageSupportFramework**或**PSF** Nuget.org 上找到该程序包。然后，安装它。

#### <a name="get-the-package-by-using-the-command-line-tool"></a>可以通过使用命令行工具获取该程序包

从该位置安装 Nuget 命令行工具： https://www.nuget.org/downloads。 然后，从 Nuget 命令行，请运行此命令：

```
nuget install Microsoft.PackageSupportFramework
```

### <a name="add-the-package-support-framework-files-to-your-package"></a>将包支持框架文件添加到程序包

在程序包目录中添加所需的 32 位和 64 位 PSF Dll 和可执行文件。 使用下表作为指南。 你还需要包含所需的任何运行时修复。 在我们的示例中，我们需要文件重定向运行时修复。

| 应用程序可执行文件是 x64 | 应用程序可执行文件是 x86 |
|-------------------------------|-----------|
| [ShimLauncher64.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimLauncher/readme.md) |  [ShimLauncher32.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimLauncher/readme.md) |
| [ShimRuntime64.dll](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimRuntime/readme.md) | [ShimRuntime32.dll](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimRuntime/readme.md) |
| [ShimRunDll64.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimRunDll/readme.md) | [ShimRunDll32.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimRunDll/readme.md) |

你的程序包内容现在应如下所示。

![包二进制文件](images/desktop-to-uwp/package_binaries.png)

### <a name="modify-the-package-manifest"></a>修改包清单

在文本编辑器中，打开你的程序包清单，然后设置`Executable`属性`Application`元素中填充程序启动程序可执行文件的名称。  如果你知道你的目标应用程序的体系结构，选择适当的版本中，ShimLauncher32.exe 或 ShimLauncher64.exe。  如果没有，ShimLauncher32.exe 将可在所有情况下。  下面提供了一个示例。

```xml
<Package ...>
  ...
  <Applications>
    <Application Id="PSFSample"
                 Executable="ShimLauncher32.exe"
                 EntryPoint="Windows.FullTrustApplication">
      ...
    </Application>
  </Applications>
</Package>
```

### <a name="create-a-configuration-file"></a>创建配置文件

创建文件名称``config.json``，并将该文件保存到你的程序包的根文件夹。 修改 config.json 文件的声明的应用 ID，以指向你只需更换的可执行文件。 使用从使用进程监视器获得的知识，你可以还设置的工作目录，以及使用文件重定向大幅重定向到.log 文件的相对于程序包的"PSFSampleApp"目录下的读取/写入。

```json
{
    "applications": [
        {
            "id": "PSFSample",
            "executable": "PSFSampleApp/PSFSample.exe",
            "workingDirectory": "PSFSampleApp/"
        }
    ],
    "processes": [
        {
            "executable": "PSFSample",
            "shims": [
                {
                    "dll": "FileRedirectionShim.dll",
                    "config": {
                        "redirectedPaths": {
                            "packageRelative": [
                                {
                                    "base": "PSFSampleApp/",
                                    "patterns": [
                                        ".*\\.log"
                                    ]
                                }
                            ]
                        }
                    }
                }
            ]
        }
    ]
}
```
下面是 config.json 架构的指南：

| 数组 | key | 值 |
|-------|-----------|-------|
| applications | id |  使用的值`Id`属性`Application`在程序包清单中的元素。 |
| applications | 可执行文件 | 想要启动的可执行文件程序包相对路径。 在大多数情况下，你可以从程序包清单文件获取此值之前对其进行修改。 它为的值的`Executable`属性`Application`元素。 |
| applications | workingDirectory | （可选）作为工作目录启动的应用程序使用程序包相对路径。 如果你未设置此值，操作系统将使用`System32`目录作为应用程序的工作目录。 |
| 进程 | 可执行文件 | 在大多数情况下，这将名称`executable`配置上方以删除路径和文件扩展名。 |
| 填充 | dll | 加载大幅.appx 程序包相对路径。 |
| 填充 | 配置 | （可选）控制大幅 dl 的行为方式。 此值的准确格式发生变化填充程序通过填充程序基于每个大幅尽量可以解释此"blob"。 |

`applications`， `processes`，并`shims`键是数组。 这意味着，你可以使用 config.json 文件指定多个应用程序、 流程和填充程序 DLL。


### <a name="package-and-test-the-app"></a>程序包和测试应用

接下来，创建程序包。

```
makeappx pack /d PackageContents /p PSFSamplePackageFixup.appx
```

然后，对其进行签名。

```
signtool sign /a /v /fd sha256 /f ExportedSigningCertificate.pfx PSFSamplePackageFixup.appx
```

有关详细信息，请参阅[如何创建包签名证书](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/how-to-create-a-package-signing-certificate)，以及[如何使用 signtool 对包](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/how-to-sign-a-package-using-signtool)

使用 PowerShell，安装程序包。

>[!NOTE]
> 请记住，请先卸载程序包。

```
powershell Add-AppxPackage .\PSFSamplePackageFixup.appx
```

运行该应用程序，并与应用的运行时修复观察行为。  诊断和打包步骤，根据需要重复。

### <a name="use-the-trace-shim"></a>使用跟踪填充程序

诊断打包的应用程序兼容性问题的替代技术是使用跟踪填充。 此 DLL 附带 PSF，并提供应用的行为，类似于进程监视器的诊断详细的视图。  它专为展示应用程序兼容性问题。  若要使用跟踪填充程序，将 DLL 添加到程序包，将以下片段添加到你 config.json，然后打包和安装你的应用程序。

```json
{
    "dll": "TraceShim.dll",
    "config": {
        "traceLevels": {
            "filesystem": "allFailures"
        }
    }
}
```

默认情况下，跟踪大幅筛选出可能会被视为"预期"的故障。  例如，应用程序可能会尝试无条件地在不检查以查看是否已存在，忽略结果的情况下删除文件。 这有遗憾结果，某些意外的失败可能会获取筛选掉，因此在上面的示例中，我们选择从文件系统函数接收所有故障。 我们这样做是因为我们知道从之前，尝试从 Config.txt 文件中读取失败，带有消息"未找到文件"。 这是经常观察到并且通常不假定为非预期的故障。 在实际中它是可能的最佳启动出筛选仅向意外失败，然后回退到所有故障如果仍无法识别的问题。

默认情况下，跟踪填充程序的输出获取发送到附加调试程序。 对于此示例中，我们不打算连接调试器，并将改为使用从 SysInternals [DebugView](https://docs.microsoft.com/en-us/sysinternals/downloads/debugview)程序以查看其输出。 之后运行该应用，我们可以看到相同的故障以前一样，这将为我们指出相同的运行时修复。

![找不到 TraceShim 文件](images/desktop-to-uwp/traceshim_filenotfound.png)

![TraceShim 访问被拒绝](images/desktop-to-uwp/traceshim_accessdenied.png)

## <a name="debug-extend-or-create-a-runtime-fix"></a>调试、 延长或创建运行时修复

你可以使用 Visual Studio 调试运行时修复，扩展运行时修复，或创建一个从零开始。 你将需要执行以下操作才能成功。

> [!div class="checklist"]
> * 添加打包项目
> * 添加项目的运行时修复
> * 添加启动可执行文件填充程序启动程序项目
> * 配置打包项目

完成后，你的解决方案将如下所示。

![已完成的解决方案](images/desktop-to-uwp/runtime-fix-project-structure.png)

让我们看一下在此示例中的每个项目。

| 项目 | 用途 |
|-------|-----------|
| DesktopApplicationPackage | 此项目基于[Windows 应用程序打包项目](desktop-to-uwp-packaging-dot-net.md)，并且它将输出 MSIX 程序包。 |
| Runtimefix | 这是一个包含一个或多个替换函数作为运行时修复的 c + + Dynamic-Linked 类库项目。 |
| ShimLauncher | 这是 c + + 空项目。 此项目是收集包支持框架的运行时可分发文件的位置。 它将输出的可执行文件。 该可执行文件是启动解决方案时运行的第一件事。 |
| WinFormsDesktopApplication | 此项目包含桌面应用程序的源代码。 |

若要查看的完整示例，包含所有这些类型的项目，请参阅[PSFSample](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/samples/PSFSample/)。

我们将演练创建和配置这些项目的每个你的解决方案中的步骤。


### <a name="create-a-package-solution"></a>创建程序包解决方案

如果你没有为桌面应用程序的解决方案，请在 Visual Studio 中创建新的**空白解决方案**。

![空白解决方案](images/desktop-to-uwp/blank-solution.png)

你可能还想要添加你有任何应用程序项目。

### <a name="add-a-packaging-project"></a>添加打包项目

如果你尚未获得**Windows 应用程序打包项目中**，创建一个并将其添加到你的解决方案。

![包项目模板](images/desktop-to-uwp/package-project-template.png)

有关 Windows 应用程序打包项目的详细信息，请参阅[包使用 Visual Studio 应用程序](desktop-to-uwp-packaging-dot-net.md)。

在**解决方案资源管理器**中，右键单击打包项目，选择**编辑**，然后添加到项目文件的底部:

```
<Target Name="PSFRemoveSourceProject" AfterTargets="ExpandProjectReferences" BeforeTargets="_ConvertItems">
<ItemGroup>
  <FilteredNonWapProjProjectOutput Include="@(_FilteredNonWapProjProjectOutput)">
  <SourceProject Condition="'%(_FilteredNonWapProjProjectOutput.SourceProject)'=='<your runtime fix project name goes here>'" />
  </FilteredNonWapProjProjectOutput>
  <_FilteredNonWapProjProjectOutput Remove="@(_FilteredNonWapProjProjectOutput)" />
  <_FilteredNonWapProjProjectOutput Include="@(FilteredNonWapProjProjectOutput)" />
</ItemGroup>
</Target>
```

### <a name="add-project-for-the-runtime-fix"></a>添加项目的运行时修复

将 c + +**动态链接库 (DLL)** 项目添加到解决方案。

![运行时修复库](images/desktop-to-uwp/runtime-fix-library.png)

右键单击该项目，然后选择**属性**。

中的属性页，查找的**标准 c + + 语言**的字段，然后在该字段旁边的下拉列表，选择**ISO C + + 17 标准 (/ std:c + + 17)** 选项。

![ISO 17 选项](images/desktop-to-uwp/iso-option.png)

右键单击该项目，，然后在上下文菜单中，选择**管理 Nuget 程序包**选项。 确保**所有**或**nuget.org**设置的**程序包源**选项。

单击设置图标下一步该字段。

搜索*PSF** Nuget 包，方法是，然后为此项目安装它。

![nuget 程序包](images/desktop-to-uwp/psf-package.png)

如果你想要调试或扩展现有的运行时修复，添加的运行时修复文件，通过使用本指南的[查找运行时修复](#find)部分中所述的指南。

如果你想要创建新的修补程序，不要添加任何到此项目尚未。 我们将帮助你将正确的文件添加到本指南后面的此项目。 现在，我们将继续设置你的解决方案。

### <a name="add-a-project-that-starts-the-shim-launcher-executable"></a>添加启动可执行文件填充程序启动程序项目

向解决方案中添加一个**空项目**的 c + + 项目。

![空项目](images/desktop-to-uwp/blank-app.png)

使用在上一节中所述的相同指南将**PSF** Nuget 程序包添加到此项目。

打开项目，并在**常规**设置页中的属性页将**目标名称**属性设置为``ShimLauncher32``或``ShimLauncher64``具体取决于你的应用程序的体系结构。

![填充程序启动程序参考](images/desktop-to-uwp/shim-exe-reference.png)

你的解决方案中添加对运行时修复项目的项目引用。

![运行时修复参考](images/desktop-to-uwp/reference-fix.png)

右键单击该引用，并在**属性**窗口中，将这些值。

| 属性 | 值 |
|-------|-----------|-------|
| 复制本地 | True |
| 复制本地卫星集 | True |
| 引用程序集输出 | True |
| 链接库依赖项 | False |
| 链接库依赖项输入 | False |

### <a name="configure-the-packaging-project"></a>配置打包项目

在打包项目中，右键单击**应用程序**文件夹，然后选择**添加引用**。

![添加项目引用](images/desktop-to-uwp/add-reference-packaging-project.png)

选择填充程序启动程序项目和你的桌面应用程序项目，然后选择**确定**按钮。

![桌面项目](images/desktop-to-uwp/package-project-references.png)

>[!NOTE]
> 如果你没有源代码你的应用程序，只需选择填充程序启动程序项目。 我们将向你介绍如何创建配置文件时引用可执行文件。

在**应用程序**节点中，右键单击大幅启动器应用程序，然后选择**设置为入口点**。

![设置入口点](images/desktop-to-uwp/set-startup-project.png)

添加一个名为文件``config.json``向打包项目，然后，复制并粘贴到文件中的以下 json 文本。 将**程序包操作**属性设置为**内容**。

```json
{
    "applications": [
        {
            "id": "",
            "executable": "",
            "workingDirectory": ""
        }
    ],
    "processes": [
        {
            "executable": "",
            "shims": [
                {
                    "dll": "",
                    "config": {
                    }
                }
            ]
        }
    ]
}
```
对于每个键提供的值。 使用此表作为指南。

| 数组 | key | 值 |
|-------|-----------|-------|
| applications | id |  使用的值`Id`属性`Application`在程序包清单中的元素。 |
| applications | 可执行文件 | 想要启动的可执行文件程序包相对路径。 在大多数情况下，你可以从程序包清单文件获取此值之前对其进行修改。 它为的值的`Executable`属性`Application`元素。 |
| applications | workingDirectory | （可选）作为工作目录启动的应用程序使用程序包相对路径。 如果你未设置此值，操作系统将使用`System32`目录作为应用程序的工作目录。 |
| 进程 | 可执行文件 | 在大多数情况下，这将名称`executable`配置上方以删除路径和文件扩展名。 |
| 填充 | dll | 填充程序加载 DLL 程序包相对路径。 |
| 填充 | 配置 | （可选）控制大幅 dl 的行为方式。 此值的准确格式发生变化填充程序通过填充程序基于每个大幅尽量可以解释此"blob"。 |

完成后，你``config.json``文件将如下所示。

```json
{
  "applications": [
    {
      "id": "DesktopApplication",
      "executable": "DesktopApplication/WinFormsDesktopApplication.exe",
      "workingDirectory": "WinFormsDesktopApplication"
    }
  ],
  "processes": [
    {
      "executable": ".*App.*",
      "shims": [ { "dll": "RuntimeFix.dll" } ]
    }
  ]
}

```

>[!NOTE]
> `applications`， `processes`，并`shims`键是数组。 这意味着，你可以使用 config.json 文件指定多个应用程序、 流程和填充程序 DLL。

### <a name="debug-a-runtime-fix"></a>调试运行时修复

在 Visual Studio 中，按 F5 启动调试程序。  启动的第一件事是大幅启动器应用程序，这反过来启动目标桌面应用程序。  若要调试目标桌面应用程序，你将需要手动将附加到桌面应用程序进程通过选择**调试**->**附加到进程**，并选择应用程序进程。 若要允许使用本机运行时修复程序 DLL 的.NET 应用程序调试，请选择托管和本机代码类型 （混合的模式调试）。  

在此设置好，你可以桌面应用程序代码和运行时修复项目中设置断点旁边的代码行。 如果你没有源代码你的应用程序，你将能够在运行时修复项目中设置断点仅旁边的代码行。

>[!NOTE]
> 在 Visual Studio 为你提供最简单的开发和调试体验，有一些限制，因此稍后在本指南中，我们将讨论其他你可以将应用的调试技术。

## <a name="create-a-runtime-fix"></a>创建运行时修复

如果不存在，但在运行时修复该问题，你想要解决，你可以通过编写替换功能，包括任何配置数据创建新的运行时修复的意义。 让我们看一下每个部分。

### <a name="replacement-functions"></a>替换函数

首先，确定 MSIX 容器中运行你的应用程序时，无法通过调用的函数。 然后，你可以创建你想要改为调用的运行时管理器的替换函数。 这使你能够函数的实现替换为符合现代的运行时环境的规则的行为。

在 Visual Studio 中，打开在本指南前面部分中创建的运行时修复项目。

声明``SHIM_DEFINE_EXPORTS``宏，然后添加为一个包含语句`shim_framework.h`的每个顶部。想要添加的运行时修复函数 CPP 文件。

```c++
#define SHIM_DEFINE_EXPORTS
#include <shim_framework.h>
```
>[!IMPORTANT]
>请确保`SHIM_DEFINE_EXPORTS`宏出现之前 include 语句。

创建具有相同的签名的函数的函数的具有你想要修改的行为。 下面是示例函数，替换`MessageBoxW`函数。

```c++
auto MessageBoxWImpl = &::MessageBoxW;
int WINAPI MessageBoxWShim(
    _In_opt_ HWND hwnd,
    _In_opt_ LPCWSTR,
    _In_opt_ LPCWSTR caption,
    _In_ UINT type)
{
    return MessageBoxWImpl(hwnd, L"SUCCESS: This worked", caption, type);
}

DECLARE_SHIM(MessageBoxWImpl, MessageBoxWShim);
```

调用`DECLARE_SHIM`地图`MessageBoxW`到新替换函数的函数。 当你的应用程序尝试调用`MessageBoxW`函数，它将调用替换函数相反。

#### <a name="protect-against-recursive-calls-to-functions-in-runtime-fixes"></a>防止递归调用的函数时在运行时修复

你可以选择将应用`reentrancy_guard`抵御递归调用的函数时在运行时修复你函数的类型。

例如，你可能会产生的替换函数`CreateFile`函数。 你的实现可能会调用`CopyFile`函数，但实现`CopyFile`函数可能会调用`CreateFile`函数。 这可能导致无限递归周期的调用`CreateFile`函数。

有关详细信息`reentrancy_guard`请参阅[authoring.md](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/Authoring.md)

### <a name="configuration-data"></a>配置数据

如果你想要将配置数据添加到你的运行时修复，请考虑将它添加到``config.json``。 这样一来，你可以使用`ShimQueryCurrentDllConfig`轻松地分析该数据。 此示例分析该配置文件中的布尔值和字符串值。

```c++
if (auto configRoot = ::ShimQueryCurrentDllConfig())
{
    auto& config = configRoot->as_object();

    if (auto enabledValue = config.try_get("enabled"))
    {
        g_enabled = enabledValue->as_boolean().get();
    }

    if (auto logPathValue = config.try_get("logPath"))
    {
        g_logPath = logPathValue->as_string().wstring();
    }
}
```

## <a name="other-debugging-techniques"></a>其他的调试技术

虽然 Visual Studio 为你提供最简单的开发和调试体验，有一些限制。

首先，F5 调试通过部署 loose 文件从包布局文件夹路径，而不是从.appx 程序包安装运行应用程序。  布局文件夹通常没有相同的安全限制作为安装的程序包文件夹。 因此，它可能无法重现之前应用运行时修复程序包路径访问拒绝错误。

若要解决此问题，请使用.appx 程序包部署，而不是 F5 松散文件部署。  若要创建的.appx 包文件，请使用 Windows SDK 中，从[MakeAppx](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/make-appx-package--makeappx-exe-)实用程序，上面所述。 或者，从 Visual Studio 中，右键单击你的应用程序的项目节点并选择**应用商店**->**创建应用包**。

使用 Visual Studio 的另一个问题是它不具有用于将附加到调试程序启动任何子进程的内置支持。   这使得更难进行调试的目标应用程序，必须手动连接由 Visual Studio 启动后启动路径中的逻辑。

若要解决此问题，请使用支持子进程的调试器附加。  请注意，它通常不可能将在实时 (JIT) 调试程序附加到目标应用程序。  这是因为大多数 JIT 技术涉及启动调试程序代替目标应用，通过 ImageFileExecutionOptions 注册表项。  这便失去意义 ShimLauncher.exe 用于 ShimRuntime.dll 注入目标应用的 detouring 机制。  WinDbg，包含在[Windows 调试工具](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/index)，并且从[Windows SDK](https://developer.microsoft.com/en-US/windows/downloads/windows-10-sdk)中，获取支持子进程附加。  它现在还支持直接[启动和调试 UWP 应用](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-a-uwp-app-using-windbg#span-idlaunchinganddebuggingauwpappspanspan-idlaunchinganddebuggingauwpappspanspan-idlaunchinganddebuggingauwpappspanlaunching-and-debugging-a-uwp-app)。

若要调试作为子进程的目标应用程序启动时，启动``WinDbg``。

```
windbg.exe -plmPackage PSFSampleWithFixup_1.0.59.0_x86__7s220nvg1hg3m -plmApp PSFSample
```

在``WinDbg``提示，启用调试的子并设置适当的断点。

```
.childdbg 1
g
```
（执行，直到目标应用程序启动和进入调试器）

```
sxe ld fixup.dll
g
```
（之前加载 DLL 修正执行）

```
bp ...
```

>[!NOTE]
> [PLMDebug](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/plmdebug)还可将调试程序附加到在启动后的应用，也包含在[Windows 调试工具](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/index)。  但是，它是更复杂，使用比现在提供 WinDbg 的直接支持。

## <a name="support-and-feedback"></a>支持和反馈

**查找问题的答案**

有问题？ 请在 Stack Overflow 上向我们提问。 我们的团队会监视这些[标记](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 你还可以在[此处](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)提问。
