---
author: normesta
Description: Fix issues that prevent your desktop application from running in an MSIX container
Search.Product: eADQiWindows 10XVcnh
title: 修复阻止桌面应用程序运行 MSIX 容器中的问题
ms.author: normesta
ms.date: 07/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 46d5705233af9e8254b9ac89a2d6e9891e90701f
ms.sourcegitcommit: 753dfcd0f9fdfc963579dd0b217b445c4b110a18
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "2861254"
---
# <a name="apply-runtime-fixes-to-an-msix-package-by-using-the-package-support-framework"></a>使用包支持框架应用于 MSIX 程序包的运行时修补程序

包支持框架是可帮助您修补程序时应用于现有 win32 应用程序不能访问的源代码，以便它可以运行 MSIX 容器中打开源工具包。 包支持框架可帮助您遵循的现代运行时环境的最佳做法的应用程序。

若要创建包支持框架，我们利用其是开发由 Microsoft 研究 (MSR) 打开源框架，可帮助 API 重定向和在挂起的[迂回的方式](https://www.microsoft.com/en-us/research/project/detours)技术。

此框架是开放源代码，轻量，您可以快速和使用它解决应用程序问题。 它还会为您提供机会咨询的全球社区和建立在他人投资的顶层。

## <a name="a-quick-look-inside-of-the-package-support-framework"></a>快速内部包支持框架

包支持框架包含可执行文件、 运行时管理器 DLL 和一组的运行时修补程序。

![包支持框架](images/desktop-to-uwp/package-support-framework.png)

它的工作原理如下。 您将创建的配置文件的指定您想要应用于您的应用程序 fix(s)。 然后，您将修改您的程序包以指向填充码启动器可执行文件。

用户启动应用程序，请填充码启动器时运行的第一个可执行文件。 它读取您的配置文件，并运行时 fix(s) 和运行时管理器 DLL 注入应用程序过程。

![包支持 Framework DLL 注入](images/desktop-to-uwp/package-support-framework-2.png)

运行时管理器需要通过运行在 MSIX 容器应用程序时应用修复。

本指南将帮助您确定应用程序兼容性问题，并以查找、 应用，并扩展运行时解决这些的修复。

<a id="identify" />

## <a name="identify-packaged-application-compatibility-issues"></a>确定打包应用程序兼容性问题

首先，创建一个包以便您的应用程序。 然后，安装、 运行它，并观察其行为。 您可能会收到错误消息，可帮助您确定兼容性问题。 您可以使用[进程监视器](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon)标识问题。  与工作目录和程序路径访问权限的应用程序推测相关的常见问题。

### <a name="using-process-monitor-to-identify-an-issue"></a>使用过程监视器标识问题

[进程监视器](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon)是用于观察应用程序的文件和注册表操作，以及其结果的强大工具。  这可以帮助您了解应用程序兼容性问题。  打开进程监视器后, 添加筛选器 (筛选器 > 筛选器...) 包括仅从应用程序可执行文件的事件。

![ProcMon 应用程序筛选器](images/desktop-to-uwp/procmon_app_filter.png)

将显示的事件的列表。 对于许多这些事件，word**成功**将出现在**结果**列。

![ProcMon 事件](images/desktop-to-uwp/procmon_events.png)

（可选） 您可以筛选事件以仅显示仅失败。

![ProcMon 排除成功](images/desktop-to-uwp/procmon_exclude_success.png)

如果您怀疑 filesystem 访问失败，搜索 System32/SysWOW64 或包文件路径下的失败事件。 筛选器还有助于在这里，太。 开始在此列表的底部，然后向上滚动。 在此列表的底部显示的故障发生最近所示。 大多数重视错误包含字符串，例如"访问被拒绝，"和"未找到路径/名称"，并忽略看起来不可疑的事项。 [PSFSample](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/samples/PSFSample/)具有两个问题。 您可以看到下图中显示的列表中的这些问题。

![ProcMon Config.txt](images/desktop-to-uwp/procmon_config_txt.png)

在图中显示的第一个问题，应用程序无法读取位于"C:\Windows\SysWOW64"路径中的"Config.txt"文件。 不太可能应用程序尝试直接引用该路径。 很可能尝试通过使用相对路径，读取从该文件，默认情况下，"System32/SysWOW64"应用程序的工作目录。 表示应用程序预期其当前的工作目录设置为其他地方程序包中。 查找在约，我们可以看到该文件存在与可执行文件位于同一目录中。

![应用程序 Config.txt](images/desktop-to-uwp/psfsampleapp_config_txt.png)

下图中显示第二个问题。

![ProcMon 日志文件](images/desktop-to-uwp/procmon_logfile.png)

在此问题，应用程序未能.log 文件写入其程序包的路径。 此建议可以帮助文件重定向填充码。

<a id="find" />

## <a name="find-a-runtime-fix"></a>查找运行时修复方法

PSF 包含您可以使用立即，如文件重定向填充码的运行时修补程序。

### <a name="file-redirection-shim"></a>文件重定向填充码

您可以使用[文件重定向填充码](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/develop/FileRedirectionShim)重定向尝试写入或读取不是从 MSIX 容器中运行的应用程序可访问的目录中的数据。

例如，如果您的应用程序写入日志文件中您的应用程序可执行相同的目录，然后可以使用[文件重定向填充码](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/develop/FileRedirectionShim)在另一个位置，如本地应用程序数据存储区中创建的日志文件。

### <a name="runtime-fixes-from-the-community"></a>从社区的运行时修补程序

请务必查看社区的贡献我们的[GitHub](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/develop)页面。 则可能其他开发人员解决了类似的问题，并且具有共享运行时修复。

## <a name="apply-a-runtime-fix"></a>应用运行时修补程序

从 Windows SDK，并通过执行以下步骤，您可以应用现有的运行时修复进行一些简单的工具。

> [!div class="checklist"]
> * 创建一个程序包布局文件夹
> * 获取包支持框架文件
> * 将其添加到包
> * 修改包清单
> * 创建一个配置文件

我们看一下每项任务。

### <a name="create-the-package-layout-folder"></a>创建程序包布局文件夹

如果您已经有一个.appx 文件，您可以将其内容解到将用作软件包的临时区域的布局文件夹中。  您可以这样从**x64 VS 2017 的本机工具命令提示符处**，或手动 SDK bin 路径中的可执行的搜索路径。

```
makeappx unpack /p PSFSamplePackage_1.0.60.0_AnyCPU_Debug.appx /d PackageContents

```

这将为您提供以下内容类似的内容。

![包布局](images/desktop-to-uwp/package_contents.png)

如果您首先没有.appx 文件，您可以从头开始创建 package 文件夹和文件。

### <a name="get-the-package-support-framework-files"></a>获取包支持框架文件

您可以通过使用 Visual Studio 中获取 PSF Nuget 程序包。 您可以使用独立 Nuget 命令行工具来获取它。

#### <a name="get-the-package-by-using-visual-studio"></a>使用 Visual Studio 中获取包

在 Visual Studio 中，右键单击您的解决方案或项目节点并选择管理 Nuget 程序包命令之一。  搜索**Microsoft.PackageSupportFramework**或**PSF** Nuget.org 上查找包。然后，安装它。

#### <a name="get-the-package-by-using-the-command-line-tool"></a>使用命令行工具获取包

从该位置安装 Nuget 命令行工具： https://www.nuget.org/downloads。 然后，从 Nuget 命令行中，运行以下命令：

```
nuget install Microsoft.PackageSupportFramework
```

### <a name="add-the-package-support-framework-files-to-your-package"></a>包支持框架文件添加到包

Package 目录中添加所需的 32 位和 64 位 PSF Dll 和可执行文件。 使用下表作为指南。 您还需要包括所需的任何运行时修补程序。 在本例中，我们需要文件重定向运行时修复。

| 应用程序可执行文件是 x64 | 应用程序可执行文件是 x86 |
|-------------------------------|-----------|
| [ShimLauncher64.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimLauncher/readme.md) |  [ShimLauncher32.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimLauncher/readme.md) |
| [ShimRuntime64.dll](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimRuntime/readme.md) | [ShimRuntime32.dll](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimRuntime/readme.md) |
| [ShimRunDll64.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimRunDll/readme.md) | [ShimRunDll32.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimRunDll/readme.md) |

包内容现在应看起来如下所示。

![包二进制文件](images/desktop-to-uwp/package_binaries.png)

### <a name="modify-the-package-manifest"></a>修改包清单

在文本编辑器中，打开您程序包的清单，然后设置`Executable`属性的`Application`元素填充码启动器可执行文件的名称。  如果您知道您的目标应用程序的体系结构，选择的相应版本，ShimLauncher32.exe 或 ShimLauncher64.exe。  如果没有，ShimLauncher32.exe 将在所有情况下工作。  下面提供了一个示例。

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

### <a name="create-a-configuration-file"></a>创建一个配置文件

创建一个文件名``config.json``，并将该文件保存到您包的根文件夹。 修改的 config.json 文件声明应用程序 ID，以指向您刚替换的可执行文件。 使用通过使用过程监视器获得的知识，您可以还设置的工作目录，以及使用文件重定向填充码将读取/写入重定向到.log 包相对于"PSFSampleApp"目录下的文件。

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
以下是 config.json 架构的指南：

| 数组 | key | 值 |
|-------|-----------|-------|
| applications | id |  使用的值`Id`属性的`Application`程序包清单中的元素。 |
| applications | 可执行文件 | 指向要启动的可执行文件的相对于程序包的路径。 在大多数情况下，您可以从您程序包的清单文件获取此值之前对其进行修改。 它是的值`Executable`属性的`Application`元素。 |
| applications | workingDirectory | （可选）要用作启动的应用程序的工作目录相对于程序包的路径。 如果不设置此值，使用操作系统`System32`作为应用程序的工作目录的目录。 |
| 流程 | 可执行文件 | 在大多数情况下，这将的名称`executable`上方与删除的路径和文件扩展名配置。 |
| 填充 | dll | 加载填充码.appx 相对于程序包的路径。 |
| 填充 | 配置 | （可选）控制填充码 dl 的行为方式。 此值的准确格式发生变化填充码通过填充码基于每个填充码可将此"blob"解释为其想。 |

`applications`， `processes`，和`shims`密钥的数组。 这意味着您可以使用 config.json 文件来指定多个应用程序、 过程和填充码 DLL。


### <a name="package-and-test-the-app"></a>包和测试应用程序

接下来，创建一个包。

```
makeappx pack /d PackageContents /p PSFSamplePackageFixup.appx
```

然后，对其进行签名。

```
signtool sign /a /v /fd sha256 /f ExportedSigningCertificate.pfx PSFSamplePackageFixup.appx
```

有关详细信息，请参阅[如何创建签名证书包](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/how-to-create-a-package-signing-certificate)以及[如何注册使用 signtool 包](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/how-to-sign-a-package-using-signtool)

使用 PowerShell，安装包。

>[!NOTE]
> 请记住首先卸载该包。

```
powershell Add-AppxPackage .\PSFSamplePackageFixup.appx
```

运行应用程序并运行时修复应用与观察的行为。  重复的诊断和打包必要的步骤。

### <a name="use-the-trace-shim"></a>使用跟踪填充码

诊断打包应用程序兼容性问题的替代技术是使用跟踪填充码。 此 DLL 附带 PSF，并提供应用程序的行为，类似于进程监视器的详细诊断视图。  精心设计以显示应用程序兼容性问题。  若要使用跟踪填充码，将 DLL 添加到包中，将以下片段添加到您 config.json，然后打包和安装应用程序。

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

默认情况下，跟踪填充码筛选出故障的可能被视为"预期"。  例如，应用程序可能会尝试无条件转而不会检查是否已存在，忽略结果中删除文件。 这有遗憾结果，可能会获取一些意外的故障筛选掉，因此在上面的示例，我们选择从文件系统函数接收所有失败。 我们这样做是因为我们知道从之前的尝试从 Config.txt 文件中读取操作失败消息"找不到文件"。 这是经常观察和通常不假定它们为意外故障。 在实践它是可能的最佳意外失败，仅对筛选，然后回退到所有失败如果问题仍然不能识别出启动。

默认情况下，从跟踪填充码输出获取发送到连接的调试器。 对于此示例，我们不打算调试器附加和将改为使用从 SysInternals [DebugView](https://docs.microsoft.com/en-us/sysinternals/downloads/debugview)程序以查看其输出。 运行后应用程序，我们可以看到相同失败，之前，其中将为我们指出的相同的运行时修补程序。

![找不到 TraceShim 文件](images/desktop-to-uwp/traceshim_filenotfound.png)

![TraceShim 访问被拒绝](images/desktop-to-uwp/traceshim_accessdenied.png)

## <a name="debug-extend-or-create-a-runtime-fix"></a>调试、 扩展或创建运行时修复

您可以使用 Visual Studio 调试运行时修复、 扩展运行时修复，或从头创建一个。 您需要执行这些操作成功。

> [!div class="checklist"]
> * 添加打包项目
> * 添加项目运行时修补程序 （英文）
> * 添加启动填充码启动器可执行的项目
> * 配置打包项目

完成后，您的解决方案将如下所示。

![已完成的解决方案](images/desktop-to-uwp/runtime-fix-project-structure.png)

让我们看一下在此示例中的每个项目。

| 项目 | 用途 |
|-------|-----------|
| DesktopApplicationPackage | 此项目基于[Windows 应用程序打包项目](desktop-to-uwp-packaging-dot-net.md)，它将输出 MSIX 包。 |
| Runtimefix | 这是包含充当运行修复的一个或多个替换函数 c + + Dynamic-Linked 库项目。 |
| ShimLauncher | 这是 c + + 空项目。 此项目是收集包支持框架的运行时的可发行软件包文件的位置。 它将输出可执行文件。 该可执行文件是首先运行时启动解决方案。 |
| WinFormsDesktopApplication | 此项目包含桌面应用程序的源代码。 |

若要查看包含所有这些类型的项目的完整示例，请参阅[PSFSample](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/samples/PSFSample/)。

让我们看一下创建和配置这些项目的每个解决方案中的步骤。


### <a name="create-a-package-solution"></a>创建解决方案包

如果您的桌面应用程序没有解决方案，请在 Visual Studio 中创建新的**空白解决方案**。

![空白解决方案](images/desktop-to-uwp/blank-solution.png)

您可能还希望添加您有任何应用程序项目。

### <a name="add-a-packaging-project"></a>添加打包项目

如果您没有**Windows 应用程序打包项目**，创建一个，并将其添加到您的解决方案。

![包项目模板](images/desktop-to-uwp/package-project-template.png)

Windows 应用程序打包项目的详细信息，请参阅[包使用 Visual Studio 应用程序](desktop-to-uwp-packaging-dot-net.md)。

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

### <a name="add-project-for-the-runtime-fix"></a>添加项目运行时修补程序 （英文）

C + +**动态链接库 (DLL)** 项目添加到解决方案。

![运行修复库](images/desktop-to-uwp/runtime-fix-library.png)

右键单击该项目，然后选择**属性**。

在属性页中，查找**c + + 语言标准**的字段中，并在该字段旁边的下拉列表列表中，选择**ISO C + + 17 标准 (/ std:c + + 17)** 选项。

![ISO 17 选项](images/desktop-to-uwp/iso-option.png)

右键单击该项目，然后在上下文菜单中，选择**管理 Nuget 程序包**选项。 确保**所有**或**nuget.org**设置**包源**选项。

单击该字段下一步设置图标。

搜索*PSF** Nuget 打包，并安装此项目。

![nuget 程序包](images/desktop-to-uwp/psf-package.png)

如果您想要调试或扩展现有的运行时修复，添加您使用本指南[查找运行时修复方法](#find)一节中所述的指南获得的运行时修复文件。

如果您想要创建全新的修复，不任何内容向该项目添加只尚未。 我们将帮助您在本指南后面此项目中添加的适当的文件。 现在，我们将继续设置您的解决方案。

### <a name="add-a-project-that-starts-the-shim-launcher-executable"></a>添加启动填充码启动器可执行的项目

C + +**空项目**项目添加到解决方案。

![空白项目](images/desktop-to-uwp/blank-app.png)

使用上一节中所述的相同指南添加到此项目的**PSF** Nuget 程序包。

打开项目，并在**常规**设置页的属性页将**目标名称**属性设置为``ShimLauncher32``或``ShimLauncher64``具体取决于您的应用程序的体系结构。

![填充码启动器参考 （英文）](images/desktop-to-uwp/shim-exe-reference.png)

在您的解决方案中添加对运行时修复项目的项目引用。

![运行时修复参考 （英文）](images/desktop-to-uwp/reference-fix.png)

引用中，右键单击，然后在**属性**窗口中，将这些值。

| 属性 | 值 |
|-------|-----------|-------|
| 复制本地 | True |
| 复制本地附属程序集 | True |
| 引用程序集输出 | True |
| 链接库依赖项 | False |
| 链接库依赖项输入 | False |

### <a name="configure-the-packaging-project"></a>配置打包项目

在打包项目中，右键单击**应用程序**文件夹，然后选择**添加引用**。

![添加项目引用](images/desktop-to-uwp/add-reference-packaging-project.png)

选择填充码启动器项目和您的桌面应用程序项目，然后选择**确定**按钮。

![桌面项目](images/desktop-to-uwp/package-project-references.png)

>[!NOTE]
> 如果您没有源代码您的应用程序，只需选择填充码启动器项目。 我们将向您如何创建配置文件时引用可执行文件。

在**应用程序**节点中，右键单击填充码启动器应用程序，，然后选择**设置为入口点**。

![设置入口点](images/desktop-to-uwp/set-startup-project.png)

添加一个名为文件``config.json``到打包项目，然后复制并粘贴到该文件中的以下 json 文本。 **包操作**属性设置为**内容**。

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
对于每个键中提供的值。 使用此表作为指南。

| 数组 | key | 值 |
|-------|-----------|-------|
| applications | id |  使用的值`Id`属性的`Application`程序包清单中的元素。 |
| applications | 可执行文件 | 指向要启动的可执行文件的相对于程序包的路径。 在大多数情况下，您可以从您程序包的清单文件获取此值之前对其进行修改。 它是的值`Executable`属性的`Application`元素。 |
| applications | workingDirectory | （可选）要用作启动的应用程序的工作目录相对于程序包的路径。 如果不设置此值，使用操作系统`System32`作为应用程序的工作目录的目录。 |
| 流程 | 可执行文件 | 在大多数情况下，这将的名称`executable`上方与删除的路径和文件扩展名配置。 |
| 填充 | dll | 相对于程序包的路径填充码加载 DLL。 |
| 填充 | 配置 | （可选）控制填充码 dl 的行为方式。 此值的准确格式发生变化填充码通过填充码基于每个填充码可将此"blob"解释为其想。 |

完成后，您``config.json``文件将如下所示。

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
> `applications`， `processes`，和`shims`密钥的数组。 这意味着您可以使用 config.json 文件来指定多个应用程序、 过程和填充码 DLL。

### <a name="debug-a-runtime-fix"></a>调试运行时修复

在 Visual Studio 中，按 F5 启动调试程序。  启动首先是填充码启动器应用程序，这又启动目标桌面应用程序。  若要调试目标桌面应用程序，您将需要手动附加到桌面应用程序进程，通过选择**调试**->**附加到进程**，，然后选择应用程序过程。 若要允许使用本机 runtime 修复 DLL 的.NET 应用程序调试，选择 （混合模式下调试） 的托管代码和本机代码类型。  

一旦您已对此进行设置，您可以桌面应用程序代码和运行时修复项目中设置断点旁边的代码行。 如果您没有源代码您的应用程序，您将能够在运行时修复项目中设置断点仅旁边的代码行。

>[!NOTE]
> 在 Visual Studio 为您提供了最简单的开发和调试体验，有一些限制，因此更高版本在本指南中，我们将讨论可应用其他调试技术。

## <a name="create-a-runtime-fix"></a>创建运行时修复

如果没有尚未运行时修复的问题，您想要解析，您可以通过编写替换功能，并包括任何配置数据创建新的运行时修复的有意义。 让我们看一下每个部件。

### <a name="replacement-functions"></a>替换功能

首先，确定哪个函数的调用失败 MSIX 容器中的应用程序运行时。 然后，您可以创建您希望运行时经理而调用的替换功能。 这使您能够函数的实现替换符合的现代运行时环境的规则的行为。

在 Visual Studio 中，打开您之前在本指南中创建的运行时修复项目。

声明``SHIM_DEFINE_EXPORTS``宏，然后添加的 include 语句`shim_framework.h`顶部的每个。要添加的运行时修复函数 CPP 文件。

```c++
#define SHIM_DEFINE_EXPORTS
#include <shim_framework.h>
```
>[!IMPORTANT]
>确保`SHIM_DEFINE_EXPORTS`宏显示之前 include 语句。

创建具有相同的函数的签名的函数谁具有您要修改的行为。 下面是示例函数，用于替换`MessageBoxW`函数。

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

调用`DECLARE_SHIM`映射`MessageBoxW`到新替换函数的函数。 当您的应用程序尝试调用`MessageBoxW`函数，它将调用替换函数相反。

#### <a name="protect-against-recursive-calls-to-functions-in-runtime-fixes"></a>防御递归调用的函数中运行时修补程序

（可选） 可以应用`reentrancy_guard`类型设置为您防御递归中运行时修补程序的函数调用的函数。

例如，您可能会产生替换函数的`CreateFile`函数。 您的实现可能会调用`CopyFile`函数，但的实现`CopyFile`函数可能会调用`CreateFile`函数。 这可能会导致调用的无限递归周期`CreateFile`函数。

有关详细信息`reentrancy_guard`，请参阅[authoring.md](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/Authoring.md)

### <a name="configuration-data"></a>配置数据

如果您想要添加到运行修复的配置数据，请考虑将其添加到``config.json``。 这样，您可以使用`ShimQueryCurrentDllConfig`轻松分析该数据。 本示例将分析该配置文件从一个 boolean 类型的值和字符串值。

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

## <a name="other-debugging-techniques"></a>其他调试技术

Visual Studio 为您提供了最简单的开发和调试体验，而不存在一些限制。

首先，F5 调试通过部署包布局文件夹路径，从松散文件，而不是从.appx 包安装运行应用程序。  布局文件夹通常不具有相同的安全限制为已安装的程序包文件夹。 因此，它可能无法以重现之前将运行时修复应用程序包路径访问拒绝错误。

若要解决此问题，使用.appx 程序包部署，而不是 F5 松散文件部署。  若要创建.appx 包文件，请使用由[MakeAppx](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/make-appx-package--makeappx-exe-)实用程序从 Windows SDK，如上面所述。 或者，从 Visual Studio 中右键单击您的应用程序项目节点并选择**存储**->**创建应用程序包**。

使用 Visual studio 创建的另一个问题是它不具有对附加到由调试器启动任何子进程的内置支持。   这样就变得比较困难调试的目标应用程序，必须手动连接由 Visual Studio 在启动后启动路径中的逻辑。

若要解决此问题，使用支持子进程调试器附加。  请注意，它通常不可能在实时 (JIT) 调试器附加到的目标应用程序。  这是因为大多数 JIT 技术涉及启动调试程序代替目标应用程序，通过 ImageFileExecutionOptions 注册表项。  这就违背了 ShimLauncher.exe 用于 ShimRuntime.dll 注入目标应用程序的 detouring 机制。  附加 WinDbg，包括在[Windows 调试工具](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/index)中，并从[Windows SDK](https://developer.microsoft.com/en-US/windows/downloads/windows-10-sdk)，获得支持子进程。  它现在还支持直接[启动和调试 UWP 应用程序](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-a-uwp-app-using-windbg#span-idlaunchinganddebuggingauwpappspanspan-idlaunchinganddebuggingauwpappspanspan-idlaunchinganddebuggingauwpappspanlaunching-and-debugging-a-uwp-app)。

若要调试作为子流程的目标应用程序启动，启动``WinDbg``。

```
windbg.exe -plmPackage PSFSampleWithFixup_1.0.59.0_x86__7s220nvg1hg3m -plmApp PSFSample
```

在``WinDbg``提示、 启用子调试并设置适当的断点。

```
.childdbg 1
g
```
（执行，直到目标应用程序启动，并进入调试器）

```
sxe ld fixup.dll
g
```
（直到修正加载 DLL 执行）

```
bp ...
```

>[!NOTE]
> [PLMDebug](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/plmdebug)还可用来将调试器附加到在启动时应用程序和，也包含[Windows 调试工具](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/index)。  但是，就更加复杂，比现在提供 WinDbg 的直接支持使用。

## <a name="support-and-feedback"></a>支持和反馈

**查找问题的答案**

有问题？ 请在 Stack Overflow 上向我们提问。 我们的团队会监视这些[标记](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 你还可以在[此处](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)提问。
