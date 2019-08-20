---
description: 本文介绍如何在桌面应用程序中托管 UWP XAML UI。
title: 在桌面应用程序中使用 UWP XAML 宿主 API
ms.date: 07/26/2019
ms.topic: article
keywords: windows 10、uwp、windows 窗体、wpf、win32、xaml 孤岛
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 14607dc3e3b32f39a840d623c7a887bb8b3687c5
ms.sourcegitcommit: f6af7aeb8506379a184207035c8e43288cb31453
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/29/2019
ms.locfileid: "68601529"
---
# <a name="using-the-uwp-xaml-hosting-api-in-a-desktop-application"></a>在桌面应用程序中使用 UWP XAML 宿主 API

从 Windows 10 版本1903开始, 非 UWP 桌面应用程序 (包括 WPF、Windows 窗体和C++ Win32 应用程序) 可以使用*UWP XAML 宿主 API*在与窗口句柄 (HWND) 关联的任何 UI 元素中承载 uwp 控件。 此 API 使非 UWP 桌面应用程序能够使用只能通过 UWP 控件获取的最新 Windows 10 UI 功能。 例如, 非 UWP 桌面应用程序可使用此 API 托管使用[熟知设计系统](/windows/uwp/design/fluent-design-system/index)并支持[WINDOWS 墨迹](/windows/uwp/design/input/pen-and-stylus-interactions)的 UWP 控件。

UWP XAML 宿主 API 为一组更广泛的控件提供基础, 使开发人员能够将熟知的 UI 引入非 UWP 桌面应用程序。 此功能称为*XAML 孤岛*。 有关此功能的概述, 请参阅[桌面应用程序中的 UWP 控件](xaml-islands.md)。

> [!NOTE]
> 如果你有关于 XAML 孤岛的反馈, 请在[Microsoft 工具包](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues)存储库中创建一个新问题, 并在此处留下你的意见。 如果您想要私下提交您的反馈, 则可以将其XamlIslandsFeedback@microsoft.com发送到。 你的见解和方案对我们至关重要。

## <a name="should-you-use-the-uwp-xaml-hosting-api"></a>是否应使用 UWP XAML 宿主 API？

UWP XAML 宿主 API 提供低级别的基础结构, 用于在桌面应用程序中承载 UWP 控件。 某些类型的桌面应用程序可以选择使用替代的更方便的 Api 来实现此目标。  

* 如果你有C++ Win32 桌面应用程序, 并且想要在应用程序中托管 UWP 控件, 则必须使用 UWP XAML 宿主 API。 此类应用程序没有替代项。

* 对于 WPF 和 Windows 窗体应用程序, 我们强烈建议你在 Windows 社区工具包中使用[包装控件](xaml-islands.md#wrapped-controls)和[主机控件](xaml-islands.md#host-controls), 而不是直接使用 UWP XAML 托管 API。 这些控件在内部使用 UWP XAML 宿主 API, 并实现你在自行使用 UWP XAML 托管 API 时需要自行处理的所有行为, 包括键盘导航和布局更改。 但是, 如果您选择, 则可以直接在这些类型的应用程序中使用 UWP XAML 宿主 API。

本文介绍如何直接在应用程序中使用 UWP XAML 宿主 API, 而不是 Windows 社区工具包提供的控件。

## <a name="prerequisites"></a>先决条件

UWP XAML 宿主 API 具有以下先决条件:

* Windows 10 版本 1903 (或更高版本) 以及相应的 Windows SDK 版本。
* 按照[这些说明](xaml-islands.md#requirements), 将项目配置为使用 Windows 运行时 api 并启用 XAML 孤岛。

## <a name="architecture-of-xaml-islands"></a>XAML 孤岛的体系结构

UWP XAML 宿主 API 包含以下主要 Windows 运行时类型和 COM 接口:

* [**WindowsXamlManager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager)。 此类表示 UWP XAML framework。 此类提供单个静态[**InitializeForCurrentThread**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread)方法, 该方法可初始化桌面应用程序中当前线程上的 UWP XAML 框架。

* [**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)。 此类表示你在桌面应用程序中托管的 UWP XAML 内容的实例。 此类中最重要的成员是[**Content**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content)属性。 将此属性分配给要承载的[**WINDOWS UI。** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) 此类还具有其他成员, 用于将键盘焦点导航入和移出 XAML 孤岛。

* **IDesktopWindowXamlSourceNative**和**IDesktopWindowXamlSourceNative2** COM 接口。 **IDesktopWindowXamlSourceNative**提供**AttachToWindow**方法, 使用该方法可以将应用程序中的 XAML 岛附加到父 UI 元素。 **IDesktopWindowXamlSourceNative2**提供**PreTranslateMessage**方法, 该方法允许 UWP XAML 框架正确处理某些 Windows 消息。
    > [!NOTE]
    > 这些接口是在 Windows SDK 中的**desktopwindowxamlsource**头文件中声明的。 默认情况下, 此文件位于% programfiles (x86)% \ Windows Kits\10\Include\\< 内部版本号\>\um。 在C++ Win32 项目中, 可以直接引用此标头文件。 在 WPF 或 Windows 窗体项目中, 需要使用[**ComImport**](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.comimportattribute)属性在应用程序代码中声明接口。 请确保您的接口声明与**desktopwindowxamlsource**中的声明完全匹配。

下图说明了在桌面应用程序中承载的 XAML 岛中对象的层次结构。

![DesktopWindowXamlSource 体系结构](images/xaml-islands/xaml-hosting-api-rev2.png)

* 基本级别是应用程序中的 UI 元素, 要在其中托管 XAML 岛。 此 UI 元素必须具有一个窗口句柄 (HWND)。 可在其中承载 XAML 岛的 UI 元素的示例包括用于 WPF 应用程序的  [System.Windows.Interop.HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) 、用于Windows 窗体应用程序的[System.Windows.Forms.Control](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)和C++ Win32窗口应用程序 [window](https://docs.microsoft.com/windows/desktop/winmsg/about-windows).

* 下一级别是**DesktopWindowXamlSource**对象。 此对象提供用于托管 XAML 岛的基础结构。 你的代码负责创建此对象并将其附加到父 UI 元素。

* 当您创建**DesktopWindowXamlSource**时, 此对象将自动创建一个本机子窗口以承载 UWP 控件。 此本机子窗口主要是从代码中提取出来的, 但如果需要, 可以访问其句柄 (HWND)。

* 最后, 最上层是要在桌面应用程序中托管的 UWP 控件。 这可以是从[**Windows**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)派生的任何 uwp 对象, 包括 Windows SDK 提供的任何 uwp 控件以及自定义用户控件。

## <a name="related-samples"></a>相关示例

您在代码中使用 UWP XAML 宿主 API 的方式取决于您的应用程序类型、应用程序的设计和其他因素。 为了帮助说明如何在完整应用程序的上下文中使用此 API, 本文引用了以下示例中的代码。

### <a name="c-win32"></a>C++Win32

[ C++ Win32 示例](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App)。 此示例演示了在未打包C++的 Win32 应用程序 (即, 未内置于 .msix 包中的应用程序) 中承载 UWP 用户控件的完整实现。

### <a name="wpf-and-windows-forms"></a>WPF 和 Windows 窗体

Windows 社区工具包中的[WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)控件充当参考示例, 用于在 WPF 和 Windows 窗体应用程序中使用 UWP 宿主 API。 源代码位于以下位置:

  * 对于控件的 WPF 版本, 请[参阅此处](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Wpf.UI.XamlHost)。 WPF 版本派生自[**system.windows.interop.hwndhost>** ](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)。

  * 对于控件的 Windows 窗体版本, 请[参阅此处](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Forms.UI.XamlHost)。 Windows 窗体版本派生自[**system.object**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)。

## <a name="host-uwp-xaml-controls"></a>宿主 UWP XAML 控件

下面是在应用程序中使用 UWP XAML 宿主 API 承载 UWP 控件的主要步骤。

1. 在您的应用程序创建它将在[**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)中承载的任何[**Windows UI. UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)对象之前, 为当前线程初始化 UWP XAML framework。

    * 如果你的应用程序在创建任何**DesktopWindowXamlSource**对象之前创建了该对象, 则在实例化**DesktopWindowXamlSource**对象时, 将初始化此框架. 在这种情况下, 你无需添加自己的任何代码来初始化该框架。

    * 但是, 如果**你的应用**程序在创建将承载这些对象的**DesktopWindowXamlSource**对象之前创建了这些对象, 则你的应用程序必须调用静态[**WindowsXamlManager InitializeForCurrentThread**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread)方法, 在实例化**Windows**对象之前显式初始化 UWP xaml framework。 在实例化承载**DesktopWindowXamlSource**的父 UI 元素时, 应用程序通常应调用此方法。

    ```cppwinrt
    Windows::UI::Xaml::Hosting::WindowsXamlManager windowsXamlManager =
        Windows::UI::Xaml::Hosting::WindowsXamlManager::InitializeForCurrentThread();
    ```

    ```csharp
    global::Windows.UI.Xaml.Hosting.WindowsXamlManager windowsXamlManager =
        global::Windows.UI.Xaml.Hosting.WindowsXamlManager.InitializeForCurrentThread();
    ```

    > [!NOTE]
    > 此方法返回一个[**WindowsXamlManager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager)对象, 该对象包含对 UWP XAML 框架的引用。 您可以根据需要在给定线程上创建任意多个**WindowsXamlManager**对象。 但是, 因为每个对象都包含对 UWP XAML framework 的引用, 所以你应该释放这些对象以确保最终释放 XAML 资源。

2. 创建一个[**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)对象, 并将其附加到应用程序中与窗口句柄关联的父 UI 元素。

    为此, 您需要执行以下步骤:

    1. 创建一个**DesktopWindowXamlSource**对象, 并将其转换为**IDesktopWindowXamlSourceNative**或**IDesktopWindowXamlSourceNative2** COM 接口。

    2. 调用**IDesktopWindowXamlSourceNative**或**IDesktopWindowXamlSourceNative2**接口的**AttachToWindow**方法, 并传入应用程序中父 UI 元素的窗口句柄。

    3. 设置**DesktopWindowXamlSource**中包含的内部子窗口的初始大小。 默认情况下, 此内部子窗口的宽度和高度均设置为0。 如果未设置窗口的大小, 则添加到**DesktopWindowXamlSource**的任何 UWP 控件都将不可见。 若要访问**DesktopWindowXamlSource**中的内部子窗口, 请使用**IDesktopWindowXamlSourceNative**或**IDesktopWindowXamlSourceNative2**接口的**WindowHandle**属性。 下面的示例使用[SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos)函数设置窗口的大小。

    下面是一些演示此过程的代码示例。

    ```cppwinrt
    // This example assumes you already have an HWND variable named 'parentHwnd' that
    // contains the handle of the parent window.
    Windows::UI::Xaml::Hosting::DesktopWindowXamlSource desktopWindowXamlSource;
    auto interop = desktopWindowXamlSource.as<IDesktopWindowXamlSourceNative>();
    check_hresult(interop->AttachToWindow(parentHwnd));

    HWND childInteropHwnd = nullptr;
    interop->get_WindowHandle(&childInteropHwnd);

    SetWindowPos(childInteropHwnd, 0, 0, 0, 300, 300, SWP_SHOWWINDOW);
    ```

    ```csharp
    // This WPF example assumes you already have an HwndHost named 'parentHwndHost'
    // that will act as the parent UI element for your XAML Island. It also assumes
    // you have used the DllImport attribute to import SetWindowPos from user32.dll
    // as a static method into a class named NativeMethods.
    Windows.UI.Xaml.Hosting.DesktopWindowXamlSource desktopWindowXamlSource =
        new Windows.UI.Xaml.Hosting.DesktopWindowXamlSource();

    IntPtr iUnknownPtr = System.Runtime.InteropServices.Marshal.GetIUnknownForObject(
        desktopWindowXamlSource);
    IDesktopWindowXamlSourceNative desktopWindowXamlSourceNative =
        System.Runtime.InteropServices.Marshal.Marshal.GetTypedObjectForIUnknown(
            iUnknownPtr, typeof(IDesktopWindowXamlSourceNative))
            as IDesktopWindowXamlSourceNative;

    desktopWindowXamlSourceNative.AttachToWindow(parentHwndHost.Handle);

    var childInteropHwnd = desktopWindowXamlSourceNative.WindowHandle;
    NativeMethods.SetWindowPos(childInteropHwnd, HWND_TOP, 0, 0, 300, 300, SWP_SHOWWINDOW);
    ```

3. 设置要承载到你的**DesktopWindowXamlSource**对象的[**Content**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content)属性的**Windows。** 下面的示例将名为的 Windows 的 " [**UI**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid) " 命名```myGrid```为 " **Content** " 属性。

   ```cppwinrt
   desktopWindowXamlSource.Content(myGrid);
   ```

   ```csharp
   desktopWindowXamlSource.Content = myGrid;
   ```

有关在工作的示例应用程序的上下文中演示这些任务的完整示例, 请参阅以下代码文件:

  * **C++Win32**请参阅[ C++ Win32 示例](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App)中的[XamlBridge](https://github.com/marb2000/XamlIslands/blob/master/1903_Samples/CppWinRT_Win32_App/SampleCppApp/XamlBridge.cpp)文件。

  * **WPF**请参阅 Windows 社区工具包中的[WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.cs)和[WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHost.cs)文件。  

  * **Windows 窗体:** 请参阅 Windows 社区工具包中的[WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.cs)和[WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHost.cs)文件。

## <a name="handle-keyboard-input-and-focus-navigation"></a>处理键盘输入和焦点导航

当应用程序托管 XAML 孤岛时, 需要执行几项操作才能适当处理键盘输入和焦点导航。

### <a name="keyboard-input"></a>键盘输入

若要正确处理每个 XAML 岛的键盘输入, 应用程序必须将所有 Windows 消息传递给 UWP XAML 框架, 以便能够正确处理某些消息。 为此, 可以在应用程序中访问消息循环的某个位置, 将每个 XAML 岛的**DesktopWindowXamlSource**对象转换为**IDesktopWindowXamlSourceNative2** COM 接口。 然后, 调用此接口的**PreTranslateMessage**方法, 并传入当前消息。

  * C++ Win32 应用程序可以直接在其主消息循环中调用**PreTranslateMessage** 。 有关示例, 请参阅[ C++ Win32 示例](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App)中的[XamlBridge](https://github.com/marb2000/XamlIslands/blob/master/1903_Samples/CppWinRT_Win32_App/SampleCppApp/XamlBridge.cpp#L6)文件。

  * WPF 应用程序可以从[**ComponentDispatcher**](https://docs.microsoft.com/dotnet/api/system.windows.interop.componentdispatcher.threadfiltermessage?view=netframework-4.7.2)事件的事件处理程序调用**PreTranslateMessage** 。 有关示例, 请参阅 Windows 社区工具包中的[WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs#L177)文件。

  * Windows 窗体应用程序可以通过[**system.windows.forms.control.preprocessmessage**](https://docs.microsoft.com/en-us/dotnet/api/system.windows.forms.control.preprocessmessage?view=netframework-4.7.2)方法的替代调用**PreTranslateMessage** 。 有关示例, 请参阅 Windows 社区工具包中的[WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs#L100)文件。

### <a name="keyboard-focus-navigation"></a>键盘焦点导航

当用户使用键盘在应用程序中浏览 UI 元素 (例如, 按**tab**或方向/箭头键) 时, 需要以编程方式将焦点移入和移出**DesktopWindowXamlSource**对象。 当用户的键盘导航到达**DesktopWindowXamlSource**时, 将焦点移到你的 UI [**Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) 的导航顺序中的第一个 Windows ui 中。当用户循环访问元素, 然后将焦点移出**DesktopWindowXamlSource**并移入父 UI 元素时, 则为 **Windows.UI.Xaml.UIElement**  

UWP XAML 宿主 API 提供若干类型和成员, 以帮助你完成这些任务。

* 当键盘导航进入**DesktopWindowXamlSource**时, 将引发[**GotFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.gotfocus)事件。 处理此事件, 并使用[**NavigateFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.navigatefocus)方法以编程方式将焦点移到第一个承载的**Windows。**

* 当用户在**DesktopWindowXamlSource**中的最后一个可获得焦点的元素上并按**Tab**键或箭头键时, 将引发[**TakeFocusRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.takefocusrequested)事件。 处理此事件并以编程方式将焦点移到主机应用程序中的下一个可获得焦点的元素。 例如, 在 WPF 应用程序中, **DesktopWindowXamlSource**托管在[**system.windows.interop.hwndhost>** ](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)中, 可以使用[**system.windows.frameworkelement.movefocus**](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.movefocus)方法将焦点转移到主机应用程序中下一个可设定焦点的元素。

有关演示如何在运行的示例应用程序的上下文中执行此操作的示例, 请参阅以下代码文件:

  * **WPF**请参阅 Windows 社区工具包中的[WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs)文件。  

  * **Windows 窗体:** 请参阅 Windows 社区工具包中的[WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs)文件。

  * **C++/Win32**:请参阅[ C++ Win32 示例](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App)中的[XamlBridge](https://github.com/marb2000/XamlIslands/blob/master/1903_Samples/CppWinRT_Win32_App/SampleCppApp/XamlBridge.cpp)文件。

## <a name="handle-layout-changes"></a>处理布局更改

当用户更改父 UI 元素的大小时, 需要处理任何必要的布局更改, 以确保 UWP 控件按预期显示。 下面是一些需要考虑的重要方案。

* 在C++ Win32 应用程序中, 当应用程序处理 WM_SIZE 消息时, 它可以使用[SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos)函数重定位托管的 XAML 岛。 有关示例, 请参阅[ C++ Win32 示例](https://github.com/marb2000/XamlIslands/tree/master/19H1_Insider_Samples/CppWin32App_With_Island)中的[sampleapp.exe](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island/SampleCppApp/SampleApp.cpp#L191)代码文件。

* 当父 UI 元素需要获取适合你在**DesktopWindowXamlSource**上承载的所需的矩形区域的大小时, 请调用 Windows 的[**Measure**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.measure)方法, 然后再调用。. 例如：

    * 在 WPF 应用程序中, 你可以从承载**DesktopWindowXamlSource**的[**system.windows.interop.hwndhost>** ](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)的[**system.windows.frameworkelement.measureoverride**](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.measureoverride)方法执行此操作。 有关示例, 请参阅 Windows 社区工具包中的[WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs)文件。

    * 在 Windows 窗体应用程序中, 你可以从承载**DesktopWindowXamlSource**的[**控件**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)的[**GetPreferredSize**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.getpreferredsize)方法执行此操作。 有关示例, 请参阅 Windows 社区工具包中的[WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs)文件。

* 父 UI 元素的大小发生更改时, 调用在**DesktopWindowXamlSource**上托管的根**Windows Ui**的 "[**排列**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.arrange)" 方法。 例如：

    * 在 WPF 应用程序中, 你可以从承载**DesktopWindowXamlSource**的[**System.windows.interop.hwndhost>** ](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)对象的[**system.windows.frameworkelement.arrangeoverride**](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.arrangeoverride)方法执行此操作。 有关示例, 请参阅 Windows 社区工具包中的[WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs)文件。

    * 在 Windows 窗体应用程序中, 你可以从承载**DesktopWindowXamlSource**的[**控件**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)的[**SizeChanged**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.sizechanged)事件的处理程序中执行此操作。 有关示例, 请参阅 Windows 社区工具包中的[WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs)文件。

## <a name="handle-dpi-changes"></a>处理 DPI 更改

UWP XAML 框架自动处理托管 UWP 控件的 DPI 更改 (例如, 当用户在具有不同屏幕 DPI 的监视器之间拖动窗口时)。 为了获得最佳体验, 我们建议将 Windows 窗体、WPF 或C++ Win32 应用程序配置为每监视器 DPI 感知。

若要将应用程序配置为每监视器 DPI 感知, 请向项目添加一个[并行程序集清单](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests)并将元素设置```<dpiAwareness>```为。 ```PerMonitorV2``` 有关此值的详细信息, 请参阅[**DPI_AWARENESS_CONTEXT_PER_MONITOR_AWARE_V2**](https://docs.microsoft.com/windows/desktop/hidpi/dpi-awareness-context)的说明。

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

## <a name="host-custom-uwp-xaml-controls"></a>承载自定义 UWP XAML 控件

请按照以下常规步骤在 XAML 岛中承载自定义 UWP XAML 控件 (您自己定义的控件或第三方提供的控件)。 您必须具有自定义控件的源代码, 以便可以使用您的应用程序进行编译。

1. 在宿主应用程序解决方案中, 集成自定义 UWP XAML 控件的源代码, 并生成自定义控件。

2. 将 UWP 应用程序项目添加到主机应用程序解决方案, 并将[XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) NuGet 包添加到此项目。

3. 在 UWP 应用程序项目中, 修改`App`你的类, 使其来自**XamlApplication**类 (由[XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) NuGet 包公开) 的类中。 此类型充当根元数据提供程序, 用于为应用程序的当前目录中的程序集中的自定义 UWP XAML 类型加载元数据。

4. 在您`App`的类的构造函数中, 调用基**XamlApplication**类的**Initialize**方法。

5. 在宿主应用程序项目中, 按照[上一部分](#host-uwp-xaml-controls)中所述的过程, 在 XAML 岛中承载自定义控件。

有关C++ win32 应用程序的示例, 请参阅[ C++ win32 示例](https://github.com/marb2000/XamlIslands/blob/master/1903_Samples/CppWinRT_Win32_App)中的以下项目:

* [SampleUserControl](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App/SampleUserControl):此项目实现名为`MyUserControl`的自定义 UWP XAML 控件, 该控件包含文本框、多个按钮和组合框。
* [MyApp](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App/MyApp):这是一个 UWP 应用程序项目, 其中包含前面步骤中描述的更改。
* [SampleCppApp](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App/SampleCppApp):这是在C++ XAML 岛中承载自定义 UWP XAML 控件的 Win32 应用程序项目。

有关 WPF 或 Windows 窗体应用程序的详细说明和示例, 请参阅[这些说明](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost#add-a-custom-uwp-control)。

## <a name="troubleshooting"></a>疑难解答

### <a name="error-using-uwp-xaml-hosting-api-in-a-uwp-app"></a>在 UWP 应用中使用 UWP XAML 托管 API 时出错

| 问题 | 分辨率 |
|-------|------------|
| 应用收到**COMException** , 其中包含以下消息:"无法激活 DesktopWindowXamlSource。 此类型不能在 UWP 应用中使用。 或 "无法激活 WindowsXamlManager。 此类型不能在 UWP 应用中使用。 | 此错误指示你正在尝试使用 uwp 应用中的 UWP XAML 宿主 API (具体而言, 是尝试实例化[**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)或[**WindowsXamlManager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager)类型)。 UWP XAML 宿主 API 仅适用于非 UWP 桌面应用, 如 WPF、Windows 窗体和C++ Win32 应用程序。 |

### <a name="error-attaching-to-a-window-on-a-different-thread"></a>附加到不同线程的窗口时出错

| 问题 | 分辨率 |
|-------|------------|
| 应用收到**COMException** , 其中包含以下消息:"AttachToWindow 方法失败, 因为指定的 HWND 是在另一个线程上创建的。" | 此错误表示你的应用程序调用了**IDesktopWindowXamlSourceNative:: AttachToWindow**方法, 并向其传递了在不同线程上创建的窗口的 HWND。 您必须传递此方法, 它是在与您从中调用方法的代码相同的线程上创建的窗口的 HWND。 |

### <a name="error-attaching-to-a-window-on-a-different-top-level-window"></a>附加到不同顶级窗口的窗口时出错

| 问题 | 分辨率 |
|-------|------------|
| 应用收到**COMException** , 其中包含以下消息:"AttachToWindow 方法失败, 因为指定的 HWND 从与以前传递到同一线程上的 AttachToWindow 的 HWND 不同的顶级窗口进行了递减。" | 此错误表示你的应用程序调用了**IDesktopWindowXamlSourceNative:: AttachToWindow**方法, 并向其传递了一个窗口, 该窗口与你之前对此方法的调用中指定的窗口相比, 此窗口的在同一线程上。</p></p>在应用程序调用特定线程上的**IDesktopWindowXamlSourceNative:: AttachToWindow**后, 同一线程上的所有其他[**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)对象只能附加到作为同一顶级的后代的 windows在对**IDesktopWindowXamlSourceNative:: AttachToWindow**的第一次调用中传递的窗口。 当关闭特定线程的所有**DesktopWindowXamlSource**对象时, 接下来的**DesktopWindowXamlSource**可以自由地附加到任何窗口。</p></p>若要解决此问题, 请关闭绑定到此线程上其他顶级窗口的所有**DesktopWindowXamlSource**对象, 或为此**DesktopWindowXamlSource**创建新线程。 |

## <a name="related-topics"></a>相关主题

* [桌面应用程序中的 UWP 控件](xaml-islands.md)
* [C++Win32 XAML 孤岛示例](https://github.com/marb2000/XamlIslands/tree/master/19H1_Insider_Samples/CppWin32App_With_Island)
