---
title: 使用 winget 工具安装和管理应用程序
description: 开发人员可以在 Windows 10 计算机上使用 winget 命令行工具来发现、安装、升级、删除和配置应用程序。
ms.date: 04/28/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: 3b3f108de117fb937a7a670497a4a1a1d5810aca
ms.sourcegitcommit: 4df8c04fc6c22ec76cdb7bb26f327182f2dacafa
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/24/2020
ms.locfileid: "85334535"
---
# <a name="use-the-winget-tool-to-install-and-manage-applications"></a>使用 winget 工具安装和管理应用程序

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

开发人员可以在 Windows 10 计算机上使用 **winget** 命令行工具来发现、安装、升级、删除和配置应用程序。 此工具是 Windows 程序包管理器服务的客户端接口。

**winget** 工具当前为预览版，因此目前并不是所有已计划的功能都可用。

## <a name="install-winget"></a>安装 winget

可使用多种方法安装 **winget** 工具：

* [Windows 应用安装程序](https://www.microsoft.com/p/app-installer/9nblggh4nns1?ocid=9nblggh4nns1_ORSEARCH_Bing&rtc=1&activetab=pivot:overviewtab)的外部测试版或预览版中包含 **winget** 工具。 必须安装**应用安装程序**的预览版本才能使用 **winget**。 若要获取提前访问权限，请将你的请求提交到 [Windows 程序包管理器预览体验计划](https://aka.ms/AppInstaller_InsiderProgram)。 参与外部测试版 Ring 将保证你可以看到最新的预览版更新。

* 参与 [Windows 外部测试版 Ring](https://insider.windows.com)。

* 安装位于 [winget 存储库](https://github.com/microsoft/winget-cli)的 release 文件夹中的 Windows 桌面应用安装程序包。

> [!NOTE]
> **winget** 工具需要 Windows 10 版本 1709 (10.0.16299) 或更高版本的 Windows 10。

## <a name="administrator-considerations"></a>管理员注意事项

安装程序的行为可能会有所不同，具体取决于你是否是以管理员权限运行 **winget**。

* 在没有管理员权限的情况下运行 **winget** 时，某些应用程序可能会[要求提升权限](https://docs.microsoft.com/windows/security/identity-protection/user-account-control/)才能进行安装。 当安装程序运行时，Windows 会提示你[提升权限](https://docs.microsoft.com/windows/security/identity-protection/user-account-control)。 如果你选择不提升权限，则应用程序无法进行安装。  

* 在管理员命令提示符下运行 **winget** 时，如果应用程序要求你提升权限，你不会看到[提升权限提示](https://docs.microsoft.com/windows/security/identity-protection/user-account-control/how-user-account-control-works)。 以管理员身份运行命令提示符时请务必小心，仅安装你信任的应用程序。

## <a name="use-winget"></a>使用 winget

安装**应用安装程序**后，可以通过在命令提示符下键入“winget”来运行 **winget**。

最常见的使用场景之一是搜索并安装你最喜欢的工具。

1. 若要[搜索](search.md)某个工具，请键入 `winget search \<appname>`。
2. 确认你需要的工具可用后，可以通过键入 `winget install \<appname>` 来[安装](install.md)该工具。 **winget** 工具会启动安装程序，将应用程序安装在你的电脑上。
    ![winget 命令行](images\install.png)

3. 除了安装和搜索外，**winget** 还提供了许多其他命令，用来[显示应用程序详细信息](show.md)，[更改源](source.md)以及[验证程序包](validate.md)。 若要获取完整的命令列表，请键入 `winget --help`。
    ![winget help](images\help.png)

### <a name="commands"></a>命令

**winget** 工具的当前预览版支持以下命令。

| 命令 | 说明 |
|---------|-------------|
| [hash](hash.md) | 为安装程序生成 SHA256 哈希。 |
| [help](help.md) | 显示 **winget** 工具命令的帮助信息。 |
| [install](install.md) | 安装指定的应用程序。 |
| [search](search.md) | 搜索某个应用程序。 |
| [show](show.md) | 显示指定应用程序的详细信息。 |
| [source](source.md) | 添加、删除和更新 **winget** 工具访问的 Windows 程序包管理器存储库。 |
| [validate](validate.md) | 验证要提交到 Windows 程序包管理器存储库的清单文件。 |

### <a name="options"></a>选项

**winget** 工具的当前预览版支持以下选项。

| 选项 | 说明 |
|--------------|-------------|
| **-v、--version** | 此选项返回 winget 的当前版本。 |
| **--info** |  info 提供有关 winget 的所有详细信息，包括许可证和隐私声明的链接。 |
| **-?、--help** |  获取有关 winget 的更多帮助信息 |

## <a name="supported-installer-formats"></a>支持的安装程序格式

**winget** 工具的当前预览版支持以下类型的安装程序。

* EXE
* MSIX
* MSI

## <a name="scripting-winget"></a>编写 winget 脚本

可以编写批处理脚本和 powershell 脚本来安装多个应用程序。

``` CMD
@echo off  
Echo Install Powertoys and Terminal  
REM Powertoys  
winget install Microsoft.Powertoys  
if %ERRORLEVEL% EQU 0 Echo Powertoys installed successfully.  
REM Terminal  
winget install Microsoft.WindowsTerminal  
if %ERRORLEVEL% EQU 0 Echo Terminal installed successfully.   %ERRORLEVEL%
```

> [!NOTE]
> 使用脚本时，**winget** 会按指定顺序启动应用程序。 当安装程序返回成功或失败时，**winget** 会启动下一个安装程序。 如果某个安装程序启动了另一进程，它可能会提前返回到 **winget**。 这会导致 **winget** 在上一个安装程序完成之前安装下一个安装程序。

## <a name="missing-tools"></a>缺少工具

如果[社区存储库](../package/repository.md)未包含你的工具或应用程序， 请将程序包提交到我们的[存储库](https://github.com/microsoft/winget-pkgs)。 添加你最喜爱的工具后，你和其他人都可以使用它。

## <a name="open-source-details"></a>开源详细信息

**winget** 工具是 GitHub 上的存储库 [https://github.com/microsoft/winget-cli/](https://github.com/microsoft/winget-cli/) 中提供的一个开源软件。 用于构建客户端的源代码位于 [src 文件夹](https://github.com/microsoft/winget-cli/tree/master/src)中。

**winget** 的源代码包含在 Visual Studio 2019 C++ 解决方案中。 若要正确构建解决方案，请安装最新的[包含 C++ 工作负荷的 Visual Studio](https://visualstudio.microsoft.com/downloads/)。

我们鼓励你为 GitHub 上的 **winget** 源代码贡献力量。 你必须先同意并签署 Microsoft CLA。
