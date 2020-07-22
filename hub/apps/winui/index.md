---
title: Windows UI 库 (WinUI)
description: 适用于 Windows 应用开发的 WinUI 库。
ms.topic: article
ms.date: 07/15/2020
keywords: windows 10, uwp, 工具包 sdk, winui, Windows UI 库
ms.openlocfilehash: 54b2d44dab1c311e6d1b75d0be35ed419056a953
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86492972"
---
# <a name="windows-ui-library-winui"></a>Windows UI 库 (WinUI)

![WinUI 徽标](../images/logo-winui.png)

Windows UI 库 (WinUI) 是适用于 Windows 桌面和 UWP 应用程序的本机用户体验 (UX) 框架。

通过将 [Fluent Design 系统](https://www.microsoft.com/design/fluent/#/)整合到所有体验、控件和样式中，WinUI 使用最新的用户界面 (UI) 模式提供一致、直观且可访问的体验。

由于同时支持桌面应用和 UWP 应用，因此你可以使用 WinUI 从头构建应用，也可以使用熟悉的语言（例如 C++、C#、Visual Basic）或通过[用于 Windows 的 React Native](https://microsoft.github.io/react-native-windows/) 使用 Javascript 逐步迁移现有的 MFC、WinForms 或 WPF 应用。

> [!Important]
> WinUI 有两个版本：WinUI 2.x 和 WinUI 3 。

## <a name="windows-ui-2x-library"></a>Windows UI 2.x 库

WinUI 2.x 可以在 UWP 应用程序中使用，并可通过 [XAML Islands](/windows/apps/desktop/modernize/xaml-islands) 纳入到新的或现有的桌面应用程序中。

> [!NOTE]
> WinUI 2.4 是最新的 WinUI 2.x 版本。 请参阅 [WinUI 2.5 里程碑](https://github.com/microsoft/microsoft-ui-xaml/milestone/10)，了解下一版中计划的工作列表。

WinUI 2.x 库与 [Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk/) 紧密耦合，为 UWP 应用提供正式的原生 Windows UI 控件和其他 UI 元素。

WinUI 2.x 控件保持与早期版本的 Windows 10 的低端兼容性，即使用户没有最新 OS，此类控件也可以运行。

![WinUI 2.x 平台支持](../images/platforms-winui2.png)

有关安装说明，请参阅 [Windows UI 库入门](winui2/getting-started.md)。

### <a name="related-links-for-winui-2x"></a>WinUI 2.x 的相关链接

- [WinUI 2.x 库概述](winui2/index.md)
- [API 文档](https://docs.microsoft.com/uwp/api/overview/winui/)
- [源代码](https://aka.ms/winui)
- [XAML 控件库应用](https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt)

## <a name="windows-ui-3-library-preview-2"></a>Windows UI 3 库（预览版 2）

WinUI 3 是 WinUI 的下一版本，它是与 [Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk/) 完全分离的本机 Windows 10 UI 平台。

> [!Important]
> 此 WinUI 3 预览版用于早期评估以及从开发人员社区收集反馈。 它**不**应该用于生产应用。
>
> 我们将在 2020 年至 2021 年初继续发布 WinUI 3 的预览版本，之后将发布第一个正式版本。
>
> 请使用 [WinUI GitHub 存储库](https://github.com/microsoft/microsoft-ui-xaml)提供反馈、报告问题并提出建议。

通过将 XAML、合成以及输入 API 从 [Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk/) 中完全分离，使 WinUI 3 涵盖完整的 Windows 10 本机 UI 平台。

WinUI 是所有 Windows 应用的转发路径，可以将其用作本机 UWP 或 Win32 应用上的 UI 层，也可以通过 [XAML Islands](https://docs.microsoft.com/windows/apps/desktop/modernize/xaml-islands) 一点点地实现桌面应用的现代化。

所有新的 XAML 功能最终都将作为 WinUI 的一部分发布。 作为 OS 的一部分发布的现有 UWP XAML API 将不会再收到新的功能更新。 但是，它们会在 Windows 10 支持生命周期内继续收到安全更新和关键修复程序。

请注意，通用 Windows 平台不止仅包含 XAML 框架。 还将继续开发并支持其他功能（例如应用程序和安全模型、媒体管道、Xbox 和 Windows 10 shell 集成）以及与各种设备的兼容性。

![WinUI 3 平台支持](../images/platforms-winui3.png)

### <a name="related-links-for-winui-3"></a>WinUI 3 的相关链接

- [Windows UI 库 3 预览版 2（2020 年 7 月）](winui3/index.md)
- [XAML 控件库（WinUI 3 预览版 2）应用](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3preview)

## <a name="winui-resources"></a>WinUI 资源

**GitHub**：WinUI 是托管在 GitHub 上的一个开源项目。 使用 [WinUI 存储库](https://github.com/microsoft/microsoft-ui-xaml)提交功能请求或 bug，与 WinUI 团队互动，并通过[路线图](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md)查看该团队针对 WinUI 3 和更高版本的计划。

**网站**：[WinUI 网站](https://aka.ms/winui)进行了产品比较，说明了 WinUI 的各种优势并有助于为产品献计献策及与产品团队保持联系。
