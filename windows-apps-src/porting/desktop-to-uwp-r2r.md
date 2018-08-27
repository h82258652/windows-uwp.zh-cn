---
author: rido-min
Description: This guide explains how to configure your Visual Studio Solution to optimize the application binaries with native images.
Search.Product: eADQiWindows 10XVcnh
title: 优化.NET 桌面应用程序的本机图像
ms.author: normesta
ms.date: 06/11/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，本机映像编译器
ms.localizationpriority: medium
ms.openlocfilehash: d98b576fb51a8f9507802796ab359d0d00d21998
ms.sourcegitcommit: 753dfcd0f9fdfc963579dd0b217b445c4b110a18
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "2855271"
---
# <a name="optimize-your-net-desktop-apps-with-native-images"></a>优化.NET 桌面应用程序的本机图像

> [!NOTE]
> 与在商业发行之前可能会进行实质性修改的预发布产品相关的一些信息。 Microsoft 对于此处提供的信息不作任何明示或默示的担保。

您可以通过预编译二进制文件的位置来提高.NET Framework 应用程序的启动时间。 大型打包和分发通过 Windows 应用商店的应用程序上，可以使用此技术。 在某些情况下，我们已观察到 20%性能改进。 您可以了解有关此技术的[技术概述](https://github.com/dotnet/coreclr/blob/master/Documentation/botr/readytorun-overview.md)的详细信息。

作为[NuGet 程序包](https://www.nuget.org/packages/Microsoft.DotNet.Framework.NativeImageCompiler)，我们已发布的本机映像编译器预览版本。 您可以将此程序包应用于面向.NET Framework 版本 4.6.2 任何.NET Framework 应用程序或更高版本。 此软件包将添加 post 生成步骤，包括对您的应用程序使用的所有二进制文件的本机负载。 在应用程序运行在.NET 4.7.2，高于早期版本仍将加载 MSIL 代码时，将加载此优化的负载。

[.NET framework 4.7.2](https://blogs.msdn.microsoft.com/dotnet/2018/04/30/announcing-the-net-framework-4-7-2/)包含[Windows 10 年 4 月 2018年更新](https://blogs.windows.com/windowsexperience/2018/04/30/how-to-get-the-windows-10-april-2018-update/)。 您还可以在 PC 上的运行 Windows 7 + 和 Windows Server 2008 R2 + 安装此版本的.NET Framework。

> [!IMPORTANT]
> 如果您想要生成的 Windows 应用程序打包项目打包应用程序的本机图像，请务必设置为 Windows 周年日更新的项目的目标平台最低版本。

## <a name="how-to-produce-native-images"></a>如何生成本机图像

请按照以下说明配置您的项目。

1. 将目标框架配置大于或等于为 4.6.2

2. 配置为 x86 或 x64 的目标平台 

3. 添加 NuGet 程序包。

4. 创建发布版本。

## <a name="configure-the-target-framework-as-462-or-above"></a>将目标框架配置大于或等于为 4.6.2

若要配置您的项目与目标.NET Framework 4.6.2 您需要的.NET Framework 4.6.2 开发工具或更高版本。 作为.NET 桌面开发工作负荷下的可选组件，这些工具都可以通过 Visual Studio 安装程序:

![安装.NET 4.6.2 开发工具](images/desktop-to-uwp/install-4.6.2-devpack.png)

此外，您可以获取从.NET 开发人员包：[https://www.microsoft.com/net/download/visual-studio-sdks](https://www.microsoft.com/net/download/visual-studio-sdks)

## <a name="configure-the-target-platform-as-x86-or-x64"></a>配置为 x86 或 x64 的目标平台

本机映像编译器优化给定平台的代码。 若要使用它，您需要配置应用程序面向 x86 或 x64 如一个特定的平台。

如果您的解决方案中有多个项目，仅入口点项目 （很可能会生成一个可执行文件的项目） 具有编译为 x86 或 x64。 即使它们作为 AnyCPU 编译，将在主项目中指定的体系结构与处理其他二进制文件从主项目引用。

配置您的项目：

1. 右键单击您的解决方案，然后选择**配置管理器**。

2. 选择 **< 新正在>** 中生成可执行文件的项目的名称旁边的**平台**下拉列表菜单。

3. 在**新建项目平台**对话框中，确保，将**从此处复制设置**下拉列表设置为**任何 CPU**。

![配置 x86](images/desktop-to-uwp/configure-x86.png)

重复此步骤`Release/x64`如果您希望生成 x64 二进制文件。

>[!IMPORTANT]
> 不支持本机映像编译器 AnyCPU 配置。

## <a name="add-the-nuget-packages"></a>添加 NuGet 程序包

本机映像编译器作为您需要向 Visual Studio 项目生成的可执行文件中添加 NuGet 程序包。 这通常是您的 Windows 窗体或 WPF 项目。 使用以下 PowerShell 命令执行的。

```PS
PM> Install-Package Microsoft.DotNet.Framework.NativeImageCompiler -Version 0.0.1-prerelease-00002  -PRE
```

> [!NOTE]
> 预览包中 NuGet.org 发布为未列出。 通过浏览 NuGet.org 或通过使用 Visual Studio 中的程序包管理器 UI，您将不会发现它们。 但是，可以从程序包管理器控制台中，安装它们以及何时您还原从其他计算机。 当我们发布的第一个非预览版本，我们将进行包完全可访问。

## <a name="create-a-release-build"></a>创建发布版本

NuGet 程序包配置要运行此附加工具用于发布版本的项目。 此工具将本机代码添加到相同的二进制文件。
若要验证工具具有处理二进制文件，您可以查看生成的输出内容以确保它包括一条消息，如本之一：

```
Native image obj\x86\Release\\R2R\DesktopApp1.exe generated successfully.
```

## <a name="faq"></a>常见问题解答

**Q。 是否在不带.NET Framework 4.7.2 机工作的新的二进制文件？**

A. 使用.NET Framework 4.7.2 运行时，将从改进受益优化的二进制文件。 运行以前的.NET framework 版本的客户端将从二进制文件加载的非优化 MSIL 代码。

**Q。 如何提供反馈或报告问题？**

A. 在 Visual Studio 2017 中使用反馈工具报告的问题。 [详细信息](https://docs.microsoft.com/visualstudio/ide/how-to-report-a-problem-with-visual-studio-2017)。

**Q。 本机图像添加到现有的二进制文件的影响是什么？**

A. 优化的二进制文件包含托管代码和本机代码，使最终文件更高版本。

**Q。 可以发行版使用此技术的二进制文件**

A. 此版本包含您可以立即使用投入许可证。
