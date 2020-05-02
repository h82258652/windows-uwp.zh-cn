---
Description: 了解如何着手为 Windows 电脑构建桌面应用，包括如何选择适合新应用的应用平台，以及如何实现 Windows 10 现有应用的现代化。
title: 构建 Windows 电脑的桌面应用
ms.topic: article
ms.date: 11/04/2019
keywords: windows win32, 桌面开发
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: c27826980127674b4a4356af8a3c36056e86bd83
ms.sourcegitcommit: f727b68e86a86c94eff00f67ed79a1c12666e7bc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "74056862"
---
# <a name="build-desktop-apps-for-windows-pcs"></a>构建 Windows 电脑的桌面应用

本文提供了开始构建适合 Windows 的桌面应用或更新现有桌面应用以在 Windows 10 中采用最新体验所需的信息。

## <a name="platforms-for-desktop-apps"></a>适合桌面应用的平台

有 4 个主要平台可用于构建适合 Windows 电脑的桌面应用。 每个平台都提供定义应用生命周期的应用模型和一组完整的 UI 控件，你还可在平台上访问一组全面的托管或本机 API，它们可用来使用 Windows 功能。

下表介绍了这些平台。 要对这些平台进行深入比较并了解适合每个平台的其他资源，请参阅[选择应用平台](choose-your-platform.md)。

<br/>

<table>
<colgroup>
<col width="20%" />
<col width="60%" />
<col width="20%" />
</colgroup>
<thead>
<tr class="header">
<th>平台</th>
<th>说明</th>
<th>文档和资源</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><a href="https://docs.microsoft.com/windows/uwp/">通用 Windows 平台 (UWP)</a></td>
<td><p>适合 Windows 10 应用和游戏的领先平台。 可构建仅使用 UWP 控件和 API 的 UWP 应用，也可在桌面应用中使用 UWP 控件和 API，这些应用根据设计可使用其他平台之一。</p></td>
<td><a href="/windows/uwp/get-started/">入门</a><br/><a href="/uwp/">API 参考</a><br/><a href="https://github.com/Microsoft/Windows-universal-samples">示例</a></td>
</tr>
<tr class="even">
<td><a href="https://docs.microsoft.com/windows/win32/">Win32</a></td>
<td><p>适合需要直接访问 Windows 和硬件的本机 C/C++ Windows 应用的首选平台。</p></td>
<td><a href="/windows/win32/desktop-programming/">入门</a><br/><a href="/windows/win32/apiindex/windows-api-list/">API 参考</a><br/><a href="https://github.com/Microsoft/Windows-classic-samples">示例</a></td>
</tr>
<tr class="odd">
<td><a href="https://docs.microsoft.com/dotnet/framework/wpf/">WPF</a></td>
<td><p>已建立的基于 .NET 的平台，它适合带有 XAML UI 模型的图形丰富的托管 Windows 应用。 这些应用可面向 <a href="https://docs.microsoft.com/dotnet/core/whats-new/dotnet-core-3-0">.NET Core 3</a> 或完整的 .NET Framework。</p></td>
<td><a href="/dotnet/framework/wpf/getting-started/">入门</a><br/><a href="https://docs.microsoft.com/dotnet/api/index">API 参考 (.NET)</a><br/><a href="https://github.com/Microsoft/WPF-Samples">示例</a></td>
</tr>
<tr class="even">
<td><a href="https://docs.microsoft.com/dotnet/framework/winforms/">Windows 窗体</a></td>
<td><p>基于 .NET 的平台，它专用于具有轻量级 UI 模型的托管业务线应用。 这些应用可面向 <a href="https://docs.microsoft.com/dotnet/core/whats-new/dotnet-core-3-0">.NET Core 3</a> 或完整的 .NET Framework。</p></td>
<td><a href="/dotnet/framework/winforms/getting-started-with-windows-forms">入门</a><br/><a href="https://docs.microsoft.com/dotnet/api/index">API 参考 (.NET)</a><br/><a href="https://code.msdn.microsoft.com/windowsdesktop/site/search?f%5B0%5D.Type=Technology&f%5B0%5D.Value=Windows%20Forms">示例</a></td>
</tr>
</tbody>
</table>

## <a name="update-existing-desktop-apps-for-windows-10"></a>针对 Windows 10 更新现有桌面应用

如果你当前有 WPF、Windows 窗体或本机 Win32 桌面应用，Windows 10 和通用 Windows 平台 (UWP) 提供了很多功能可用来在应用中提供新式体验。 你可按照自己的进度，在应用中将其中大多数功能用作模块化组件，而不必为其他平台重新编写应用。

有很多功能可用于增强你的现有桌面应用，下面仅举几例：

* 使用 [MSIX](/windows/msix/) 打包和部署桌面应用。 MSIX 是一种新式 Windows 应用包格式，提供适合所有 Windows 应用的通用打包体验。 MSIX 汇集了 MSI、.appx、App-V 和 ClickOnce 安装技术的最佳方面，按照安全可靠的目标构建。
* 使用[包扩展](/windows/apps/desktop/modernize/desktop-to-uwp-extensions)将桌面应用与 Windows 10 体验相集成。 例如，将“启动”磁贴指向你的应用，将你的应用设为共享目标，或者通过你的应用发送 toast 通知。
* 使用 [XAML 孤岛](/windows/apps/desktop/modernize/xaml-islands)在桌面应用中托管 UWP XAML 控件。 很多最新 Windows 10 UI 功能仅适用于 UWP XAML 控件。

有关详细信息，请参阅以下文章。

<br/>

| 文章 | 说明 |
|---------|-------------|
| [实现桌面应用的现代化](/windows/apps/desktop/modernize) | 介绍可在 WPF、Windows 窗体和 C++ Win32 应用等任何桌面应用中使用的最新 Windows 10 和 UWP 开发功能。 |
| [教程：实现 WPF 应用现代化](/windows/apps/desktop/modernize/modernize-wpf-tutorial) | 按照分步说明将 UWP 墨迹和日历控件添加到应用中并将其打包到 MSIX 包中，从而将现有 WPF 业务线示例应用现代化。  |

## <a name="create-new-desktop-apps"></a>创建新的桌面应用

如果要新建适合 Windows 的桌面应用，下面有一些资源可帮助你入门。

<br/>

| 文章 | 说明 |
|---------|-------------|
| [选择应用平台](choose-your-platform.md) | 提供对主要桌面应用的深入比较，并可帮助你选择适合你的需求的平台。 本文还提供了介绍每个平台的文档的有用链接。 |
| [实现桌面应用的现代化](/windows/apps/desktop/modernize) | 介绍可在 WPF、Windows 窗体和 C++ Win32 应用等任何桌面应用中使用的最新 Windows 10 和 UWP 开发功能。 |
| [功能和技术](/windows/apps/features-and-technologies) | 概要介绍可通过每个主要桌面应用平台和相关文档的链接访问的 Windows 功能。 |

## <a name="related-documentation-and-technologies"></a>相关文档和技术

| 资源 | 说明 |
|---------|-------------|
| [.NET Core 3.0](https://docs.microsoft.com/dotnet/core/whats-new/dotnet-core-3-0) | 了解 .NET Core 3.0 的最新功能，包括对 WPF 和 Windows 窗体应用的增强功能。 |
| [关于 WPF 和 .NET Core 3.0 的桌面指南](https://docs.microsoft.com/dotnet/desktop-wpf/overview/index) | 开发面向 .NET Core 3.0 而不是整个 .NET Framework 的 WPF 应用。  |
| [Azure](https://docs.microsoft.com/azure/) | 使用 Azure 云服务扩大应用的覆盖范围。 |
| [Visual Studio](https://docs.microsoft.com/visualstudio/) | 了解如何使用 Visual Studio 开发应用和服务。 |
| [MSIX](https://docs.microsoft.com/windows/msix/) | 采用新式通用打包格式打包和部署任何 Windows 应用。 |
| [Windows AI](https://docs.microsoft.com/windows/ai/) | 使用 Windows AI 构建智能解决方案来处理应用中的复杂问题。 |
| [Windows 容器](https://docs.microsoft.com/virtualization/windowscontainers/) | 在运行快速、完全隔离的 Windows 环境中将应用程序与其依赖项打包在一起。 |
| [渐进式 Web 应用](https://docs.microsoft.com/microsoft-edge/progressive-web-apps) | 将 Web 应用转换为可分发并可在 Windows 10 上作为 UWP 应用运行的渐进式 Web 应用。 |
| [Xamarin](https://docs.microsoft.com/xamarin/) | 使用 .NET 代码和平台专属用户界面构建适合 Windows,、Android、iOS 和 macOS 的跨平台应用。 |
| [Windows 8.x 及更低版本的文档存档](https://docs.microsoft.com/previous-versions/windows/) | 访问存档的文档，了解如何构建适合 Windows 8.x 及更低版本的应用。 |
