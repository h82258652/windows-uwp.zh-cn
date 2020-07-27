---
title: WinUI 3 预览版 2（2020 年 7 月）
description: WinUI 3 预览版 2 发布概述。
ms.date: 07/15/2020
ms.topic: article
ms.openlocfilehash: 0acea4520f10d5f64baa29cb64fdf0ba1cc4552e
ms.sourcegitcommit: e1104689fc1db5afb85701205c2580663522ee6d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/23/2020
ms.locfileid: "86997954"
---
# <a name="windows-ui-library-3-preview-2-july-2020"></a>Windows UI 库 3 预览版 2（2020 年 7 月）

Windows UI 库 (WinUI) 3 是适用于 Windows 桌面和 UWP 应用的本机用户体验 (UX) 框架。

WinUI 3 预览版 2 是兼具质量和稳定性的发行版本，重点修复了预览版 1 发行版本中的 Bug 和已知问题。

**请参阅[预览版 2 限制和已知问题](#preview-2-limitations-and-known-issues)** 。

> [!Important]
> 此 WinUI 3 预览版用于早期评估以及从开发人员社区收集反馈。 它**不**应该用于生产应用。
>
> 我们将在 2020 年至 2021 年初继续发布 WinUI 3 的预览版本，之后将发布第一个正式版本。
>
> 请使用 [WinUI GitHub 存储库](https://github.com/microsoft/microsoft-ui-xaml)提供反馈、报告问题并提出建议。

## <a name="install-winui-3-preview-2"></a>安装 WinUI 3 预览版 2

WinUI 3 预览版 2 提供有 Visual Studio 项目模板和 NuGet 包，前者有助于你开始使用基于 WinUI 的用户界面构建应用，后者包含 WinUI 库。 若要安装 WinUI 3 预览版 2，请执行以下步骤。

> [!NOTE]
> 你还可以克隆并生成 [XAML 控件库](#xaml-controls-gallery-winui-3-preview-2-branch)的 WinUI 3 预览版 2 版本。

1. 确保你的开发计算机上已安装 Windows 10 版本 1803（内部版本 17134）或更高版本。

2. 安装 [Visual Studio 2019 版本 16.7 预览版 3](https://visualstudio.microsoft.com/vs/preview)

    安装 Visual Studio 时，必须包括以下工作负载：
    - .NET 桌面开发
    - 通用 Windows 平台开发

    若要生成 C++ 应用，还必须包括以下工作负载：
    - 使用 C++ 的桌面开发
    - 适用于通用 Windows 平台工作负载的 C++ (v142) 通用 Windows 平台工具可选组件（请参阅右窗格中“通用 Windows 平台开发”部分下的“安装详细信息”）

3. 若要为 C#/.NET 5 和 C++/Win32 应用创建桌面 WinUI 项目，还必须安装 .NET 5 预览版 5 的 x64 和 x86 版本：

    - x64：[https://aka.ms/dotnet/net5/preview5/Sdk/dotnet-sdk-win-x64.exe](https://aka.ms/dotnet/net5/preview5/Sdk/dotnet-sdk-win-x64.exe)
    - x86：[https://aka.ms/dotnet/net5/preview5/Sdk/dotnet-sdk-win-x86.exe](https://aka.ms/dotnet/net5/preview5/Sdk/dotnet-sdk-win-x86.exe)

4. 下载并安装 [WinUI 3 预览版 2 VSIX 包](https://aka.ms/winui3/previewdownload)。 此 VSIX 包将 WinUI 3 项目模板和包含 WinUI 3 库的 NuGet 包添加到 Visual Studio 2019。

    有关如何将 VSIX 包添加到 Visual Studio 的说明，请参阅[查找和使用 Visual Studio 扩展](https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions?view=vs-2019#install-without-using-the-manage-extensions-dialog-box)。

## <a name="create-winui-projects"></a>创建 WinUI 项目

安装 WinUI 3 预览版 2 VSIX 包后，即可使用 Visual Studio 中的一个 WinUI 项目模板创建一个新项目。 若要在“创建新项目”对话框中访问 WinUI 项目模板，请将语言筛选为“C++”或“C#”，将平台筛选为“Windows”，将项目类型筛选为“WinUI”    。 或者可以搜索“WinUI”并选择一个可用的 C# 或 C++ 模板。

![WinUI 项目模板](images/winui-projects-csharp.png)

有关 WinUI 项目模板入门的详细信息，请参阅以下文章：

- [适用于桌面应用的 WinUI 3 入门](get-started-winui3-for-desktop.md)
- [适用于 UWP 应用的 WinUI 3 入门](get-started-winui3-for-uwp.md)

除[限制和已知问题](#preview-2-limitations-and-known-issues)外，使用 WinUI 项目生成应用类似于使用 XAML 和 WinUI 2.x 生成 UWP 应用。 因此，有关 UWP 应用和 Windows SDK 中的 Windows.UI WinRT 命名空间的大多数指南和文档均适用。

如果使用 WinUI 3 预览版 1 创建了一个项目，则可以升级该项目以使用预览版 2。 请参阅 [GitHub 存储库](https://aka.ms/winui3/upgrade-instructions)中的详细说明。

### <a name="project-templates-for-winui-3"></a>适用于 WinUI 3 的项目模板

可以使用这些 WinUI 项目模板来创建应用。

| 模板 | Language | 说明 |
|----------|----------|-------------|
| 打包的空白应用（桌面版 WinUI） | C# 和 C++ | 使用基于 WinUI 的用户界面创建桌面 NET 5 (C#) 或本机 Win32 (C++) 应用。 生成的项目包括一个基本窗口，该窗口派生自 WinUI 库中的 Microsoft.UI.Xaml.Window 类，可用于生成 UI。 有关此项目类型的详细信息，请参阅[适用于桌面应用的 WinUI 3 入门](get-started-winui3-for-desktop.md)。<p></p>此解决方案还包括一个 [Windows 应用程序打包项目](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)，该项目经配置后可将应用生成为 [MSIX 包](/windows/msix/overview)。 这提供了一种新式部署体验，能够通过包扩展与 Windows 10 功能集成以及更多其他功能。  |
| 空白应用（UWP 版 WinUI）  | C# 和 C++ | 创建一个具有基于 WinUI 的用户界面的 UWP 应用。 生成的项目包括一个基础页面，该页面派生自 WinUI 库中的 Microsoft.UI.Xaml.Controls.Page 类，可用于生成 UI。 有关此项目类型的详细信息，请参阅[适用于 UWP 应用的 WinUI 3 入门](get-started-winui3-for-uwp.md)。 |

可以使用这些 WinUI 项目模板来构建可由基于 WinUI 的应用加载和使用的组件。

| 模板 | Language | 说明 |
|----------|----------|-------------|
| 类库（桌面版 WinUI） | 仅限 C# | 使用 C# 创建一个 .NET 5 托管类库 (DLL)，使其可由具有基于 WinUI 的用户界面的其他 .NET 5 桌面应用使用。  |
| 类库（UWP 版 WinUI）  | 仅限 C# | 使用 C# 创建一个托管类库 (DLL)，使其可由具有基于 WinUI 的用户界面的其他 UWP 应用使用。 |
| Windows 运行时组件（UWP 版 WinUI） | C# 和 C++ | 使用 C# 或 C++/WinRT 创建一个 [Windows 运行时组件](/windows/uwp/winrt-components/)，使其可由任何具有基于 WinUI 的用户界面的 UWP 应用使用，而不管这些应用使用哪种编程语言编写。 |

### <a name="item-templates-for-winui-3"></a>适用于 WinUI 3 的项模板

以下项模板可用于 WinUI 项目。 若要访问这些 WinUI 项目模板，请在“解决方案资源管理器”中右键单击项目节点，选择“添加” -> “新建项”，然后在“添加新项”对话框中单击“WinUI”    。

![WinUI 项模板](images/winui-items-csharp.png)

| 模板 | Language | 说明 |
|----------|----------|-------------|
| 空白页 (WinUI) | C# 和 C++ | 添加 XAML 文件和定义了新页面的代码文件，该页面派生自 WinUI 库中的 Microsoft.UI.Xaml.Controls.Page 类。 |
| 空白窗口（桌面版 WinUI） | C# 和 C++ | 添加 XAML 文件和定义了新窗口的代码文件，该窗口派生自 WinUI 库中的 Microsoft.UI.Xaml.Window 类。 |
| 自定义控件 (WinUI) | C# 和 C++ | 添加用于创建具有默认样式的模板化控件的代码文件。 该模板化控件派生自 WinUI 库中的 Microsoft.UI.Xaml.Controls.Control 类。<p></p>有关如何使用该项模板的演练，请参阅[使用 C++/WinRT 模板化 UWP 和 WinUI 3 应用的 XAML 控件](xaml-templated-controls-cppwinrt-winui3.md)。 有关模板化控件的详细信息，请参阅[自定义 XAML 控件](https://docs.microsoft.com/archive/msdn-magazine/2019/may/xaml-custom-xaml-controls)。 |
| 资源字典 (WinUI) | C# 和 C++ | 添加 XAML 资源的空键控集合。 有关详细信息，请参阅 [ResourceDictionary 和 XAML 资源参考](/windows/uwp/design/controls-and-patterns/resourcedictionary-and-xaml-resource-references)。 |
| 资源文件 (WinUI) | C# 和 C++ | 添加用于存储应用的字符串和条件资源的文件。 可以借助此项对应用程序进行本地化。 有关详细信息，请参阅[对 UI 和应用包清单中的字符串进行本地化](/windows/uwp/app-resources/localize-strings-ui-manifest)。 |
| 用户控件 (WinUI) | C# 和 C++ | 添加 XAML 文件和用于创建用户控件的代码文件，该用户控件派生自 WinUI 库中的 Microsoft.UI.Xaml.Controls.UserControl 类。 通常，用户控件封装相关的现有控件并提供其自己的逻辑。<p></p>有关用户控件的详细信息，请参阅[自定义 XAML 控件](https://docs.microsoft.com/archive/msdn-magazine/2019/may/xaml-custom-xaml-controls)。 |

## <a name="bug-fixes-and-other-improvements-in-winui-3-preview-2"></a>WinUI 3 预览版 2 中的 Bug 修复和其他改进

这是预览版 2 的 Bug 修复和其他更新的完整列表。 请参阅我们的[发布公告](https://aka.ms/winui3/preview2-announcement)，以获得该发行版中解决的最关键 Bug 的列表。

> [!NOTE]
> WinUI 3 预览版 2 使用 WinUI 2 库的版本 2.4.2。 

- [INotifyCollectionChanged](https://docs.microsoft.com/dotnet/api/system.collections.specialized.inotifycollectionchanged?view=net-5.0) 和 [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=net-5.0) 目前在 C# 桌面应用中按预期方式工作
  - 这解决了其他一些问题，即有关集合控件在后端中更新时不会在 UI 中更新的问题。
  - 感谢 @hshristov 在 GitHub 上提交了[类似的问题](https://github.com/microsoft/microsoft-ui-xaml/issues/2490)！
- 预览版 2 现与桌面应用的 [.NET 5 预览版 5](https://docs.microsoft.com/dotnet/api/?view=net-5.0) 兼容
- WinUI 3 现在与 [WinUI 2.4](../winui2/release-notes/winui-2.4.md) 具有同等性能，其中包括新的控件和功能，例如[分层 NavigationView](../winui2/release-notes/winui-2.4.md#hierarchical-navigation) 和 [ProgressRing](../winui2/release-notes/winui-2.4.md#progressring)。
- 已修复故障：触控使用 [TabView](/windows/uwp/design/controls-and-patterns/tab-view)
- [XAML 控件库示例](#xaml-controls-gallery-winui-3-preview-2-branch)中的 [NavigationView](/windows/uwp/design/controls-and-patterns/navigationview) 现使用左侧模式而不是左紧凑模式
- 已修复故障：输入验证和 [RichEditBox](/windows/uwp/design/controls-and-patterns/rich-edit-box) 中的键入速度太快
  - 感谢 @paulovilla 在 GitHub 上提交了[此问题](https://github.com/microsoft/microsoft-ui-xaml/issues/2563)！
- 已修复故障：[TextBox](/windows/uwp/design/controls-and-patterns/text-box) 菜单打开时与 XAML UI 进行交互
- 导航至多个页面后，[XAML 控件库示例](#xaml-controls-gallery-winui-3-preview-2-branch)标题文本不再打乱
- 触控使用 [WebView2](https://docs.microsoft.com/microsoft-edge/webview2/) 不再出现轻微偏移
- WinUIEdit.dll 中的类已从 Windows.UI.Text 命名空间移至 Microsoft.UI.Text 命名空间
- 已修复故障：可在多选模式下选择 [TreeView](/windows/uwp/design/controls-and-patterns/tree-view) 中的项目（Windows 10 版本 1803）
- 在桌面应用程序的 API 的 C# 投影中，Point、Rect 和 Size 成员现为双类型。
  - 感谢 @dotMorten 在 GitHub 上提交了[此问题](https://github.com/microsoft/microsoft-ui-xaml/issues/2474)！
- 已修复故障：将 [RichEditBox](/windows/uwp/design/controls-and-patterns/rich-edit-box) 与 .rtf 文件配合使用
- [TabView](/windows/uwp/design/controls-and-patterns/tab-view)“关闭”按钮不再具有空白工具提示
- [图像](/windows/uwp/design/controls-and-patterns/images-imagebrushes)控件现在可以正确呈现 SVG 文件
  - 感谢 @mqudsi 在 GitHub 上提交了[此问题](https://github.com/microsoft/microsoft-ui-xaml/issues/2565)！
- 已修复故障：使用/导航到页面元素
- 现在，可以通过触摸选择 [NavigationView](/windows/uwp/design/controls-and-patterns/navigationview) 中的项目（在单选模式下）取消选择所有其他项目
- 已修复故障：LayoutSliderException（由于在特定大小的[滑块](/windows/uwp/design/controls-and-patterns/slider)控件上设置的值）不再发生 
  - 感谢 @hig-dev 在 GitHub 上提交了[此问题](https://github.com/microsoft/microsoft-ui-xaml/issues/477)！
- 已修复故障：使用 [ColorPicker](/windows/uwp/design/controls-and-patterns/color-picker) 导致关机故障
- 已修复故障：使用 [Pivot](/windows/uwp/design/controls-and-patterns/pivot) 导致关机故障
- 已修复故障：Windows 10 版本 1803 中的资源丢失导致 [NavigationView](/windows/uwp/design/controls-and-patterns/navigationview) 故障
- 已修复故障：侧重于 [RichEditBox](/windows/uwp/design/controls-and-patterns/rich-edit-box) 自定义编辑器 
- 已修复故障：[SemanticZoom](/windows/uwp/design/controls-and-patterns/semantic-zoom) 
- 现在，绑定可以按预期在标记中正常工作，并且其中的 Mode=OneWay 是隐式的
  - 感谢 @tomasfabian 在 GitHub 上提交了[此问题](https://github.com/microsoft/microsoft-ui-xaml/issues/2630)！
- 已修复动画：[XAML 控件库示例](#xaml-controls-gallery-winui-3-preview-2-branch)中的新增功能

## <a name="new-features-and-capabilities-introduced-in-winui-3-preview-1"></a>WinUI 3 预览版 1 中引入的新特性与功能

以下特性和功能是在 WinUI 3 预览版 1 中引入的，并且在 WinUI 3 预览版 2 中继续受支持。

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

若要详细了解 WinUI 3 的优势和 WinUI 路线图，请参阅 GitHub 上的 [Windows UI 库路线图](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md)。

### <a name="provide-feedback-and-suggestions"></a>提供反馈和建议

欢迎在 [WinUI GitHub 存储库](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose)中提供反馈。

## <a name="preview-2-limitations-and-known-issues"></a>预览版 2 的限制和已知问题

预览版 2 版本只是一个预览版。 围绕桌面 Win32 应用制定的方案尤其新颖。 Bug、限制和其他问题是少不了的。

以下各项是 WinUI 3 预览版 2 的一些已知问题。 如果发现下面未列出的问题，请在 [WinUI GitHub 存储库](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose)中为现有问题贡献内容，或者提交新问题，通过这种方式告知我们。

### <a name="platform-and-os-support"></a>平台和 OS 支持

WinUI 3 预览版 2 与运行 Windows 10 2018 年 4 月更新（版本 1803 - 内部版本 17134）及更高版本的电脑兼容。

### <a name="developer-tools"></a>开发人员工具

- 目前仅支持 C# 和 C++/WinRT 应用
- 桌面应用支持 .NET 5 和 C# 8，并且必须打包
- UWP 应用支持 .NET Native 和 C# 7.3
- Intellisense 不完整
- 没有可视化设计器
- 没有热重载
- 没有实时可视化树
- 尚不支持使用 VS Code 进行开发
- 不支持新的 C++/CX 应用，不过，现有应用可继续运行（请尽快迁移到 C++/WinRT）
- WinUI 3 内容在每个进程中只能位于一个窗口中，在每个应用中只能位于一个 ApplicationView 中
- 不支持未打包的桌面部署
- 不支持 ARM64
- UWP 应用中的 C# 自定义控件：不自动生成 `Themes/Generic.xaml`。 若要解决此问题，可以在类中手动创建“Themes”文件夹，并将 XAML 文件放在名为 `Generic.xaml` 的文件中。
- 将 WinUI 自定义控件添加到项目后，文件可能缺少“CustomControl”标头。 可以通过手动将该标头添加到 `pch.h` 文件中来解决此问题。
- 添加 DataGrid、其他 Windows 社区工具包控件和第三方库控件可能会导致生成失败。 若要解决此问题，请将此合并字典添加到 `App.xaml` 文件：
  ```xaml
  <ResourceDictionary Source="ms-appx:///<library_name>/Themes/Generic.xaml"/>
  ```

### <a name="missing-platform-features"></a>缺少的平台功能

- Xbox 支持
- HoloLens 支持
- 窗口式弹出窗口
- 墨迹书写支持
- 背景亚克力
- MediaElement 和 MediaPlayerElement
- RenderTargetBitmap
- MapControl
- SwapChainPanel 不支持透明度
- 全局展示使用回退行为，即纯色画笔
- 此版本不支持 XAML 岛
- 第三方生态系统库将无法完全正常运行
- IME 不起作用

### <a name="known-issues"></a>已知问题

- 在 C# 桌面应用中：
  - 需要使用 `WinRT.WeakReference<T>`（而不是 `System.WeakReference<T>`）来对 Windows 对象（包括 XAML 对象）进行弱引用。

## <a name="xaml-controls-gallery-winui-3-preview-2-branch"></a>XAML 控件库（WinUI 3 预览版 2 分支）

请参阅 [XAML 控件库的 WinUI 3 预览版 2 分支](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3preview)，了解包含所有 WinUI 3 预览版 2 控件和功能的示例应用。

![WinUI 3 预览版 2 XAML 控件库应用](images/WinUI3XamlControlsGalleryP2.png)<br/>
WinUI 3 预览版 2 XAML 控件库应用示例

要下载该示例，请使用以下命令克隆 winui3preview 分支：

```
git clone --single-branch --branch winui3preview https://github.com/microsoft/Xaml-Controls-Gallery.git
```

克隆后，请确保在本地 Git 环境中切换到 winui3preview 分支：

```
git checkout winui3preview
```
