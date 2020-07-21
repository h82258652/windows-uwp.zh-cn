---
title: Windows UI 库
description: 提供了有关 WinUI 2.x 和 Windows 应用开发的信息。
ms.topic: article
ms.date: 04/15/2020
keywords: windows 10, uwp, 工具包 sdk, winui, Windows UI 库
ms.custom: RS5
ms.openlocfilehash: c1828405c424ca54dcb70e587479fd5307b1046d
ms.sourcegitcommit: 3a7f9f05f0127bc8e38139b219e30a8df584cad3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/21/2020
ms.locfileid: "83775861"
---
# <a name="windows-ui-library-2x"></a>Windows UI 库 2.x

![WinUI 控件](images/winUI-library-767.png)

Windows UI 库为 Windows 应用提供官方的本机 Windows UI 控件和其他用户界面元素。

它保持与早期版本的 Windows 10 的底层兼容性。因此，即使用户没有最新 OS，你的应用也可以工作。

> [!NOTE]
> 请查看 [WinUI 3.0 预览版 1](../winui3/index.md)，它是计划于 2020 年发布的 Windows 10 UI 平台的重大更新。

## <a name="features"></a>功能

* **新控件**：Windows UI 库包含不作为默认 Windows 平台的一部分提供的新控件。

* **现有控件的更新版本**：该库还包含现有 Windows 平台控件的更新版本，这些版本可以与早期版 Windows 10 配合使用。

* **对早期版 Windows 10 的支持**：Windows UI 库 API 可以在较早的 Windows 10 版本上运行，因此你不需要添加版本检查或条件 XAML 来支持那些可能未运行最新 OS 的用户。

* **对 XamlDirect 的支持**：XamlDirect API 是为中间件开发人员设计的，可让你访问较低级别的 Xaml 功能，从而提供更好的 CPU 和工作集性能。 有了 XamlDirect，就可以在早期版本的 Windows 10 上使用 XamlDirect API，无需编写特殊代码来处理多个目标 Windows 10 版本。

## <a name="examples"></a>示例

Xaml 控件库示例应用包括介绍如何使用 WinUI 控件的交互式演示和示例代码。

* 从 [Microsoft Store](
https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt) 安装 XAML 控件库应用

* Xaml 控件库也[在 GitHub 上开源](
https://github.com/Microsoft/Xaml-Controls-Gallery)

## <a name="documentation"></a>文档

Windows UI 库控件的操作方法文章包含在[通用 Windows 平台控件文档](/windows/uwp/design/controls-and-patterns/)中。

API 参考文档位于此处：[Windows UI 库 API](/uwp/api/overview/winui/)。

## <a name="install-and-use-the-windows-ui-library"></a>安装并使用 Windows UI 库

有关说明，请参阅 [Getting started with the Windows UI Library](getting-started.md)（Windows UI 库入门）。

## <a name="open-source-and-developer-roadmap"></a>开源和开发人员路线图

WinUI 是托管在 GitHub 上的开源项目。 我们欢迎你在 [Windows UI 库存储库](https://aka.ms/winui)中提供 Bug 报告、提交功能请求和贡献社区代码。

我们会继续开发和发展 WinUI，为更多开发人员方案提供支持。 有关我们的 WinUI 计划的最新详细信息，请参阅 Windows UI 库存储库中的[路线图](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md)。

## <a name="nuget-package-list"></a>NuGet 包列表

Windows UI 库包含多个 NuGet 包：[Windows UI 库 NuGet 包列表](nuget-packages.md)。

## <a name="see-also"></a>另请参阅

[Windows UI 库 2.x 发行说明](release-notes/index.md)
