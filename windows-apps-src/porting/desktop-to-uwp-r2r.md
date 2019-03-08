---
Description: 本指南介绍如何配置 Visual Studio 解决方案来优化本机映像使用的应用程序二进制文件。
Search.Product: eADQiWindows 10XVcnh
title: 优化用于本机映像.NET 桌面应用
ms.date: 06/11/2018
ms.topic: article
keywords: windows 10 中，编译器的本机映像
ms.localizationpriority: medium
ms.openlocfilehash: 3071b843a1605d765ab5b087d5e1bfb96a220218
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57642872"
---
# <a name="optimize-your-net-desktop-apps-with-native-images"></a>优化用于本机映像.NET 桌面应用

> [!NOTE]
> 与在商业发行之前可能会进行实质性修改的预发布产品相关的一些信息。 Microsoft 不对此处提供的信息作任何明示或默示的担保。

可以通过预编译二进制文件的位置来提高您的.NET Framework 应用程序的启动时间。 在大型应用程序打包并通过 Microsoft Store 分发，可以使用此技术。 在某些情况下，我们已发现 20%的性能改善。 你可以了解有关此技术的详细[技术概述](https://github.com/dotnet/coreclr/blob/master/Documentation/botr/readytorun-overview.md)。

我们发布了预览版本的本机映像编译器作为[NuGet 包](https://www.nuget.org/packages/Microsoft.DotNet.Framework.NativeImageCompiler)。 可以将此包应用到任何面向.NET Framework 版本 4.6.2 的.NET Framework 应用程序或更高版本。 此程序包将添加包含到应用程序所使用的所有二进制文件的本机有效负载的 post 生成步骤。 在应用程序运行在.NET 4.7.2 及更高版本上一版本仍将加载的 MSIL 代码时，将加载此优化的有效负载。

[.NET framework 4.7.2](https://blogs.msdn.microsoft.com/dotnet/2018/04/30/announcing-the-net-framework-4-7-2/)中包含[Windows 10 2018 年 4 月更新](https://blogs.windows.com/windowsexperience/2018/04/30/how-to-get-the-windows-10-april-2018-update/)。 此外可以在 PC 上的运行 Windows 7 + 和 Windows Server 2008 R2 及更高的安装此版本的.NET Framework。

> [!IMPORTANT]
> 如果您想要生成本机映像为 Windows 应用程序打包项目的打包应用程序，请确保将项目的目标平台最低版本设置为 Windows 周年更新。

## <a name="how-to-produce-native-images"></a>如何生成本机映像

按照这些说明配置你的项目。

1. 配置目标框架为 4.6.2 或更高版本

2. 将目标平台配置为 x86 或 x64 

3. 添加 NuGet 包。

4. 创建发布版本。

## <a name="configure-the-target-framework-as-462-or-above"></a>配置目标框架为 4.6.2 或更高版本

若要配置你的项目面向.NET Framework 4.6.2 到需要.NET Framework 4.6.2 开发工具或更高版本。 作为.NET 桌面开发工作负荷下的可选组件，这些工具是可通过 Visual Studio 安装程序：

![安装.NET 4.6.2 的开发工具](images/desktop-to-uwp/install-4.6.2-devpack.png)

或者，可以获取中的.NET 开发人员包： [https://www.microsoft.com/net/download/visual-studio-sdks](https://www.microsoft.com/net/download/visual-studio-sdks)

## <a name="configure-the-target-platform-as-x86-or-x64"></a>将目标平台配置为 x86 或 x64

本机映像编译器优化给定平台的代码。 若要使用它，需要应用程序配置为一个特定的平台，如 x86 或 x64。

如果你的解决方案中有多个项目，只将入口点项目 （很可能会生成可执行文件是项目） 必须编译为 x86 或 x64。 在主项目中指定的体系结构将处理从主项目中引用的其他二进制文件，即使它们编译为 AnyCPU。

若要配置你的项目：

1. 右键单击解决方案，然后依次**Configuration Manager**。

2. 选择 **< 新...>** 中**平台**生成可执行文件的项目名称旁的下拉列表菜单。

3. 在中**新建项目平台**对话框框中，请确保**从此处复制设置**下拉列表设置为**任何 CPU**。

![配置 x86](images/desktop-to-uwp/configure-x86.png)

重复此步骤为`Release/x64`如果您希望生成 x64 二进制文件。

>[!IMPORTANT]
> 本机映像编译器不支持 AnyCPU 配置。

## <a name="add-the-nuget-packages"></a>添加 NuGet 包

本机映像编译器需要将添加到生成的可执行文件的 Visual Studio 项目的 NuGet 包的形式提供。 这通常是你的 Windows 窗体或 WPF 项目。 使用此 PowerShell 命令来执行该操作。

```PS
PM> Install-Package Microsoft.DotNet.Framework.NativeImageCompiler -Version 0.0.1-prerelease-00002  -PRE
```

> [!NOTE]
> 预览包作为未列出 NuGet.org 中发布。 通过浏览 NuGet.org 或通过使用 Visual Studio 中的包管理器 UI 不会找到它们。 但是，可以从包管理器控制台安装它们，当你从另一台计算机还原。 当我们发布的第一个非预览版本时，我们将使包可以完全访问。

## <a name="create-a-release-build"></a>创建版本生成

NuGet 包配置项目以运行另一种工具适用于发行版本。 此工具将本机代码添加到相同的二进制文件。
若要验证该工具已处理的二进制文件可以查看生成的输出内容以确保它包含此类消息：

```
Native image obj\x86\Release\\R2R\DesktopApp1.exe generated successfully.
```

## <a name="faq"></a>常见问题

**问题与解答。新的二进制文件是否在计算机而无需.NET Framework 4.7.2 工作？**

A. 当运行使用.NET Framework 4.7.2，优化的二进制文件将从提升中获益。 运行以前的.NET framework 版本的客户端将从二进制文件加载的非优化 MSIL 代码。

**问题与解答。如何提供反馈或报告问题？**

A. 通过在 Visual Studio 2017 中使用反馈工具来报告问题。 [详细信息](https://docs.microsoft.com/visualstudio/ide/how-to-report-a-problem-with-visual-studio-2017)。

**问题与解答。将本机映像添加到现有的二进制文件的影响是什么？**

A. 优化的二进制文件包含托管和本机代码，使最终文件更高版本。

**问题与解答。可以发布使用此技术的二进制文件？**

A. 此版本包括您可以立即使用的 Go Live 许可证。
