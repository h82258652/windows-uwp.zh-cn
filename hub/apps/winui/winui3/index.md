---
title: WinUI 3.0 预览版 1（2020 年 5 月）
description: 概述 WinUI 3.0 预览版。
ms.date: 05/14/2020
ms.topic: article
ms.openlocfilehash: cbf61c618685501957e7dec081ae132995f15df5
ms.sourcegitcommit: 48e047a581fcfcc9a4084d65a78b89f2c01cf4f3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/26/2020
ms.locfileid: "85448377"
---
# <a name="windows-ui-library-30-preview-1-may-2020"></a>Windows UI 库 3.0 预览版 1（2020 年 5 月）

Windows UI 库 (WinUI) 3.0 是一个重大更新，它将 WinUI 转换成了一个适用于所有 Windows 应用类型（从 Win32 到 UWP）的完整 UX 框架。

> [!Important]
> 此 WinUI 3.0 预览版用于早期评估以及从开发人员社区收集反馈。 它**不**应该用于生产应用。
>
> **请参阅[预览版 1 限制和已知问题](#preview-1-limitations-and-known-issues)** 。
## <a name="new-features-in-winui-30-preview-1"></a>WinUI 3.0 预览版 1 中的新增功能

- 能够创建采用 WinUI 的桌面应用，包括适用于 Win32 应用的 [.NET 5](https://github.com/dotnet/core/tree/master/release-notes/5.0)
- [RadialGradientBrush](/windows/uwp/design/style/brushes#radial-gradient-brushes)
- [TabView 更新](/windows/uwp/design/controls-and-patterns/tab-view)
- 深色主题更新
- 对 [WebView2](https://docs.microsoft.com/microsoft-edge/hosting/webview2) 的改进和更新
  - 支持高 DPI
  - 支持调整窗口大小和移动窗口
  - 已更新为面向较新版本的 Edge
  - 不再需要引用 WebView2 特定的 Nuget 程序包
- SwapChainPanel
- 开放源代码迁移所需的改进

若要详细了解 WinUI 3.0 的优势和 WinUI 路线图，请参阅 GitHub 上的 [Windows UI Library Roadmap](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md)（Windows UI 库路线图）。

### <a name="provide-feedback-and-suggestions"></a>提供反馈和建议

欢迎在 [WinUI GitHub 存储库](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose)中提供反馈。

## <a name="try-winui-30-preview-1"></a>试用 WinUI 3.0 预览版 1

配置你的开发环境（有关详细说明，请参阅[配置开发环境](#configure-your-dev-environment)），通过以下链接安装 WinUI 3.0 预览版 1 VSIX，并试用 WinUI 3.0 项目模板。

<table>
<tr>
<td align="center">
<a href="https://aka.ms/winui3/previewdownload"><img src="images/downloadbuttontx.png" alt="Download the WinUI 3.0 Preview 1 VSIX"/></a>
<!--
<br/>
<a href="https://aka.ms/winui3/previewdownload">Download the WinUI 3.0 Preview 1 VSIX</a>
-->
</td>
</tr>
</table>

你还可以克隆并生成 [Xaml 控件库](#xaml-controls-gallery-winui-30-preview-1-branch)的 WinUI 3.0 预览版 1 版本。

### <a name="configure-your-dev-environment"></a>配置你的开发环境

确保你的开发计算机上已安装了 Windows 10 2018 年 4 月更新（版本 1803 - 内部版本 17134）或更高版本。

安装 Visual Studio 2019 版本 16.7 预览版 1。 可以从 [Visual Studio 预览版](https://visualstudio.microsoft.com/vs/preview)下载此版本。

安装 Visual Studio Preview 时，必须包括以下工作负载：

- .NET 桌面开发
- 通用 Windows 平台开发

若要生成 C++ 应用，还必须包括以下工作负载：

- 使用 C++ 的桌面开发
- 适用于通用 Windows 平台工作负载的 C++ (v142) 通用 Windows 平台工具可选组件

### <a name="visual-studio-project-templates"></a>Visual Studio 项目模板

若要访问 WinUI 3.0 预览版 1 和项目模板，请转到 **https://aka.ms/winui3/previewdownload**

下载 Visual Studio 扩展 (`.vsix`)，以便将 WinUI 项目模板和 NuGget 程序包添加到 Visual Studio 2019。

有关如何将 `.vsix` 添加到 Visual Studio 的说明，请参阅[查找和使用 Visual Studio 扩展](https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions?view=vs-2019#install-without-using-the-manage-extensions-dialog-box)。

安装 `.vsix` 扩展后，可以搜索“WinUI”并选择可用的 C# 或 C++ 模板之一，通过这种方式创建新的 WinUI 3.0 项目。

![WinUI 3.0 Visual Studio 模板](images/WinUI3Templates.png)<br/>
WinUI 3.0 Visual Studio 模板示例

### <a name="visual-studio-project-configuration"></a>Visual Studio 项目配置

使用某个 WinUI 3.0 预览版 1 模板创建项目时，请将“目标版本”设置为 Windows 10 版本 1903（内部版本 18362），将“最低版本”设置为 Windows 10 版本 1803（内部版本 17134）。

若要在创建项目后更改这些值，请在**解决方案资源管理器**中右键单击该项目，然后选择“属性”。

### <a name="creating-a-desktop-win32-app-with-winui-30-preview-1"></a>使用 WinUI 3.0 预览版 1 创建桌面 Win32 应用

请参阅 [Get started with WinUI 3.0 for desktop apps](get-started-winui3-for-desktop.md)（适用于桌面应用的 WinUI 3.0 入门）。

除了下面所述的限制和已知问题外，使用 WinUI 3.0 预览版 1 构建应用与使用 Xaml 和 WinUI 2.x 构建 UWP 应用非常相似。因此，UWP 应用和 `Windows.UI` API 的大多数指南和文档都适用。

## <a name="preview-1-limitations-and-known-issues"></a>预览版 1 的限制和已知问题

预览版 1 版本只是一个预览版。 围绕桌面 Win32 应用制定的方案尤其新颖。 Bug、限制和问题是少不了的。

以下各项是 WinUI 3.0 预览版 1 的一些已知问题。 如果发现下面未列出的问题，请在 [WinUI GitHub 存储库](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose)中为现有问题贡献内容，或者提交新问题，通过这种方式告知我们。

### <a name="platform-and-os-support"></a>平台和 OS 支持

WinUI 3.0 预览版 1 与运行 Windows 10 2018 年 4 月更新（版本 1803 - 内部版本 17134）及更高版本的电脑兼容。

### <a name="developer-tools"></a>开发人员工具

- 目前仅支持 C# 和 C++/WinRT 应用
- 桌面应用支持 .NET 5 和 C# 8，并且必须打包。
- UWP 应用支持 .NET Native 和 C# 7.3
- Intellisense 不完整
- 没有可视化设计器
- 没有热重载
- 没有实时可视化树
- 尚不支持使用 VS Code 进行开发
- 不支持新的 C++/CX 应用，不过，现有应用可继续运行（请尽快迁移到 C++/WinRT）
- WinUI 3.0 内容在每个进程中只能位于一个窗口中，在每个应用中只能位于一个 ApplicationView 中
- 不支持未打包的桌面部署
- 不支持 ARM64

### <a name="missing-platform-features"></a>缺少的平台功能

- 不支持 Xbox
- 不支持 HoloLens
- 不支持开窗弹出窗口
- 不支持墨迹书写
- 背景亚克力
- MediaElement 和 MediaPlayerElement
- RenderTargetBitmap
- MapControl
- 带 NavigationView 的分层导航
- SwapChainPanel 不支持透明度
- 全局展示使用回退行为，即纯色画笔
- 此版本不支持 XAML 岛
- 第三方生态系统库将无法完全正常运行
- IME 不起作用
- 无法调用基于 Windows.UI.Text 命名空间的方法

### <a name="known-issues"></a>已知问题

- 在 C# 桌面应用中：
   - 需要使用 `WinRT.WeakReference<T>`（而不是 `System.WeakReference<T>`）来对 Windows 对象（包括 Xaml 对象）进行弱引用。
   - [Point](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point)、[Rect](https://docs.microsoft.com/uwp/api/Windows.Foundation.Rect) 和 [Size](https://docs.microsoft.com/uwp/api/Windows.Foundation.Size) 结构的成员类型为 Float 而不是 Double。


## <a name="xaml-controls-gallery-winui-30-preview-1-branch"></a>Xaml 控件库（WinUI 3.0 预览版 1 分支）

请参阅 [Xaml 控件库的 WinUI 3.0 预览版 1 分支](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3preview)，了解包含所有 WinUI 3.0 预览版 1 控件和功能的示例应用。

![WinUI 3.0 预览版 1 Xaml 控件库应用](images/WinUI3XamlControlsGallery.png)<br/>
*WinUI 3.0 预览版 1 Xaml 控件库应用示例*

要下载该示例，请使用以下命令克隆 winui3preview 分支：

> `git clone --single-branch --branch winui3preview https://github.com/microsoft/Xaml-Controls-Gallery.git`

克隆后，请确保在本地 Git 环境中切换到 winui3preview 分支：

> `git checkout winui3preview`
