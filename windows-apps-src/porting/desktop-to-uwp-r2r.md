---
author: rido-min
Description: This guide explains how to configure your Visual Studio Solution to optimize the application binaries with native images.
Search.Product: eADQiWindows 10XVcnh
title: 优化你的.NET 桌面应用使用本机映像
ms.author: normesta
ms.date: 06/11/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，编译器的本机映像
ms.localizationpriority: medium
ms.openlocfilehash: d98b576fb51a8f9507802796ab359d0d00d21998
ms.sourcegitcommit: c8f6866100a4b38fdda8394ea185b02d7af66411
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/13/2018
ms.locfileid: "3964731"
---
# <a name="optimize-your-net-desktop-apps-with-native-images"></a>优化你的.NET 桌面应用使用本机映像

> [!NOTE]
> 与在商业发行之前可能会进行实质性修改的预发布产品相关的一些信息。 Microsoft 对于此处提供的信息不作任何明示或默示的担保。

可以通过预编译你的二进制文件来提高你的.NET Framework 应用程序的启动时间。 你可以使用此技术上的大型应用程序打包并通过 Windows 应用商店进行分配。 在某些情况下，我们已观测到 20%性能改进。 你可以了解有关此技术的[技术概述](https://github.com/dotnet/coreclr/blob/master/Documentation/botr/readytorun-overview.md)的详细信息。

我们已发布的本机映像编译器的预览版本作为[NuGet 程序包](https://www.nuget.org/packages/Microsoft.DotNet.Framework.NativeImageCompiler)。 你可以将此包应用到面向.NET Framework 版本 4.6.2 的任何.NET Framework 应用程序或更高版本。 此包添加 post 生成步骤，包括对你的应用程序使用的所有二进制文件的本机负载。 在应用程序运行在.NET 4.7.2 和更高版本的同时，以前的版本将仍可以加载的 MSIL 代码时，将加载此优化的负载。

[.NET framework 4.7.2](https://blogs.msdn.microsoft.com/dotnet/2018/04/30/announcing-the-net-framework-4-7-2/)包含在[Windows 10 2018 年 4 月更新](https://blogs.windows.com/windowsexperience/2018/04/30/how-to-get-the-windows-10-april-2018-update/)。 你还可以在电脑上的运行 Windows 7 + 和 Windows Server 2008 R2 + 安装此版本的.NET Framework。

> [!IMPORTANT]
> 如果你想要生成的 Windows 应用程序打包项目打包应用程序的本机映像，请确保将该项目的目标平台最低版本设置为 Windows 周年更新。

## <a name="how-to-produce-native-images"></a>如何生成本机映像

请按照以下说明来配置你的项目。

1. 将目标框架配置为 4.6.2 或以上

2. 将目标平台配置为 x86 或 x64 

3. 添加 NuGet 程序包。

4. 创建发布版本。

## <a name="configure-the-target-framework-as-462-or-above"></a>将目标框架配置为 4.6.2 或以上

若要配置你的项目面向.NET Framework 4.6.2 需要.NET Framework 4.6.2 开发工具或更高版本。 这些工具可以通过 Visual Studio 安装程序作为下的.NET 桌面开发工作负荷的可选组件：

![安装.NET 4.6.2 开发工具](images/desktop-to-uwp/install-4.6.2-devpack.png)

或者，你可以获取从.NET 开发人员包：[https://www.microsoft.com/net/download/visual-studio-sdks](https://www.microsoft.com/net/download/visual-studio-sdks)

## <a name="configure-the-target-platform-as-x86-or-x64"></a>将目标平台配置为 x86 或 x64

本机映像编译器优化给定平台的代码。 若要使用它，你需要配置你的应用程序面向一个特定的平台，如 x86 或 x64。

如果你的解决方案中有多个项目，仅入口点项目 （最有可能生成可执行文件） 必须进行编译为 x86 或 x64。 从主项目引用的其他二进制文件将会处理与在主项目中，指定的体系结构，即使它们作为 AnyCPU 编译。

若要配置你的项目：

1. 右键单击你的解决方案，然后选择**配置管理器**。

2. 选择 **< 新.>** 旁边的项目的生成可执行文件的名称在**平台**下拉菜单中。

3. 在**新建项目平台**对话框中，请确保，**从此处复制设置**下拉列表设置为**任何 CPU**。

![配置 x86](images/desktop-to-uwp/configure-x86.png)

重复此步骤中的`Release/x64`如果你想生成 x64 二进制文件。

>[!IMPORTANT]
> 由本机映像编译器不支持 AnyCPU 配置。

## <a name="add-the-nuget-packages"></a>添加 NuGet 程序包

你需要将添加到生成的可执行文件的 Visual Studio 项目的 NuGet 程序包形式提供的本机映像编译器。 这通常是 Windows 窗体或 WPF 项目。 使用此 PowerShell 命令执行该操作。

```PS
PM> Install-Package Microsoft.DotNet.Framework.NativeImageCompiler -Version 0.0.1-prerelease-00002  -PRE
```

> [!NOTE]
> 预览包的发布 NuGet.org 作为未列出。 通过浏览 NuGet.org 或使用 Visual Studio 中的程序包管理器用户界面，将不会找到它们。 但是，从程序包管理器控制台，安装它们，当你从另一台计算机还原。 当我们发布的第一个非预览版本，我们会将程序包完全进行访问。

## <a name="create-a-release-build"></a>创建一个发行版本

NuGet 包配置要运行另一种工具为发布版本的项目。 此工具将本机代码添加到相同的二进制文件。
若要验证工具已处理的二进制文件可以查看生成的输出以确保它包含一条消息，如这个：

```
Native image obj\x86\Release\\R2R\DesktopApp1.exe generated successfully.
```

## <a name="faq"></a>常见问题解答

**Q。 新的二进制文件执行操作在没有.NET Framework 4.7.2 的计算机上运行？**

A. 使用.NET Framework 4.7.2 运行时，优化的二进制文件将受益于改进。 运行以前的.NET framework 版本的客户端将从二进制文件加载非优化 MSIL 代码。

**Q。 如何提供反馈或报告问题？**

A. 通过在 Visual Studio 2017 的反馈工具报告问题。 [详细信息](https://docs.microsoft.com/visualstudio/ide/how-to-report-a-problem-with-visual-studio-2017)。

**Q。 什么是将本机映像添加到现有的二进制文件的影响？**

A. 优化的二进制文件包含托管和本机代码，使最终文件更高版本。

**Q。 我是否可以发布的二进制文件使用此技术？**

A. 此版本包含你现在可以使用投入许可证。
