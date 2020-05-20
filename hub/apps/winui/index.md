---
title: Windows UI 库 (WinUI)
description: 适用于 Windows 应用开发的 WinUI 库。
ms.topic: article
ms.date: 05/11/2020
keywords: windows 10, uwp, 工具包 sdk, winui, Windows UI 库
ms.openlocfilehash: 2afa6b1eadc98300e3de76a1dfc6ede66a2a56e5
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/19/2020
ms.locfileid: "83580174"
---
# <a name="windows-ui-library-winui"></a>Windows UI 库 (WinUI)

![工具包大图](../images/logo-winui.png)

Windows UI 库 (WinUI) 是适用于 Windows 应用（包括 Win32 和 UWP）的新式原生用户界面 (UI) 平台。

通过将 [Fluent Design 系统](https://www.microsoft.com/design/fluent/#/)整合到所有控件和样式中，WinUI 使用最新的 UI 模式提供一致、直观且可访问的体验。

由于同时支持 Win32 和 UWP 应用，因此你可以使用 WinUI 从头构建应用，也可以按照自己的节奏迁移现有的 MFC、WinForms 或 WPF 应用。在此过程中，你可以使用熟悉的语言，例如 C++、C#、Visual Basic，甚至可以通过[用于 Windows 的 React Native](https://microsoft.github.io/react-native-windows/) 来使用 Javascript。

需要注意 WinUI 的两个版本：**WinUI 2.x** 和 **WinUI 3.0**。

## <a name="windows-ui-2x-library"></a>Windows UI 2.x 库

WinUI 2.x 现在可以在 UWP 应用程序中使用，并可通过 [XAML 岛](/windows/apps/desktop/modernize/xaml-islands)纳入到现有的桌面应用程序中。

WinUI 2.x 库与 UWP SDK 紧密耦合，为 UWP 应用提供正式的原生 Windows UI 控件和其他 UI 元素。

WinUI 2.x 控件保持与早期版本的 Windows 10 的低端兼容性，即使用户没有最新 OS，此类控件也可以运行。

![WinUI 2.x 平台支持](../images/platforms-winui2.png)

> [!NOTE]
> WinUI 2.4 是最新的 WinUI 2.x 版本。 请参阅 [WinUI 2.5 里程碑](https://github.com/microsoft/microsoft-ui-xaml/milestone/10)，了解下一版中计划的工作列表。

有关安装说明，请参阅 [Windows UI 库入门](winui2/getting-started.md)。

### <a name="related-links"></a>相关链接

- [WinUI 2.x 库概述](winui2/index.md)
- [API 文档](https://docs.microsoft.com/uwp/api/overview/winui/)
- [源代码](https://aka.ms/winui)
- [XAML 控件库应用](https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt)

## <a name="windows-ui-30-library-preview-1"></a>Windows UI 3.0 库（预览版 1）

WinUI 3 是 WinUI 的下一版本，它是与 UWP SDK 完全分离的原生 Windows 10 UI 平台。

通过将 Xaml、合成以及输入 API 从 UWP SDK 中完全分离，Windows UI 3.0 库可以极大地扩展 WinUI 的范围，使之包括完整的 Windows 10 原生 UI 平台。

WinUI 充当所有 Windows 应用的转发路径 - 可以将其用作原生 UWP 或 Win32 应用上的 UI 层，也可以使用它通过 [Xaml 岛](https://docs.microsoft.com/windows/apps/desktop/modernize/xaml-islands)一点点地实现桌面应用的现代化。
 
> [!NOTE]
> WinUI 3.0 预览版 1 是 WinUI 3.0 的预发布版本。 欢迎在 [WinUI GitHub 存储库](https://github.com/microsoft/microsoft-ui-xaml)中提供反馈。

所有新的 Xaml 功能都将作为 WinUI 的一部分发布。 作为 OS 的一部分发布的现有 UWP Xaml API 将不会再收到新的功能更新。 但是，它们会在 Windows 10 支持生命周期内继续收到安全更新和关键修复程序。

通用 Windows 平台不仅包含 Xaml 框架，还包含其他功能（例如应用程序和安全模型、媒体管道、Xbox 和 Windows 10 shell 集成、广泛的设备支持），会继续得到支持。

![WinUI 3.0 平台支持](../images/platforms-winui3.png)

> [!Important]
> WinUI 3.0 预览版 1 用于早期评估以及从开发人员社区收集反馈。 它**不**应该用于生产应用。

### <a name="related-links"></a>相关链接

- [WinUI 3.0 预览版 1（2020 年 5 月）](winui3/index.md)
- [XAML 控件库（WinUI 3.0 预览版 1）应用](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3alpha)

## <a name="winui-resources"></a>WinUI 资源

**GitHub**：WinUI 是托管在 GitHub 上的一个开源项目。 在 [WinUI 存储库](https://github.com/microsoft/microsoft-ui-xaml)中，你可以提交功能请求或 bug，与 WinUI 团队互动，并通过[路线图](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md)查看该团队针对 WinUI 3 和更高版本的计划。

**网站**：[WinUI 网站](https://aka.ms/winui)提供了各种有用信息，涉及产品比较、WinUI 的优势，以及如何为产品献计献策并与产品团队保持联系。
