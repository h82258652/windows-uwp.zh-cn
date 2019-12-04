---
Description: 使用包资源索引（PRI） Api，可以为 UWP 应用的资源开发自定义生成系统。 生成系统可以创建、版本和转储 PRI 文件，以适应 UWP 应用所需的任何级别的复杂性。
title: 包资源索引和自定义生成系统
template: detail.hbs
ms.date: 05/07/2018
ms.topic: article
keywords: windows 10, uwp, 资源, 图像, 资产, MRT, 限定符
ms.localizationpriority: medium
ms.openlocfilehash: ebbec3a89d795dde5c47d3dce76f77be06feebe2
ms.sourcegitcommit: ae9c1646398bb5a4a888437628eca09ae06e6076
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2019
ms.locfileid: "74734922"
---
# <a name="package-resource-indexing-pri-apis-and-custom-build-systems"></a>包资源索引 (PRI) API 和自定义生成系统
通过[包资源索引 (PRI) API](https://docs.microsoft.com/windows/desktop/menurc/pri-indexing-reference)，你可以为 UWP 应用的资源开发自定义生成系统。 生成系统可以创建、改编和转储（作为 XML）包资源索引 (PRI) 文件，以适应 UWP 应用所需的任何级别的复杂性。 如果具有当前使用 MakePri.exe 命令行工具的自定义生成系统（请参阅[使用 MakePri.exe 手动编译资源](makepri-exe-command-options.md)），为了提供更佳的性能和控制，我们建议更改为调用 PRI API，而不是调用 MakePri.exe。

适用于 Windows 10 版本 1803 的 Windows SDK 中引入了 PRI API。 该 API 采用 Win32 Windows API 的形式，这意味着你拥有一些用于调用它们的选项。 可以直接从 Win32 应用中对它们进行调用，也可以从 .NET 应用或甚至从 UWP 应用通过[平台调用](/dotnet/framework/interop/consuming-unmanaged-dll-functions?branch=live)对它们进行调用。

本主题中的方案演示如何从 Win32 Visual C++ Windows 控制台应用程序项目调用 PRI API。 有关背景信息，请参阅[资源管理系统](resource-management-system.md)。

PRI 文件的大小限制为 64 千字节。

> [!NOTE]
> 此警告不太可能引发问题，因为你通常不会将自定义构建系统应用提交到 Microsoft Store。 但是，如果选择以 UWP 应用的形式开发自定义构建系统的选项，则它将成为异常 UWP 应用，你无法将其提交到 Microsoft Store。 这是因为使用平台调用的 UWP 应用无法通过 Microsoft Store 认证。 注意，在这种情况下，平台调用*将仅存在于自定义生成系统中*；而*不* 存在于发布的 UWP 应用（正在为其生成 PRI 文件）中。

## <a name="scenario-walkthroughs"></a>方案演练
|主题|描述|
|-|-|
|[方案1：从字符串资源和资产文件生成 PRI 文件](pri-apis-scenario-1.md)|在此方案中，我们将让新应用来代表我们的自定义生成系统。 我们将创建一个资源索引器，并向其添加字符串和其他类型的资源。 然后我们将生成和转储 PRI 文件。|

## <a name="important-apis"></a>重要的 API
* [包资源索引（PRI）参考](https://docs.microsoft.com/windows/desktop/menurc/pri-indexing-reference)

## <a name="related-topics"></a>相关主题
* [使用 MakePri.exe 手动编译资源](makepri-exe-command-options.md)
* [使用非托管 DLL 函数](/dotnet/framework/interop/consuming-unmanaged-dll-functions?branch=live)
* [资源管理系统](resource-management-system.md)
