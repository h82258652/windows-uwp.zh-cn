---
title: Windows UI 库的 NuGet 包列表
description: 列出 Windows UI 库的 NuGet 程序包
ms.topic: article
ms.date: 04/15/2020
keywords: windows 10, uwp, 工具包 sdk
ms.openlocfilehash: fdb11193273f7f6c54ef82076939c033d5a4168c
ms.sourcegitcommit: 6cdba316bdbd85a2429259ebfb59ff94440e234a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/02/2020
ms.locfileid: "85882867"
---
# <a name="windows-ui-library-nuget-packages"></a>Windows UI 库 NuGet 包

NuGet 是内置到 Visual Studio 中的 .Net 应用程序的标准包管理器。 在打开的解决方案中选择“工具”菜单，单击“NuGet 包管理器”，然后单击“管理解决方案的 NuGet 包...”，以便打开 UI。  输入下面的包名称之一，进行联机搜索。

| NuGet 包名称 | 说明 |
| --- | --- |
| [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml/) | 适用于 UWP 应用的控件。 包含以下命名空间中的 API：[Microsoft.UI.Xaml](/uwp/api/microsoft.ui.xaml)、[Microsoft.UI.Xaml.Automation.Peers](/uwp/api/microsoft.ui.xaml.automation.peers)、[Microsoft.Ui.Xaml.Controls](/uwp/api/microsoft.ui.xaml.controls)、[Microsoft.UI.Xaml.Controls.Primitives](/uwp/api/microsoft.ui.xaml.controls.primitives)、[Microsoft.UI.Xaml.CustomAttributes](/uwp/api/microsoft.ui.xaml.customattributes)、[Microsoft.UI.Xaml.Media](/uwp/api/microsoft.ui.xaml.media)、[Microsoft.Ui.Xaml.XamlTypeInfo](/uwp/api/microsoft.ui.xaml.xamltypeinfo) |
| [Microsoft.UI.Xaml.Core.Direct](https://www.nuget.org/packages/Microsoft.UI.Xaml.Core.Direct) | 允许在早期版本的 Windows 10 上使用 [XamlDirect](/uwp/api/microsoft.ui.xaml.core.direct.xamldirect) API，无需编写特殊代码来处理多个目标 Windows 10 版本。 |


## <a name="search-in-visual-studio"></a>在 Visual Studio 中搜索

在 Visual Studio 包管理器中搜索时，应该会看到与此类似的列表（版本号可能不同，但名称应相同）。

![NuGet 包管理器](images/NugetPackages.png)

## <a name="update-nuget-packages"></a>更新 NuGet 程序包

我们会定期更新 Windows UI 库，更新内容包括新的控件、服务、API 以及重要性更高的 Bug 修复。 若要确保使用最新版本，请在 Visual Studio 中打开项目，选择“工具”菜单，接着选择“NuGet 包管理器” -> “管理解决方案的 NuGet 包...”，然后选择“更新”选项卡。选择要更新的包，然后单击“安装”，更新到最新版本。

## <a name="getting-started"></a>入门

阅读 [Windows UI 库入门](getting-started.md)，详细了解如何在自己的项目中使用这些 NuGet 包。
