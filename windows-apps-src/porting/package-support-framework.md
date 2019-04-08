---
Description: 修复阻止 MSIX 容器中运行桌面应用程序的问题
Search.Product: eADQiWindows 10XVcnh
title: 修复阻止 MSIX 容器中运行桌面应用程序的问题
ms.date: 07/02/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 80f9c8bad9445bd9cfef9b09c00f99929fda37aa
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57590722"
---
# <a name="apply-runtime-fixes-to-an-msix-package-by-using-the-package-support-framework"></a>使用包支持框架将运行时修补程序应用于 MSIX 包

包支持框架是一个开放源代码工具包，可帮助你应用修补程序到现有的 win32 应用程序时不能访问源代码，以便它可以在 MSIX 容器中运行。 包支持框架可帮助遵循最佳做法现代的运行时环境的应用程序。

若要了解详细信息，请参阅[包支持框架](https://docs.microsoft.com/windows/msix/package-support-framework-overview)。

本指南将帮助您识别应用程序兼容性问题和查找、 应用和扩展运行时修补程序解决它们。

<a id="identify" />

## <a name="identify-packaged-application-compatibility-issues"></a>确定打包的应用程序兼容性问题

首先，创建你的应用程序的包。 然后，安装并运行，然后观察其行为。 可能会收到错误消息可以帮助你确定兼容性问题。 此外可以使用[Process Monitor](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon)识别问题。  常见的问题与应用程序工作目录和程序路径权限方面的假设。

### <a name="using-process-monitor-to-identify-an-issue"></a>使用进程监视器以确定问题

[进程监视器](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon)是一个强大的工具来观察应用的文件和注册表操作和它们的结果。  这可以帮助你了解应用程序兼容性问题。  打开进程监视器后, 添加筛选器 (筛选器 > 筛选器...) 以包括仅从应用程序可执行文件的事件。

![Procmon 配合应用筛选器](images/desktop-to-uwp/procmon_app_filter.png)

将显示事件的列表。 许多这些事件，该单词**成功**将显示在**结果**列。

![Procmon 配合事件](images/desktop-to-uwp/procmon_events.png)

（可选） 你可以筛选事件以仅显示仅失败。

![Procmon 配合排除成功](images/desktop-to-uwp/procmon_exclude_success.png)

如果您怀疑文件系统访问失败，搜索在 System32/SysWOW64 或包文件路径下的失败事件。 在这里，太还有助于筛选器。 在此列表的底部开始，向上滚动。 具有最近出现在此列表的底部显示的失败。 注意大多数错误包含字符串，例如"访问被拒绝，"和"未找到路径/名称"，并忽略看上去可疑的操作。 [PSFSample](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/samples/PSFSample/)有两个问题。 您可以看到在下图中显示的列表中的这些问题。

![Procmon 配合 Config.txt](images/desktop-to-uwp/procmon_config_txt.png)

在此图中显示的第一个问题，在应用程序无法从位于"C:\Windows\SysWOW64"路径中的"Config.txt"文件中读取。 不太可能尝试直接引用该路径的应用程序。 很可能尝试使用相对路径，从该文件读取，并且默认情况下，"System32/SysWOW64"为应用程序的工作目录。 这表明应用程序应为其当前工作目录设置为某个位置的包中。 查看在 appx 内，我们可以看到该文件存在可执行文件所在的同一目录中。

![应用 Config.txt](images/desktop-to-uwp/psfsampleapp_config_txt.png)

在下图显示第二个问题。

![Procmon 配合日志文件](images/desktop-to-uwp/procmon_logfile.png)

在本期中，应用程序无法为它的包路径写入.log 文件。 这会建议可能有助于文件重定向链接地址信息。

<a id="find" />

## <a name="find-a-runtime-fix"></a>查找运行时修补程序

PSF 包含可以使用，稍后再试，例如文件重定向链接地址信息的运行时修补程序。

### <a name="file-redirection-fixup"></a>文件重定向链接地址信息

可以使用[文件重定向链接地址信息](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/fixups/FileRedirectionFixup)重定向写入或读取数据的目录中，无法从 MSIX 容器中运行的应用程序访问尝试。

例如，如果你的应用程序将写入到你的应用程序可执行文件所在的同一目录中的日志文件，然后您可以使用[文件重定向链接地址信息](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/fixups/FileRedirectionFixup)在另一个位置，例如本地应用程序的数据存储中创建该日志文件。

### <a name="runtime-fixes-from-the-community"></a>来自社区的运行时修补程序

请务必查看对社区贡献我们[GitHub](https://github.com/Microsoft/MSIX-PackageSupportFramework)页。 很可能其他开发人员已经解决了你境况相似的问题并获得的共享运行时修补程序。

## <a name="apply-a-runtime-fix"></a>应用运行时修补程序

从 Windows SDK 中，并通过执行以下步骤，您可以应用几个简单的工具的现有运行时修补的程序。

> [!div class="checklist"]
> * 创建的包布局文件夹
> * 获取包支持框架文件
> * 将它们添加到您的包
> * 修改包清单
> * 创建配置文件

让我们通过每个任务。

### <a name="create-the-package-layout-folder"></a>创建包布局文件夹

如果你已具有.msix （或.appx） 文件，您可以到将用作临时区域为您的包的布局文件夹解压缩其内容。 您可以使用基于你的 sdk 的安装路径的 MakeAppx 工具在命令提示符下执行此操作，这是你将在其中查找 Windows 10 电脑上存在 makeappx.exe 工具： x86:C:\Program 文件 (x86) \Windows Kits\10\bin\x86\makeappx.exe x64:C:\Program 文件 (x86) \Windows Kits\10\bin\x64\makeappx.exe

```ps
makeappx unpack /p PSFSamplePackage_1.0.60.0_AnyCPU_Debug.msix /d PackageContents

```

这样一来您看起来如下所示。

![包布局](images/desktop-to-uwp/package_contents.png)

如果没有.msix （或.appx） 文件开始，你可以从头开始创建的包文件夹和文件。

### <a name="get-the-package-support-framework-files"></a>获取包支持框架文件

通过使用独立 Nuget 命令行工具或通过 Visual Studio，可以获取 PSF Nuget 包。

#### <a name="get-the-package-by-using-the-command-line-tool"></a>通过使用命令行工具获取包

从该位置安装的 Nuget 命令行工具： https://www.nuget.org/downloads。 然后，在 Nuget 命令行中，运行以下命令：

```ps
nuget install Microsoft.PackageSupportFramework
```

#### <a name="get-the-package-by-using-visual-studio"></a>通过使用 Visual Studio 中获取包

在 Visual Studio 中，右键单击你的解决方案或项目节点并选择管理 Nuget 包命令之一。  搜索**Microsoft.PackageSupportFramework**或**PSF** Nuget.org 上查找包。然后，安装它。

### <a name="add-the-package-support-framework-files-to-your-package"></a>将包支持框架文件添加到您的包

将所需的 32 位和 64 位 PSF Dll 和可执行文件添加到包目录中。 使用下表作为指南。 此外需要包括任何所需的运行时修补程序。 在本示例中，我们需要文件重定向运行时修补程序。

| 应用程序可执行文件是 x64 | 应用程序可执行文件是 x86 |
|-------------------------------|-----------|
| [PSFLauncher64.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/PsfLauncher/readme.md) |  [PSFLauncher32.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/PsfLauncher/readme.md) |
| [PSFRuntime64.dll](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/PsfRuntime/readme.md) | [PSFRuntime32.dll](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/PsfRuntime/readme.md) |
| [PSFRunDll64.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/PsfRunDll/readme.md) | [PSFRunDll32.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/PsfRunDll/readme.md) |

包内容应现在如下所示。

![包二进制文件](images/desktop-to-uwp/package_binaries.png)

### <a name="modify-the-package-manifest"></a>修改包清单

在文本编辑器中，打开包清单，然后设置`Executable`属性的`Application`PSF 启动器可执行文件的名称的元素。  如果您知道您的目标应用程序的体系结构，选择适当的版本，PSFLauncher32.exe 或 PSFLauncher64.exe。  如果没有，PSFLauncher32.exe 将在所有情况下工作。  下面提供了一个示例。

```xml
<Package ...>
  ...
  <Applications>
    <Application Id="PSFSample"
                 Executable="PSFLauncher32.exe"
                 EntryPoint="Windows.FullTrustApplication">
      ...
    </Application>
  </Applications>
</Package>
```

### <a name="create-a-configuration-file"></a>创建配置文件

创建文件名称``config.json``，并将该文件保存到您的包的根文件夹。 修改 config.json 文件声明的应用的 ID，以指向刚替换的可执行文件。 使用通过使用 Process Monitor 获得的知识，您可以还设置工作目录，以及使用文件重定向链接地址信息重定向到的包相对"PSFSampleApp"目录下的.log 文件的读/写。

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
            "fixups": [
                {
                    "dll": "FileRedirectionFixup.dll",
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

以下是针对 config.json 架构的指南：

| 数组 | 键 | 值 |
|-------|-----------|-------|
| applications | id |  使用的值`Id`属性的`Application`包清单中的元素。 |
| applications | 可执行文件 | 你想要启动的可执行文件包相对路径。 在大多数情况下，您可以从包清单文件获取此值之前对其进行修改。 它是值的`Executable`属性的`Application`元素。 |
| applications | WorkingDirectory | （可选）使用作为启动的应用程序的工作目录的包相对路径。 如果未设置此值，操作系统使用`System32`目录视为应用程序的工作目录。 |
| 进程 | 可执行文件 | 在大多数情况下，这将是名称的`executable`上面配置了路径和文件扩展名已移除。 |
| 修正 | dll | 修正，.msix/.appx 要加载的包相对路径。 |
| 修正 | 配置 | （可选）控制链接地址信息 dl 的行为方式。 此值的确切格式而异的链接地址信息通过修正基础为每个链接地址信息可以解释此"blob"，因为它需要。 |

`applications`， `processes`，和`fixups`密钥是数组。 这意味着您可以使用的 config.json 文件来指定多个应用程序、 进程和 DLL 的链接地址信息。

### <a name="package-and-test-the-app"></a>包和测试应用程序

接下来，创建的包。

```ps
makeappx pack /d PackageContents /p PSFSamplePackageFixup.msix
```

然后，对其签名。

```ps
signtool sign /a /v /fd sha256 /f ExportedSigningCertificate.pfx PSFSamplePackageFixup.msix
```

有关详细信息，请参阅[如何创建程序包签名证书](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/how-to-create-a-package-signing-certificate)和[如何使用 signtool 对包进行签名](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/how-to-sign-a-package-using-signtool)

使用 PowerShell 安装包。

>[!NOTE]
> 请记住，请先卸载包。

```ps
powershell Add-AppPackage .\PSFSamplePackageFixup.msix
```

运行应用程序并观察行为与应用的运行时修补程序。  诊断和打包步骤，根据需要重复。

### <a name="use-the-trace-fixup"></a>使用跟踪链接地址信息

打包应用程序兼容性问题诊断的替代技术是使用跟踪链接地址信息。 此 DLL 包括在 PSF 并提供应用的行为类似于 Process Monitor 的详细诊断视图。  这是专门为了暴露出应用程序兼容性问题。  若要使用跟踪链接地址信息、 将 DLL 添加到包中，将以下片段添加到你 config.json，然后包并安装你的应用程序。

```json
{
    "dll": "TraceFixup.dll",
    "config": {
        "traceLevels": {
            "filesystem": "allFailures"
        }
    }
}
```

默认情况下，跟踪修正筛选出可能被视为"应为"的故障。  例如，应用程序可能会尝试无条件地删除文件，而不检查以查看它是否已存在，正在忽略结果。 这有一些意外的故障可能会获取筛选掉，非常不幸的后果，因此在上面的示例中，我们决定从文件系统函数接收的所有故障。 我们这样做是因为我们知道从在此之前，尝试从 Config.txt 文件中读取失败，出现消息"未找到文件"。 这是经常观察，并通常不假定为意外的故障。 在实践中它是可能的最佳开始筛选仅对意外故障，然后回退到所有失败如果仍不能识别出现问题。

默认情况下，跟踪链接地址信息的输出发送到连接的调试器。 对于此示例中，我们不是要附加调试器，然后将改为使用[DebugView](https://docs.microsoft.com/en-us/sysinternals/downloads/debugview)从 SysInternals 查看其输出的程序。 后运行该应用程序，我们可以看到相同的故障与之前一样，这会为我们指出同一运行时修补程序。

![找不到 TraceShim 文件](images/desktop-to-uwp/traceshim_filenotfound.png)

![TraceShim 访问被拒绝](images/desktop-to-uwp/traceshim_accessdenied.png)

## <a name="debug-extend-or-create-a-runtime-fix"></a>调试、 扩展或创建的运行时修补程序

可以使用 Visual Studio 调试运行时修补程序、 扩展运行时修复问题，或创建一个从零开始。 你将需要执行以下操作才能成功。

> [!div class="checklist"]
> * 添加打包项目
> * 添加运行时修补程序的项目
> * 添加启动 PSF 启动器可执行文件的项目
> * 配置打包项目

完成后，你的解决方案将如下所示。

![已完成的解决方案](images/desktop-to-uwp/runtime-fix-project-structure.png)

让我们看看在此示例中的每个项目。

| 项目 | 用途 |
|-------|-----------|
| DesktopApplicationPackage | 此项目基于[Windows 应用程序打包项目](desktop-to-uwp-packaging-dot-net.md)并输出 MSIX 包。 |
| Runtimefix | 这是包含一个或多个替代函数，用作运行时修补程序的 c + + Dynamic-Linked 库项目。 |
| PSFLauncher | 这是 c + + 空项目。 此项目是收集包支持框架的运行时可分发文件的位置。 它输出可执行文件。 该可执行文件是启动解决方案时运行的第一件事。 |
| WinFormsDesktopApplication | 此项目包含桌面应用程序的源代码。 |

若要查看包含所有这些类型的项目的完整示例，请参阅[PSFSample](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/samples/PSFSample/)。

让我们逐步完成各步骤来创建和配置每个项目在解决方案中。

### <a name="create-a-package-solution"></a>创建包的解决方案

如果还没有为桌面应用程序解决方案，创建一个新**空白解决方案**Visual Studio 中。

![空白解决方案](images/desktop-to-uwp/blank-solution.png)

您可能还想要添加你有任何应用程序项目。

### <a name="add-a-packaging-project"></a>添加打包项目

如果还没有**Windows 应用程序打包项目**，创建一个并将其添加到你的解决方案。

![包项目模板](images/desktop-to-uwp/package-project-template.png)

Windows 应用程序打包项目的详细信息，请参阅[通过使用 Visual Studio 打包应用程序，](desktop-to-uwp-packaging-dot-net.md)。

在中**解决方案资源管理器**，右键单击打包项目，选择**编辑**，然后将此添加到项目文件的底部：

```xml
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

### <a name="add-project-for-the-runtime-fix"></a>添加运行时修补程序的项目

添加 c + +**动态链接库 (DLL)** 到解决方案。

![运行时修补程序库](images/desktop-to-uwp/runtime-fix-library.png)

右键单击该项目，，然后选择**属性**。

在属性页中，找到**c + + 语言标准**字段，然后再在该字段旁边的下拉列表，选择**ISO C + + 17 标准 (/ /std: c + + 17)** 选项。

![ISO 17 选项](images/desktop-to-uwp/iso-option.png)

右键单击该项目，然后在上下文菜单中，选择**管理 Nuget 包**选项。 絋粄**包源**选项设置为**所有**或**nuget.org**。

单击设置图标下一步该字段。

搜索*PSF** Nuget 包，并将其安装为此项目。

![nuget 包](images/desktop-to-uwp/psf-package.png)

如果你想要调试或扩展现有的运行时修补程序，将使用中所述的指南获取运行时修补程序文件添加[查找运行时修复](#find)本指南的部分。

如果你想要创建全新的修补程序，不添加任何内容到此项目还为时尚早。 我们将帮助你将正确的文件添加到此项目在本指南后面。 现在，我们将继续设置你的解决方案。

### <a name="add-a-project-that-starts-the-psf-launcher-executable"></a>添加启动 PSF 启动器可执行文件的项目

添加 c + +**空项目**到解决方案。

![空项目](images/desktop-to-uwp/blank-app.png)

添加**PSF**到通过使用在上一部分中所述的相同指导此项目的 Nuget 包。

打开该项目，并在属性页**常规**设置页上，设置**目标名称**属性设置为``PSFLauncher32``或``PSFLauncher64``具体取决于你的应用程序的体系结构。

![PSF 启动器引用](images/desktop-to-uwp/shim-exe-reference.png)

在你的解决方案中添加对运行时修复项目的项目引用。

![运行时修复引用](images/desktop-to-uwp/reference-fix.png)

右键单击引用，然后在**属性**窗口中，将这些值应用。

| 属性 | 值 |
|-------|-----------|
| 复制本地 | True |
| 复制本地附属程序集 | True |
| 引用程序集输出 | True |
| 链接库依赖项 | False |
| 链接库依赖项输入 | False |

### <a name="configure-the-packaging-project"></a>配置打包项目

在打包项目中，右键单击**应用程序**文件夹，然后选择**添加引用**。

![添加项目引用](images/desktop-to-uwp/add-reference-packaging-project.png)

选择 PSF 启动器项目和桌面应用程序项目，并选择**确定**按钮。

![桌面项目](images/desktop-to-uwp/package-project-references.png)

>[!NOTE]
> 如果您没有源代码应用程序，只需选择 PSF 启动器项目。 我们将介绍如何创建配置文件时引用的可执行文件。

在中**应用程序**节点，右键单击 PSF 启动器应用程序，然后再选择**设置为入口点**。

![设置入口点](images/desktop-to-uwp/set-startup-project.png)

添加名为的文件``config.json``到打包项目，然后，复制并粘贴到该文件的以下 json 文本。 设置**包操作**属性设置为**内容**。

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
            "fixups": [
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

对于每个键提供值。 使用此表作为指南。

| 数组 | 键 | 值 |
|-------|-----------|-------|
| applications | id |  使用的值`Id`属性的`Application`包清单中的元素。 |
| applications | 可执行文件 | 你想要启动的可执行文件包相对路径。 在大多数情况下，您可以从包清单文件获取此值之前对其进行修改。 它是值的`Executable`属性的`Application`元素。 |
| applications | WorkingDirectory | （可选）使用作为启动的应用程序的工作目录的包相对路径。 如果未设置此值，操作系统使用`System32`目录视为应用程序的工作目录。 |
| 进程 | 可执行文件 | 在大多数情况下，这将是名称的`executable`上面配置了路径和文件扩展名已移除。 |
| 修正 | dll | 链接地址信息 DLL 加载的包相对路径。 |
| 修正 | 配置 | （可选）控制链接地址信息 DLL 的行为方式。 此值的确切格式而异的链接地址信息通过修正基础为每个链接地址信息可以解释此"blob"，因为它需要。 |

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
      "fixups": [ { "dll": "RuntimeFix.dll" } ]
    }
  ]
}

```

>[!NOTE]
> `applications`， `processes`，和`fixups`密钥是数组。 这意味着您可以使用的 config.json 文件来指定多个应用程序、 进程和 DLL 的链接地址信息。

### <a name="debug-a-runtime-fix"></a>调试运行时修补程序

在 Visual Studio 中，按 F5 启动调试器。  启动第一件事是 PSF 启动器应用程序，反过来，启动目标桌面应用程序。  若要调试目标桌面应用程序，你将需要手动选择到桌面应用程序进程中附加**调试**->**附加到进程**，然后选择应用程序过程。 若要允许.NET 应用程序使用本机运行时修补程序 DLL 的调试，选择托管和本机代码类型 （混合的模式调试）。  

一旦你已设置此功能，可以在桌面应用程序代码和运行时修补程序项目中设置断点的代码行旁边。 如果没有源代码到应用程序，你将能够在运行时修补程序项目中设置断点的代码行旁边仅。

>[!NOTE]
> 尽管 Visual Studio 为您提供了最简单的开发和调试体验，有一些限制，因此稍后在本指南中，我们将讨论可以应用其他调试技术。

## <a name="create-a-runtime-fix"></a>创建运行时修补程序

如果尚不存在运行时修复的问题，你想要解决，可以通过编写替换函数并包含任何配置数据创建新的运行时修补程序的意义。 让我们看看每个部分。

### <a name="replacement-functions"></a>替换函数

首先，确定哪一个函数调用失败时 MSIX 容器中运行应用程序。 然后，可以创建你想要改为调用的运行时管理器的替代函数。 这使你有机会函数的实现替换为符合现代的运行时环境的规则的行为。

在 Visual Studio 中，打开在本指南前面部分中创建的运行时修补程序项目。

声明``FIXUP_DEFINE_EXPORTS``宏，然后添加 include 语句进行`fixup_framework.h`顶部的每个。要添加运行时修补程序的函数 CPP 文件。

```c++
#define FIXUP_DEFINE_EXPORTS
#include <fixup_framework.h>
```

>[!IMPORTANT]
>请确保`FIXUP_DEFINE_EXPORTS`宏 include 语句之前出现。

创建具有相同签名的函数的函数的人员具有你想要修改的行为。 下面是替换的示例函数`MessageBoxW`函数。

```c++
auto MessageBoxWImpl = &::MessageBoxW;
int WINAPI MessageBoxWFixup(
    _In_opt_ HWND hwnd,
    _In_opt_ LPCWSTR,
    _In_opt_ LPCWSTR caption,
    _In_ UINT type)
{
    return MessageBoxWImpl(hwnd, L"SUCCESS: This worked", caption, type);
}

DECLARE_FIXUP(MessageBoxWImpl, MessageBoxWFixup);
```

在调用`DECLARE_FIXUP`映射`MessageBoxW`到新的替换函数的函数。 当你的应用程序尝试调用`MessageBoxW`函数，它将调用替换函数相反。

#### <a name="protect-against-recursive-calls-to-functions-in-runtime-fixes"></a>防止对运行时修补程序中的函数的递归调用

可以选择应用`reentrancy_guard`类型设置为您防止对运行时修补程序中的函数的递归调用的函数。

例如，可能会产生的替换函数`CreateFile`函数。 您的实现可能会调用`CopyFile`函数，但实现`CopyFile`函数可能调用`CreateFile`函数。 这可能会导致无限递归循环的调用的`CreateFile`函数。

有关详细信息`reentrancy_guard`请参阅[authoring.md](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/Authoring.md)

### <a name="configuration-data"></a>配置数据

如果你想要将配置数据添加到运行时修补程序，请考虑将其添加到``config.json``。 这样一来，可以使用`FixupQueryCurrentDllConfig`来轻松地分析该数据。 此示例中分析来自该配置文件的布尔值和字符串值。

```c++
if (auto configRoot = ::FixupQueryCurrentDllConfig())
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

## <a name="other-debugging-techniques"></a>其他调试技术

Visual Studio 为你提供最简单的开发和调试体验，虽然有一些限制。

首先，F5 调试运行此应用程序部署松散文件从包布局的文件夹路径，而不是从.msix 安装 /.appx 包。  布局文件夹通常没有相同的安全限制与安装的包文件夹。 因此，它可能无法重新生成之前应用的运行时修补程序的包路径访问拒绝错误。

若要解决此问题，请使用.msix /.appx 包部署而不是 F5 松散文件部署。  若要创建.msix /.appx 包文件，使用[MakeAppx](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/make-appx-package--makeappx-exe-)实用程序从 Windows SDK 中，按上文所述。 或者，从 Visual Studio 中，右键单击你的应用程序项目节点并选择**应用商店**->**创建应用程序包**。

使用 Visual Studio 的另一个问题是它不具有对附加到调试器启动的任何子进程的内置支持。   这使得难以调试的目标应用程序，必须手动附加 Visual studio 启动后启动路径中的逻辑。

若要解决此问题，使用支持子进程调试程序附加。  请注意，它通常不可能将在实时 (JIT) 调试程序附加到目标应用程序。  这是因为大多数 JIT 技术涉及启动目标应用程序中，通过 ImageFileExecutionOptions 注册表项代替调试器。  这就违背了 PSFLauncher.exe 用于向目标应用插入 FixupRuntime.dll detouring 机制。  中包含的 WinDbg 中，[有关 Windows 调试工具](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/index)，并从获取[Windows SDK](https://developer.microsoft.com/en-US/windows/downloads/windows-10-sdk)，支持子进程附加。  它现在还支持直接[启动并调试 UWP 应用](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-a-uwp-app-using-windbg#span-idlaunchinganddebuggingauwpappspanspan-idlaunchinganddebuggingauwpappspanspan-idlaunchinganddebuggingauwpappspanlaunching-and-debugging-a-uwp-app)。

若要调试目标应用程序启动时作为子进程，启动``WinDbg``。

```ps
windbg.exe -plmPackage PSFSampleWithFixup_1.0.59.0_x86__7s220nvg1hg3m -plmApp PSFSample
```

在``WinDbg``提示、 启用子调试并设置适当的断点。

```ps
.childdbg 1
g
```

（执行，直到目标应用程序启动并进入调试器）

```ps
sxe ld fixup.dll
g
```

（执行之前加载 DLL 的链接地址信息）

```ps
bp ...
```

>[!NOTE]
> [PLMDebug](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/plmdebug)还可用来将调试器附加到应用程序启动时，并且还包含在[的 Windows 调试工具](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/index)。  但是，它是更复杂，使用比现在提供 WinDbg 的直接支持。

## <a name="support-and-feedback"></a>支持和反馈

**查找问题的答案**

有问题？ 请在 Stack Overflow 上向我们提问。 我们的团队会监视这些[标记](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 你还可以在[此处](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)提问。
