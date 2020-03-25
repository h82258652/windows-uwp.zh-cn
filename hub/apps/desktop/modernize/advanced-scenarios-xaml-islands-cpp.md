---
description: 本文介绍 Win32 应用程序的C++高级 XAML 岛承载方案。
title: Win32 应用中C++ XAML 孤岛的高级方案
ms.date: 03/23/2020
ms.topic: article
keywords: windows 10、uwp、cpp、win32、xaml 孤岛、包装控件、标准控件
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 50ee005fc0de52a3e0217a71fb3d391445c486db
ms.sourcegitcommit: c660def841abc742600fbcf6ed98e1f4f7beb8cc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/24/2020
ms.locfileid: "80226231"
---
# <a name="advanced-scenarios-for-xaml-islands-in-c-win32-apps"></a>Win32 应用中C++ XAML 孤岛的高级方案

[宿主标准 uwp 控件](host-standard-control-with-xaml-islands-cpp.md)和[宿主自定义 uwp 控件](host-custom-control-with-xaml-islands-cpp.md)文章提供了在C++ Win32 应用程序中托管 XAML 孤岛的说明和示例。 不过，这些文章中的代码示例不处理许多高级方案，桌面应用程序可能需要处理这些方案来提供平稳的用户体验。 本文提供了有关这些方案的指南，并提供指向相关代码示例的链接。

## <a name="keyboard-input"></a>键盘输入

若要正确处理每个 XAML 岛的键盘输入，应用程序必须将所有 Windows 消息传递给 UWP XAML 框架，以便能够正确处理某些消息。 为此，可以在应用程序中访问消息循环的某个位置，将每个 XAML 岛的**DesktopWindowXamlSource**对象转换为**IDesktopWindowXamlSourceNative2** COM 接口。 然后，调用此接口的**PreTranslateMessage**方法，并传入当前消息。

  * Win32：：应用可以直接在其主消息循环中调用**PreTranslateMessage** 。 **C++** 有关示例，请参阅[XamlBridge](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/XamlBridge.cpp#L16)文件。

  * **WPF：** 应用可以从[ComponentDispatcher](https://docs.microsoft.com/dotnet/api/system.windows.interop.componentdispatcher.threadfiltermessage)事件的事件处理程序调用**PreTranslateMessage** 。 有关示例，请参阅 Windows 社区工具包中的[WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs#L177)文件。

  * **Windows 窗体：** 应用可以通过[system.windows.forms.control.preprocessmessage](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.preprocessmessage)方法的替代调用**PreTranslateMessage** 。 有关示例，请参阅 Windows 社区工具包中的[WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs#L100)文件。

## <a name="keyboard-focus-navigation"></a>键盘焦点导航

当用户使用键盘在应用程序中浏览 UI 元素（例如，按**tab**或方向/箭头键）时，需要以编程方式将焦点移入和移出**DesktopWindowXamlSource**对象。 当用户的键盘导航到达**DesktopWindowXamlSource**时，将焦点移到你的 UI 的导航顺序中的第一个[windows](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) DesktopWindowXamlSource 对象中，在用户循环访问元素时继续将焦点移到以下**windows. uielement**对象，然后将焦点移出**DesktopWindowXamlSource**并移入父 UI 元素。  

UWP XAML 宿主 API 提供若干类型和成员，以帮助你完成这些任务。

* 当键盘导航进入**DesktopWindowXamlSource**时，将引发[GotFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.gotfocus)事件。 处理此事件，并使用[NavigateFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.navigatefocus)方法以编程方式将焦点移到第一个承载的**Windows。**

* 当用户在**DesktopWindowXamlSource**中的最后一个可获得焦点的元素上并按**Tab**键或箭头键时，将引发[TakeFocusRequested](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.takefocusrequested)事件。 处理此事件并以编程方式将焦点移到主机应用程序中的下一个可获得焦点的元素。 例如，在 WPF 应用程序中， **DesktopWindowXamlSource**托管在[system.windows.interop.hwndhost>](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)中，可以使用[system.windows.frameworkelement.movefocus](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.movefocus)方法将焦点转移到主机应用程序中下一个可设定焦点的元素。

有关演示如何在运行的示例应用程序的上下文中执行此操作的示例，请参阅以下代码文件：

  * /Win32：请参阅[XamlBridge](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/XamlBridge.cpp)文件。  **C++**

  * **WPF：** 请参阅 Windows 社区工具包中的[WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs)文件。  

  * **Windows 窗体：** 请参阅 Windows 社区工具包中的[WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs)文件。

## <a name="handle-layout-changes"></a>处理布局更改

当用户更改父 UI 元素的大小时，需要处理任何必要的布局更改，以确保 UWP 控件按预期显示。 下面是一些需要考虑的重要方案。

* 在C++ Win32 应用程序中，当应用程序处理 WM_SIZE 消息时，它可以使用[SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos)函数重新定位托管的 XAML 岛。 有关示例，请参阅[sampleapp.exe](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/SampleApp.cpp#L170)代码文件。

* 当父 UI 元素需要获取适合你在**DesktopWindowXamlSource**上**承载的所**需的矩形区域的大小时，请调用**Windows**的[Measure](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.measure)方法，然后再调用方法。 例如：

    * 在 WPF 应用程序中，你可以从承载**DesktopWindowXamlSource**的[system.windows.interop.hwndhost>](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)的[system.windows.frameworkelement.measureoverride](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.measureoverride)方法执行此操作。 有关示例，请参阅 Windows 社区工具包中的[WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs)文件。

    * 在 Windows 窗体应用程序中，你可以从承载**DesktopWindowXamlSource**的[控件](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)的[GetPreferredSize](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.getpreferredsize)方法执行此操作。 有关示例，请参阅 Windows 社区工具包中的[WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs)文件。

* 父 UI 元素的大小发生更改时，调用在**DesktopWindowXamlSource**上托管的根**Windows Ui**的 "[排列](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.arrange)" 方法。 例如：

    * 在 WPF 应用程序中，你可以从承载**DesktopWindowXamlSource**的[System.windows.interop.hwndhost>](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)对象的[system.windows.frameworkelement.arrangeoverride](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.arrangeoverride)方法执行此操作。 有关示例，请参阅 Windows 社区工具包中的[WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs)文件。

    * 在 Windows 窗体应用程序中，你可以从承载**DesktopWindowXamlSource**的[控件](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)的[SizeChanged](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.sizechanged)事件的处理程序中执行此操作。 有关示例，请参阅 Windows 社区工具包中的[WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs)文件。

## <a name="handle-dpi-changes"></a>处理 DPI 更改

UWP XAML 框架自动处理托管 UWP 控件的 DPI 更改（例如，当用户在具有不同屏幕 DPI 的监视器之间拖动窗口时）。 为了获得最佳体验，我们建议将 Windows 窗体、WPF 或C++ Win32 应用程序配置为每监视器 DPI 感知。

若要将应用程序配置为每监视器 DPI 感知，请将[并行程序集清单](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests)添加到项目中，并将 **\<dpiAwareness\>** 元素设置为**PerMonitorV2**。 有关此值的详细信息，请参阅[DPI_AWARENESS_CONTEXT_PER_MONITOR_AWARE_V2](https://docs.microsoft.com/windows/desktop/hidpi/dpi-awareness-context)的说明。

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

* [在桌面应用中托管 UWP XAML 控件（XAML 孤岛）](xaml-islands.md)
* [在C++ Win32 应用中使用 UWP XAML 宿主 API](using-the-xaml-hosting-api.md)
* [在C++ Win32 应用程序中托管标准 UWP 控件](host-standard-control-with-xaml-islands-cpp.md)
* [在C++ Win32 应用程序中托管自定义 UWP 控件](host-custom-control-with-xaml-islands-cpp.md)
* [XAML 孤岛代码示例](https://github.com/microsoft/Xaml-Islands-Samples)
* [C++Win32 XAML 孤岛示例](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Samples/Win32/SampleCppApp)
