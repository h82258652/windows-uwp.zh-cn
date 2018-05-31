---
title: ARM32 UWP 应用疑难解答
author: msatranjr
description: 在 ARM 上运行 ARM32 应用的常见问题以及如何解决这些问题。
ms.author: misatran
ms.date: 01/18/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10 s, 始终连接, 基于 ARM 的 ARM32 应用, 基于 ARM 的 windows 10, 疑难解答
ms.localizationpriority: medium
ms.openlocfilehash: 71d92ec26311514e0eebdfa4a1dab39e86ce72fc
ms.sourcegitcommit: 11edca90aaf7856c762e68903483079d30ad3877
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/19/2018
ms.locfileid: "1595100"
---
# <a name="troubleshooting-arm32-uwp-apps"></a>ARM32 UWP 应用疑难解答
如果你的 ARM32 UWP 应用无法在 ARM 上正常工作，以下指南可能会有所帮助。 

## <a name="common-issues"></a>常见问题
在排除 ARM32 应用故障时，请牢记以下常见问题。

### <a name="using-windows-10-mobile-only-apis-on-arm-based-processors"></a>在基于 ARM 的处理器上使用只适用于 Windows 10 移动版的 API 
使用只适用于移动版的 API（例如 **HardwareButtons**）时，ARM32 应用可能会遇到问题。 若要缓解这种风险，你可以在调用此类 API 前动态检测你的应用是否运行在 Windows 10 移动版上。 请按照博客文章[使用 API 协定动态检测功能](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/)中的指南操作。

### <a name="including-dependencies-not-supported-by-uwp-apps"></a>包含 UWP 应用不支持的依赖关系
未正确使用 Visual Studio 和 UWP SDK 构建的通用 Windows 平台 (UWP) 应用可能会依赖于在 ARM64 系统上运行的 ARM32 应用无法使用的操作系统组件。 这些依赖关系的示例包括：

- 假设能够使用 .NET Framework 的某些部分。
- 引用与 UWP 不兼容的第三方 .NET 组件。

可通过以下方式解决此类问题：移除不可用的依赖关系并使用最新的 Microsoft Visual Studio 和 UWP SDK 版本重新构建应用；或者作为最后手段，从 Microsoft Store 中删除 ARM32 应用，从而让用户电脑下载应用的 x86 版本（如果可用）。 

有关可用于 UWP 应用的 .NET API 的详细信息，请参阅[适用于 UWP 应用的 .NET](https://msdn.microsoft.com/library/windows/apps/mt185501.aspx)

### <a name="compiling-an-app-with-an-older-version-of-visual-studio-and-sdk"></a>使用较早版本的 Visual Studio 和 SDK 编译应用
如果遇到问题，请务必使用最新版本的 Microsoft Visual Studio 和 Windows SDK 编译你的应用。 使用早期版本的 Visual Studio 和 SDK 编译的应用可能存在已在更高版本中修复的问题。

## <a name="debugging"></a>调试
你可以使用现有工具为 ARM 平台开发 ARM32 应用。 下面是一些有用的资源。

- Visual Studio 15.5 Preview 1 及更高版本支持使用通用身份验证模式运行 ARM32 应用。 这将自动启动必要的远程调试工具。
- 有关在 ARM 上进行调试的工具和策略的详细信息，请参阅[在 ARM64 上进行调试](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-arm64)。