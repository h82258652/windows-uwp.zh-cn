---
description: 本文介绍如何在桌面 C++ Win32 应用中托管 UWP XAML UI。
title: 在 C++ Win32 应用中使用 UWP XAML 托管 API
ms.date: 03/23/2020
ms.topic: article
keywords: windows 10, uwp, windows 窗体, wpf, win32, xaml 岛
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 36c3aeb7a51c84e92c5bca461aee7efe50740237
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "80218457"
---
# <a name="using-the-uwp-xaml-hosting-api-in-a-c-win32-app"></a>在 C++ Win32 应用中使用 UWP XAML 托管 API

从 Windows 10 版本 1903 开始，非 UWP 桌面应用（包括 C++ Win32、WPF 和 Windows 窗体应用）可以使用 UWP XAML 托管 API，在与窗口句柄 (HWND) 相关联的任何 UI 元素中托管 UWP 控件  。 此 API 使非 UWP 桌面应用可以使用仅可通过 UWP 控件获取的最新 Windows 10 UI 功能。 例如，非 UWP 桌面应用可以使用此 API 来托管使用 [Fluent Design System](/windows/uwp/design/fluent-design-system/index) 并支持 [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions) 的 UWP 控件。

UWP XAML 托管 API 为我们提供更广泛的控件以使开发人员能够将 Fluent UI 引入非 UWP 桌面应用奠定了基础。 此功能称为“XAML 岛”  。 有关此功能的概述，请参阅[在桌面应用中托管 UWP XAML 控件（XAML 岛）](xaml-islands.md)。

> [!NOTE]
> 如果有关于 XAML 岛的反馈，请在 [Microsoft.Toolkit.Win32 存储库](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues)中创建一个新问题，将你的意见留在那里。 若要私下提交反馈，可将其发送到 XamlIslandsFeedback@microsoft.com。 你的意见和方案对我们至关重要。

## <a name="is-the-uwp-xaml-hosting-api-the-right-choice-for-your-desktop-app"></a>UWP XAML 托管 API 是否适桌合面应用？

UWP XAML 托管 API 提供了用于在桌面应用中托管 UWP 控件的低级别基础结构。 某些类型的桌面应用可以选择使用其他更方便的 API 来实现此目标。

* 如果你拥有 C++ Win32 桌面应用且想要在应用中托管 UWP 控件，则必须使用 UWP XAML 托管 API。 这些类型的应用没有其他选择。

* 对于 WPF 和 Windows 窗体应用，强烈建议你使用 Windows 社区工具包中的 [XAML 岛 .NET 控件](xaml-islands.md#wpf-and-windows-forms-applications)，而不是直接使用 UWP XAML 托管 API。 这些控件在内部使用 UWP XAML 托管 API，并实现你在直接使用 UWP XAML 托管 API 时需要自行处理的所有行为，包括键盘导航和布局更改。

因为我们建议仅 C++ Win32 应用使用 UWP XAML 托管 API，所以本文主要提供 C++ Win32 应用的说明和示例。 但是，你可以选择在 WPF 和 Windows 窗体应用中使用 UWP XAML 托管 API。 本文指向适用于 Windows 社区工具包中 WPF 和 Windows 窗体的[主机控件](xaml-islands.md#host-controls)的相关源代码，以便你可以了解这些控件如何使用 UWP XAML 托管 API。

## <a name="learn-how-to-use-the-xaml-hosting-api"></a>了解如何使用 XAML 托管 API

若要按照提供了代码示例的分步说明在 C++ Win32 应用中使用 XAML 托管 API，请参阅以下文章：

* [托管标准 UWP 控件](host-standard-control-with-xaml-islands-cpp.md)
* [托管自定义 UWP 控件](host-custom-control-with-xaml-islands-cpp.md)
* [高级方案](advanced-scenarios-xaml-islands-cpp.md)

## <a name="samples"></a>示例

在代码中使用 UWP XAML 托管 API 的方式取决于你的应用类型、应用的设计以及其他因素。 为了帮助说明如何在完整应用的上下文中使用此 API，本文引用了以下示例中的代码。

### <a name="c-win32"></a>C++ Win32

以下示例演示如何在 C++ Win32 应用中使用 UWP XAML 托管 API：

* [简单 XAML 岛示例](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Standalone_Samples/CppWinRT_Basic_Win32App)。 此示例演示了在未打包的 C++ Win32 应用（即，未内置于 MSIX 包中的应用）中托管 UWP 控件的基本实现。

* [带有自定义控件示例的 XAML 岛](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Samples/Win32)。 此示例演示了在打包的 C++ Win32 应用中托管自定义 UWP 控件以及处理其他行为（如键盘输入和焦点导航）的完整实现。

### <a name="wpf-and-windows-forms"></a>WPF 和 Windows 窗体

Windows 社区工具包中的 [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 控件用作在 WPF 和 Windows 窗体应用中使用 UWP 托管 API 的参考示例。 源代码在以下位置提供：

* 对于 WPF 版本的控件，请[转到此处](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Wpf.UI.XamlHost)。 WPF 版本派生自 [System.Windows.Interop.HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)。

* 对于 Windows 窗体版本的控件，请[转到此处](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Forms.UI.XamlHost)。 Windows 窗体版本派生自 [System.Windows.Forms.Control](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)。

> [!NOTE]
> 强烈建议你在 Windows 社区工具包中使用 [XAML 岛 .NET 控件](xaml-islands.md#wpf-and-windows-forms-applications)，而不是直接在 WPF 和 Windows 窗体应用中使用 UWP XAML 托管 API。 本文中的 WPF 和 Windows 窗体示例链接仅用于说明目的。

## <a name="architecture-of-the-api"></a>API 的体系结构

UWP XAML 托管 API 包含这些主要 Windows 运行时类型和 COM 接口。

|  类型或接口 | 说明 |
|--------------------|-------------|
| [WindowsXamlManager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) | 此类表示 UWP XAML 框架。 此类提供单个静态 [InitializeForCurrentThread](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) 方法，该方法可在桌面应用中的当前线程上初始化 UWP XAML 框架。 |
| [DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) | 此类表示你在桌面应用中托管的 UWP XAML 内容的实例。 此类最重要的成员是 [Content](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content) 属性。 需将此属性分配给要托管的 [Windows.UI.Xaml.UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)。 此类还有其他成员，这些成员用于将键盘焦点导航入和导航出 XAML 岛。 |
| IDesktopWindowXamlSourceNative | 此 COM 接口提供 AttachToWindow 方法，可使用该方法将应用中的 XAML 岛附加到父 UI 元素  。 每个 DesktopWindowXamlSource 对象都实现此接口  。 此接口在 windows.ui.xaml.hosting.desktopwindowxamlsource.h 中声明。 |
| IDesktopWindowXamlSourceNative2 | 此 COM 接口提供 PreTranslateMessage 方法，该方法使 UWP XAML 框架能够正确处理某些 Windows 消息  。 每个 DesktopWindowXamlSource 对象都实现此接口  。 此接口在 windows.ui.xaml.hosting.desktopwindowxamlsource.h 中声明。 |

下图说明了在桌面应用中托管的 XAML 岛中对象的层次结构。

* 基本级别是你希望在其中托管 XAML 岛的应用中的 UI 元素。 此 UI 元素必须具有一个窗口句柄 (HWND)。 可在其中托管 XAML 岛的 UI 元素示例包括适用于 C++ Win32 应用的[窗口](https://docs.microsoft.com/windows/desktop/winmsg/about-windows)、适用于 WPF 应用的 [System.Windows.Interop.HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)，以及适用于 Windows 窗体应用的 [System.Windows.Forms.Control](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)。

* 下一级别是 DesktopWindowXamlSource 对象  。 此对象提供用于托管 XAML 岛的基础结构。 你的代码负责创建此对象并将其附加到父 UI 元素。

* 创建 DesktopWindowXamlSource 时，此对象将自动创建一个本机子窗口以托管 UWP 控件  。 此本机子窗口主要提取自代码，但如果需要，你可以访问其句柄 (HWND)。

* 最后，最高级别是要在桌面应用中托管的 UWP 控件。 这可以是派生自 [Windows.UI.Xaml.UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) 的任何 UWP 对象，包括 Windows SDK 提供的任何 UWP 控件以及自定义用户控件。

![DesktopWindowXamlSource 体系结构](images/xaml-islands/xaml-hosting-api-rev2.png)

> [!NOTE]
> 在桌面应用中托管 XAML 岛时，可以同时在同一线程上运行多个 XAML 内容树。 若要访问 XAML 岛中 XAML 内容树的根元素并获取在其中托管它的上下文的相关信息，请使用 [XamlRoot](https://docs.microsoft.com/uwp/api/windows.ui.xaml.xamlroot) 类。 [CoreWindow](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow)、[ApplicationView](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview) 和[窗口](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window) API 不会为 XAML 岛提供正确的信息。 有关详情，请参阅[本部分](xaml-islands.md#window-host-context-for-xaml-islands)。

## <a name="troubleshooting"></a>疑难解答

### <a name="error-using-uwp-xaml-hosting-api-in-a-uwp-app"></a>在 UWP 应用中使用 UWP XAML 托管 API 时出错

| 问题 | 解决方法 |
|-------|------------|
| 你的应用收到 COMException，其中包含以下消息  ：“无法激活 DesktopWindowXamlSource。 此类型不能在 UWP 应用中使用。” 或者“无法激活 WindowsXamlManager。 此类型不能在 UWP 应用中使用。” | 此错误表示你正尝试在 UWP 应用中使用 UWP XAML 托管 API（具体而言，你正尝试实例化 [DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) 或 [WindowsXamlManager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) 类型）。 UWP XAML 托管 API 仅适用于非 UWP 桌面应用，如 WPF、Windows 窗体和 C++ Win32 应用程序。 |

### <a name="error-trying-to-use-the-windowsxamlmanager-or-desktopwindowxamlsource-types"></a>尝试使用 WindowsXamlManager 或 DesktopWindowXamlSource 类型时出错

| 问题 | 解决方法 |
|-------|------------|
| 你的应用收到异常，其中包含以下消息：“面向 Windows 版本 10.0.18226.0 及更高版本的应用支持 WindowsXamlManager 和 DesktopWindowXamlSource。 请检查应用程序清单或程序包清单，确保 MaxTestedVersion 属性已更新。” | 此错误表示你的应用程序尝试在 UWP XAML 托管 API 中使用 WindowsXamlManager 或 DesktopWindowXamlSource 类型，但操作系统无法确定该应用是否是面向 Windows 10 版本 1903 或更高版本而生成的   。 在早期版本的 Windows 10 中，UWP XAML 托管 API 最初作为预览引入，仅从 Windows 10 版本 1903 开始才受支持。</p></p>若要解决此问题，请为应用创建 MSIX 包，并从该包运行应用，或在项目中安装 [Microsoft.Toolkit.Win32.UI.SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) NuGet 包。  |

### <a name="error-attaching-to-a-window-on-a-different-thread"></a>附加到不同线程上的窗口时出错

| 问题 | 解决方法 |
|-------|------------|
| 你的应用收到 COMException，其中包含以下消息  ：“AttachToWindow 方法失败，因为指定的 HWND 是在另一个线程上创建的。” | 此错误表示你的应用程序调用了 IDesktopWindowXamlSourceNative::AttachToWindow 方法，并向其传递了在不同线程上创建的窗口的 HWND  。 必须向此方法传递在与调用该方法的代码相同的线程上创建的窗口的 HWND。 |

### <a name="error-attaching-to-a-window-on-a-different-top-level-window"></a>附加到不同最高级别窗口上的窗口时出错

| 问题 | 解决方法 |
|-------|------------|
| 你的应用收到 COMException，其中包含以下消息  ：“AttachToWindow 方法失败，因为指定的 HWND 来自于与先前在同一线程上传递给 AttachToWindow 的 HWND 不同的最高级别窗口。” | 此错误表示你的应用程序调用了 IDesktopWindowXamlSourceNative::AttachToWindow 方法，并向其传递了一个窗口的 HWND，该 HWND 来自于与你在同一线程上对此方法的先前调用中所指定的窗口不同的最高级别窗口  。</p></p>在应用程序调用特定线程上的 AttachToWindow 后，同一线程上的所有其他 [DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) 对象只能附加到首次调用时传递到 AttachToWindow 的相同最高级别窗口的后代窗口   。 当特定线程的所有 DesktopWindowXamlSource 对象均关闭时，下一个 DesktopWindowXamlSource 就可以自由附加到任何窗口   。</p></p>若要解决此问题，请关闭与此线程上的其他最高级别窗口绑定的所有 DesktopWindowXamlSource 对象，或为此 DesktopWindowXamlSource 创建一个新线程   。 |

## <a name="related-topics"></a>相关主题

* [在桌面应用中托管 UWP XAML 控件（XAML 岛）](xaml-islands.md)
* [在 C++ Win32 应用中托管标准 UWP 控件](host-standard-control-with-xaml-islands-cpp.md)
* [在 C++ Win32 应用中托管自定义 UWP 控件](host-custom-control-with-xaml-islands-cpp.md)
* [C++ Win32 应用中 XAML 岛的高级应用场景](advanced-scenarios-xaml-islands-cpp.md)
* [XAML 岛代码示例](https://github.com/microsoft/Xaml-Islands-Samples)
* [C++ Win32 XAML 岛示例](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Samples/Win32/SampleCppApp)
