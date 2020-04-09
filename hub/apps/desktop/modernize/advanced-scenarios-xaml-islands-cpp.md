---
description: 本文介绍 C++ Win32 应用的高级 XAML 岛托管方案。
title: C++ Win32 应用中 XAML 岛的高级方案
ms.date: 03/23/2020
ms.topic: article
keywords: windows 10, uwp, cpp, win32, xaml 岛, 包装控件, 标准控件
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 50ee005fc0de52a3e0217a71fb3d391445c486db
ms.sourcegitcommit: c660def841abc742600fbcf6ed98e1f4f7beb8cc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/24/2020
ms.locfileid: "80226231"
---
# <a name="advanced-scenarios-for-xaml-islands-in-c-win32-apps"></a>C++ Win32 应用中 XAML 岛的高级方案

[托管标准 UWP 控件](host-standard-control-with-xaml-islands-cpp.md)和[托管自定义 UWP 控件](host-custom-control-with-xaml-islands-cpp.md)文章提供了在 C++ Win32 应用中托管 XAML 岛的说明和示例。 不过，这些文章中的代码示例并不能处理桌面应用程序可能需要处理以提供流畅用户体验的许多高级方案。 本文提供了有关这些方案的指南，并提供指向相关代码示例的链接。

## <a name="keyboard-input"></a>键盘输入

若要正确处理每个 XAML 岛的键盘输入，应用程序必须将所有 Windows 消息传递给 UWP XAML 框架，以便能够正确处理某些消息。 为此，在应用程序中可以访问消息循环的某个位置，将每个 XAML 岛的 DesktopWindowXamlSource  对象强制转换为 IDesktopWindowXamlSourceNative2  COM 接口。 然后，调用此接口的 PreTranslateMessage  方法，并传入当前消息。

  * **C++ Win32：** 应用可直接在其主消息循环中调用 PreTranslateMessage  。 有关示例，请参阅 [XamlBridge.cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/XamlBridge.cpp#L16) 文件。

  * **WPF：** 应用可以从 [ComponentDispatcher.ThreadFilterMessage](https://docs.microsoft.com/dotnet/api/system.windows.interop.componentdispatcher.threadfiltermessage) 事件的事件处理程序调用 PreTranslateMessage  。 有关示例，请参阅 Windows 社区工具包中的 [WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs#L177) 文件。

  * **Windows 窗体：** 应用可以从 [Control.PreprocessMessage](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.preprocessmessage) 的替代方法调用 PreTranslateMessage  。 有关示例，请参阅 Windows 社区工具包中的 [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs#L100) 文件。

## <a name="keyboard-focus-navigation"></a>键盘焦点导航

当用户使用键盘在应用程序中导航 UI 元素时（例如，通过按 Tab  或方向/箭头键），需要以编程方式将焦点移入和移出 DesktopWindowXamlSource  对象。 当用户的键盘导航到达 DesktopWindowXamlSource  时，将焦点移到 UI 导航顺序中的第一个 [Windows.UI.Xaml.UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) 对象中，在用户循环访问元素时继续将焦点移到下面的 Windows.UI.Xaml.UIElement  对象，然后将焦点移出 DesktopWindowXamlSource  并移入父 UI 元素。  

UWP XAML 托管 API 提供若干类型和成员，可帮助你完成这些任务。

* 当键盘导航进入 DesktopWindowXamlSource  时，将引发 [GotFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.gotfocus) 事件。 处理此事件，并使用 [NavigateFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.navigatefocus) 方法以编程方式将焦点移到第一个托管的 Windows.UI.Xaml.UIElement  。

* 当用户位于 DesktopWindowXamlSource  中的最后一个可聚焦的元素上，按 Tab  键或箭头键将引发 [TakeFocusRequested](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.takefocusrequested) 事件。 处理此事件，并以编程方式将焦点移到主机应用程序的下一个可聚焦的元素。 例如，在 WPF 应用程序中，DesktopWindowXamlSource  托管在 [System.Windows.Interop.HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) 中，可以使用 [MoveFocus](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.movefocus) 方法将焦点转移到主机应用程序中的下一个可聚焦元素。

有关演示如何在运行的示例应用程序的上下文中执行此操作的示例，请参阅以下代码文件：

  * **C++/Win32**：请参阅 [XamlBridge.cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/XamlBridge.cpp) 文件。

  * **WPF：** 请参阅 Windows 社区工具包中的 [WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs) 文件。  

  * **Windows 窗体：** 请参阅 Windows 社区工具包中的 [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs) 文件。

## <a name="handle-layout-changes"></a>处理布局更改

当用户更改父 UI 元素的大小时，需要处理任何必要的布局更改，以确保 UWP 控件按预期显示。 下面是一些需要考虑的重要场景。

* 在 C++ Win32 应用程序中，当应用程序处理 WM_SIZE 消息时，它可以通过使用 [SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos) 函数重定位托管的 XAML 岛。 有关示例，请参阅 [SampleApp.cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/SampleApp.cpp#L170) 代码文件。

* 当父 UI 元素需要获取所需的矩形区域的大小以适应托管在 DesktopWindowXamlSource  上的 Windows.UI.Xaml.UIElement  时，请调用 Windows.UI.Xaml.UIElement  的 [Measure](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.measure) 方法。 例如：

    * 在 WPF 应用程序中，你可以通过托管 DesktopWindowXamlSource  的 [HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) 的 [MeasureOverride](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.measureoverride) 方法执行此操作。 有关示例，请参阅 Windows 社区工具包中的 [WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) 文件。

    * 在 Windows 窗体应用程序中，你可以通过托管 DesktopWindowXamlSource  的 [Control](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) 的 [GetPreferredSize](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.getpreferredsize) 方法执行此操作。 有关示例，请参阅 Windows 社区工具包中的 [WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) 文件。

* 当父 UI 元素的大小发生更改时，请调用托管在 DesktopWindowXamlSource  上的根 Windows.UI.Xaml.UIElement  的 [Arrange](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.arrange) 方法。 例如：

    * 在 WPF 应用程序中，你可以通过托管 DesktopWindowXamlSource  的 [HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) 对象的 [ArrangeOverride](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.arrangeoverride) 方法执行此操作。 有关示例，请参阅 Windows 社区工具包中的 [WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) 文件。

    * 在 Windows 窗体应用程序中，你可以通过托管 DesktopWindowXamlSource  的 [Control](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) 的 [SizeChanged](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.sizechanged) 事件的处理程序执行此操作。 有关示例，请参阅 Windows 社区工具包中的 [WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) 文件。

## <a name="handle-dpi-changes"></a>处理 DPI 变更

UWP XAML 框架自动处理托管 UWP 控件的 DPI 变更（例如，当用户在具有不同屏幕 DPI 的监视器之间拖动窗口时）。 为了获得最佳体验，建议将 Windows 窗体、WPF 或C++ Win32 应用程序配置为感知每个监视器 DPI。

若要将应用程序配置为感知每个监视器 DPI，请将[并行程序集清单](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests)添加到项目，并将 \<dpiAwareness **\>** 元素设置为“PerMonitorV2”  。 有关此值的详细信息，请参阅 [DPI_AWARENESS_CONTEXT_PER_MONITOR_AWARE_V2](https://docs.microsoft.com/windows/desktop/hidpi/dpi-awareness-context) 说明。

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<assembly xmlns="urn:schemas-microsoft-com:asm.v1" manifestVersion="1.0">
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
        <windowsSettings>
            <dpiAwareness xmlns="http://schemas.microsoft.com/SMI/2016/WindowsSettings">PerMonitorV2</dpiAwareness>
        </windowsSettings>
    </application>
</assembly>
```

## <a name="related-topics"></a>相关主题

* [在桌面应用中托管 UWP XAML 控件（XAML 岛）](xaml-islands.md)
* [在 C++ Win32 应用中使用 UWP XAML 托管 API](using-the-xaml-hosting-api.md)
* [在 C++ Win32 应用中托管标准 UWP 控件](host-standard-control-with-xaml-islands-cpp.md)
* [在 C++ Win32 应用中托管自定义 UWP 控件](host-custom-control-with-xaml-islands-cpp.md)
* [XAML 岛代码示例](https://github.com/microsoft/Xaml-Islands-Samples)
* [C++ Win32 XAML 岛示例](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Samples/Win32/SampleCppApp)
