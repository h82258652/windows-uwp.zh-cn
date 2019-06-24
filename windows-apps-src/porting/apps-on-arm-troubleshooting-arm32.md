---
title: ARM32 UWP 应用疑难解答
description: 在 ARM 上运行 ARM32 应用的常见问题以及如何解决这些问题。
ms.date: 01/03/2019
ms.topic: article
keywords: windows 10 s, 始终连接, 基于 ARM 的 ARM32 应用, 基于 ARM 的 windows 10, 疑难解答
ms.localizationpriority: medium
ms.openlocfilehash: b93a4a3bdf4a6abd12a0df8bfc1871cb38647f63
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2019
ms.locfileid: "67319749"
---
# <a name="troubleshooting-arm-uwp-apps"></a>故障排除 ARM UWP 应用

如果你 ARM32 或 ARM64 UWP 应用程序未在 ARM 上正常工作，下面是一些指南，可帮助。

>[!NOTE]
> 若要生成 UWP 应用程序以本机面向 ARM64 的平台，你必须具有 Visual Studio 2017 版本 15.9 或更高版本。 有关详细信息，请参阅[这篇博客文章](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development/)。

## <a name="common-issues"></a>常见问题
以下是一些常见的问题进行故障排除 ARM32 和 ARM64 应用时，需要注意。

### <a name="using-windows-10-mobile-only-apis-on-arm-based-processors"></a>在基于 ARM 的处理器上使用只适用于 Windows 10 移动版的 API
ARM 应用可能会遇到问题，使用仅限移动的 Api 时 (例如， **HardwareButtons**)。 若要缓解这种风险，你可以在调用此类 API 前动态检测你的应用是否运行在 Windows 10 移动版上。 请按照博客文章[使用 API 协定动态检测功能](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/)中的指南操作。

### <a name="including-dependencies-not-supported-by-uwp-apps"></a>包含 UWP 应用不支持的依赖关系
未正确使用 Visual Studio 和 UWP SDK 中内置的通用 Windows 平台 (UWP) 应用程序可能依赖于不可用于 ARM 应用 ARM64 系统上运行的操作系统组件。 这些依赖关系的示例包括：

- 假设能够使用 .NET Framework 的某些部分。
- 引用与 UWP 不兼容的第三方 .NET 组件。

可通过解决这些问题： 删除不可用的依赖项和重新生成应用程序使用的最新的 Microsoft Visual Studio 和 UWP SDK 版本;或作为最后的手段，从 Microsoft Store 中，删除 ARM 应用，以便 x86 版本的应用 （如果可用） 下载到用户的 Pc。

有关可用于 UWP 应用的 .NET API 的详细信息，请参阅[适用于 UWP 应用的 .NET](https://docs.microsoft.com/dotnet/api/index?view=dotnet-uwp-10.0)

### <a name="compiling-an-app-with-an-older-version-of-visual-studio-and-sdk"></a>使用较早版本的 Visual Studio 和 SDK 编译应用
如果遇到问题，请务必使用最新版本的 Microsoft Visual Studio 和 Windows SDK 编译你的应用。 使用早期版本的 Visual Studio 和 SDK 编译的应用可能存在已在更高版本中修复的问题。

## <a name="debugging"></a>调试
用于开发面向 ARM 平台的应用，可以使用现有的工具。 下面是一些有用的资源。

- Visual Studio 15.5 Preview 1 及更高版本支持使用通用身份验证模式运行 ARM32 应用。 这将自动启动必要的远程调试工具。
- 有关在 ARM 上进行调试的工具和策略的详细信息，请参阅[在 ARM64 上进行调试](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-arm64)。
