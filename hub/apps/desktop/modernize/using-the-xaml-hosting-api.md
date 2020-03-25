---
description: 本文介绍如何在桌面C++ Win32 应用程序中托管 UWP XAML UI。
title: 在 C++ Win32 应用中使用 UWP XAML 托管 API
ms.date: 03/23/2020
ms.topic: article
keywords: windows 10、uwp、windows 窗体、wpf、win32、xaml 孤岛
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 36c3aeb7a51c84e92c5bca461aee7efe50740237
ms.sourcegitcommit: c660def841abc742600fbcf6ed98e1f4f7beb8cc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/24/2020
ms.locfileid: "80218457"
---
# <a name="using-the-uwp-xaml-hosting-api-in-a-c-win32-app"></a>在 C++ Win32 应用中使用 UWP XAML 托管 API

从 Windows 10 版本1903开始，非 UWP 桌面应用（包括C++ WIN32、WPF 和 Windows 窗体应用）可以使用*UWP XAML 宿主 API*在与窗口句柄（HWND）关联的任何 UI 元素中托管 uwp 控件。 此 API 使非 UWP 桌面应用可以使用仅可通过 UWP 控件获取的最新 Windows 10 UI 功能。 例如，非 UWP 桌面应用程序可使用此 API 来托管使用[熟知设计系统](/windows/uwp/design/fluent-design-system/index)并支持[WINDOWS 墨迹](/windows/uwp/design/input/pen-and-stylus-interactions)的 UWP 控件。

UWP XAML 宿主 API 为一组更广泛的控件提供基础，使开发人员能够将熟知的 UI 引入非 UWP 桌面应用程序。 此功能称为*XAML 孤岛*。 有关此功能的概述，请参阅[在桌面应用中宿主 UWP XAML 控件（XAML 孤岛）](xaml-islands.md)。

> [!NOTE]
> 如果有关于 XAML 岛的反馈，请在 [Microsoft.Toolkit.Win32 存储库](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues)中创建一个新问题，将你的意见留在那里。 若要私下提交反馈，可将其发送到 XamlIslandsFeedback@microsoft.com。 你的意见和方案对我们至关重要。

## <a name="is-the-uwp-xaml-hosting-api-the-right-choice-for-your-desktop-app"></a>UWP XAML 托管 API 是否适合桌面应用？

UWP XAML 宿主 API 提供低级别的基础结构，用于在桌面应用中承载 UWP 控件。 某些类型的桌面应用可以选择使用替代的更方便的 Api 来实现此目标。

* 如果你有C++ Win32 桌面应用，并且想要在应用中托管 UWP 控件，则必须使用 UWP XAML 宿主 API。 此类应用没有替代项。

* 对于 WPF 和 Windows 窗体应用程序，我们强烈建议你在 Windows 社区工具包中使用[XAML 岛 .net 控件](xaml-islands.md#wpf-and-windows-forms-applications)，而不是直接使用 UWP XAML 托管 API。 这些控件在内部使用 UWP XAML 宿主 API，并实现你在自行使用 UWP XAML 托管 API 时需要自行处理的所有行为，包括键盘导航和布局更改。

因为我们建议只有C++ win32 应用使用 UWP XAML 托管 API，但本文主要介绍 win32 应用的C++说明和示例。 但是，如果您选择，可以在 WPF 中使用 UWP XAML 宿主 API，并 Windows 窗体应用。 本文指向适用于 WPF 的[主机控件](xaml-islands.md#host-controls)和 Windows 社区工具包中 Windows 窗体的相关源代码，以便您可以看到这些控件如何使用 UWP XAML 宿主 API。

## <a name="learn-how-to-use-the-xaml-hosting-api"></a>了解如何使用 XAML 宿主 API

若要按照有关在 Win32 应用中C++使用 XAML 托管 API 的代码示例，请参阅以下文章：

* [承载标准 UWP 控件](host-standard-control-with-xaml-islands-cpp.md)
* [承载自定义 UWP 控件](host-custom-control-with-xaml-islands-cpp.md)
* [高级方案](advanced-scenarios-xaml-islands-cpp.md)

## <a name="samples"></a>示例

您在代码中使用 UWP XAML 宿主 API 的方式取决于您的应用程序类型、应用程序的设计和其他因素。 为了帮助说明如何在完整应用程序的上下文中使用此 API，本文引用了以下示例中的代码。

### <a name="c-win32"></a>C++Win32

下面的示例演示如何在C++ Win32 应用程序中使用 UWP XAML 宿主 API：

* [简单的 XAML 岛示例](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Standalone_Samples/CppWinRT_Basic_Win32App)。 此示例演示了在未打包C++的 Win32 应用程序（即，未内置到 .msix 包中的应用程序）中承载 UWP 控件的基本实现。

* [带有自定义控件示例的 XAML 岛](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Samples/Win32)。 此示例演示在打包C++的 Win32 应用程序中托管自定义 UWP 控件以及处理其他行为（如键盘输入和焦点导航）的完整实现。

### <a name="wpf-and-windows-forms"></a>WPF 和 Windows 窗体

Windows 社区工具包中的[WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)控件作为参考示例，用于在 WPF 和 Windows 窗体应用中使用 UWP 宿主 API。 源代码位于以下位置：

* 对于控件的 WPF 版本，请[参阅此处](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Wpf.UI.XamlHost)。 WPF 版本派生自[system.windows.interop.hwndhost>](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)。

* 对于控件的 Windows 窗体版本，请[参阅此处](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Forms.UI.XamlHost)。 Windows 窗体版本派生自[system.object](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)。

> [!NOTE]
> 强烈建议你在 Windows 社区工具包中使用[XAML 岛 .net 控件](xaml-islands.md#wpf-and-windows-forms-applications)，而不是直接在 WPF 和 Windows 窗体应用中使用 UWP XAML 托管 API。 本文中的 WPF 和 Windows 窗体示例链接仅用于说明目的。

## <a name="architecture-of-the-api"></a>API 的体系结构

UWP XAML 宿主 API 包含这些主要 Windows 运行时类型和 COM 接口。

|  类型或接口 | 说明 |
|--------------------|-------------|
| [WindowsXamlManager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) | 此类表示 UWP XAML framework。 此类提供单个静态[InitializeForCurrentThread](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread)方法，该方法可初始化桌面应用程序中当前线程上的 UWP XAML 框架。 |
| [DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) | 此类表示你在桌面应用中托管的 UWP XAML 内容的实例。 此类中最重要的成员是[Content](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content)属性。 将此属性分配给要承载的[WINDOWS UI。](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) 此类还具有其他成员，用于将键盘焦点导航入和移出 XAML 孤岛。 |
| IDesktopWindowXamlSourceNative | 此 COM 接口提供**AttachToWindow**方法，可用于将应用中的 XAML 岛附加到父 UI 元素。 每个**DesktopWindowXamlSource**对象都实现此接口。 此接口是在 desktopwindowxamlsource 中声明的。 |
| IDesktopWindowXamlSourceNative2 | 此 COM 接口提供**PreTranslateMessage**方法，该方法可使 UWP XAML 框架正确处理某些 Windows 消息。 每个**DesktopWindowXamlSource**对象都实现此接口。 此接口是在 desktopwindowxamlsource 中声明的。 |

下图说明了在桌面应用中托管的 XAML 岛中对象的层次结构。

* 在基本级别上，是你要在其中托管 XAML 岛的应用程序中的 UI 元素。 此 UI 元素必须具有一个窗口句柄（HWND）。 可在其中承载 XAML 岛的 UI 元素的示例包括C++适用于 Win32 应用的[窗口](https://docs.microsoft.com/windows/desktop/winmsg/about-windows)、system.windows.interop.hwndhost> for WPF apps，以及用于 Windows 窗体应用的[System.Windows.Interop.HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) 。 [System.Windows.Forms.Control](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)

* 下一级别是**DesktopWindowXamlSource**对象。 此对象提供用于托管 XAML 岛的基础结构。 你的代码负责创建此对象并将其附加到父 UI 元素。

* 当您创建**DesktopWindowXamlSource**时，此对象将自动创建一个本机子窗口以承载 UWP 控件。 此本机子窗口主要是从代码中提取出来的，但如果需要，可以访问其句柄（HWND）。

* 最后，最上层是要在桌面应用中托管的 UWP 控件。 这可以是从[Windows](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)派生的任何 uwp 对象，包括 Windows SDK 提供的任何 uwp 控件以及自定义用户控件。

![DesktopWindowXamlSource 体系结构](images/xaml-islands/xaml-hosting-api-rev2.png)

> [!NOTE]
> 在桌面应用中托管 XAML 岛时，可以同时在同一线程上运行多个 XAML 内容树。 若要访问 XAML 岛中 XAML 内容树的根元素并获取在其中托管它的上下文的相关信息，请使用 [XamlRoot](https://docs.microsoft.com/uwp/api/windows.ui.xaml.xamlroot) 类。 [CoreWindow](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow)、 [w](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview)和[WINDOW](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window) api 不会为 XAML 孤岛提供正确的信息。 有关详细信息，请参阅[此部分](xaml-islands.md#window-host-context-for-xaml-islands)。

## <a name="troubleshooting"></a>故障排除

### <a name="error-using-uwp-xaml-hosting-api-in-a-uwp-app"></a>在 UWP 应用中使用 UWP XAML 托管 API 时出错

| 问题 | 分辨率 |
|-------|------------|
| 应用收到**COMException** ，其中包含以下消息： "无法激活 DesktopWindowXamlSource。 此类型不能在 UWP 应用中使用。 或 "无法激活 WindowsXamlManager。 此类型不能在 UWP 应用中使用。 | 此错误指示你正在尝试使用 uwp 应用中的 UWP XAML 宿主 API （具体而言，是尝试实例化[DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)或[WindowsXamlManager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager)类型）。 UWP XAML 宿主 API 仅适用于非 UWP 桌面应用，如 WPF、Windows 窗体和C++ Win32 应用程序。 |

### <a name="error-trying-to-use-the-windowsxamlmanager-or-desktopwindowxamlsource-types"></a>尝试使用 WindowsXamlManager 或 DesktopWindowXamlSource 类型时出错

| 问题 | 分辨率 |
|-------|------------|
| 您的应用程序收到一个异常，其中包含以下消息： "WindowsXamlManager 和 DesktopWindowXamlSource 支持面向 Windows 版本10.0.18226.0 和更高版本的应用程序。 请检查应用程序清单或包清单，确保 MaxTestedVersion 属性已更新。 " | 此错误表示你的应用程序尝试在 UWP XAML 宿主 API 中使用**WindowsXamlManager**或**DesktopWindowXamlSource**类型，但 OS 无法确定此应用是否构建为面向 Windows 10 1903 版或更高版本。 在早期版本的 Windows 10 中，UWP XAML 宿主 API 首先作为预览引入，但仅支持从 Windows 10 版本1903开始。</p></p>若要解决此问题，请为应用程序创建 .msix 包，并从包中运行它，或在项目中安装[NuGet 包。](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK)  |

### <a name="error-attaching-to-a-window-on-a-different-thread"></a>附加到不同线程的窗口时出错

| 问题 | 分辨率 |
|-------|------------|
| 应用收到**COMException** ，其中包含以下消息： "AttachToWindow 方法失败，因为指定的 HWND 是在另一个线程上创建的。" | 此错误表示你的应用程序调用了**IDesktopWindowXamlSourceNative：： AttachToWindow**方法，并向其传递了在不同线程上创建的窗口的 HWND。 您必须传递此方法，它是在与您从中调用方法的代码相同的线程上创建的窗口的 HWND。 |

### <a name="error-attaching-to-a-window-on-a-different-top-level-window"></a>附加到不同顶级窗口的窗口时出错

| 问题 | 分辨率 |
|-------|------------|
| 你的应用收到**COMException** ，其中包含以下消息： "AttachToWindow 方法失败，因为指定的 HWND 从与以前传递到同一线程上的 ATTACHTOWINDOW 的 HWND 更不同的顶级窗口。" | 此错误表示你的应用程序调用了**IDesktopWindowXamlSourceNative：： AttachToWindow**方法，并向其传递了一个窗口的 HWND，该窗口与你先前在同一线程上对此方法的调用中指定的窗口相比，</p></p>在应用程序调用特定线程上的**AttachToWindow**后，同一线程上的所有其他[DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)对象只能附加到作为首次调用**AttachToWindow**时传递的相同顶级窗口的后代的 windows。 当关闭特定线程的所有**DesktopWindowXamlSource**对象时，接下来的**DesktopWindowXamlSource**可以自由地附加到任何窗口。</p></p>若要解决此问题，请关闭绑定到此线程上其他顶级窗口的所有**DesktopWindowXamlSource**对象，或为此**DesktopWindowXamlSource**创建新线程。 |

## <a name="related-topics"></a>相关主题

* [在桌面应用中托管 UWP XAML 控件（XAML 孤岛）](xaml-islands.md)
* [在C++ Win32 应用程序中托管标准 UWP 控件](host-standard-control-with-xaml-islands-cpp.md)
* [在C++ Win32 应用程序中托管自定义 UWP 控件](host-custom-control-with-xaml-islands-cpp.md)
* [Win32 应用中C++ XAML 孤岛的高级方案](advanced-scenarios-xaml-islands-cpp.md)
* [XAML 孤岛代码示例](https://github.com/microsoft/Xaml-Islands-Samples)
* [C++Win32 XAML 孤岛示例](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Samples/Win32/SampleCppApp)
