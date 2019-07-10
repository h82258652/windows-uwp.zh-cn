---
description: 本文介绍如何在桌面应用程序中托管 UWP XAML 用户界面。
title: 使用桌面应用程序中托管 API UWP XAML
ms.date: 04/16/2019
ms.topic: article
keywords: windows 10、 uwp、 windows 窗体、 wpf、 win32、 xaml 群岛
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: fa38c952d4d46d83ea9b4e9f0db5f516fe09ce59
ms.sourcegitcommit: f9a30bfd1e8eab50d0b1db97dd2f650ce66b5d34
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/09/2019
ms.locfileid: "67690890"
---
# <a name="using-the-uwp-xaml-hosting-api-in-a-desktop-application"></a>使用桌面应用程序中托管 API UWP XAML

从 Windows 10，版本 1903年，非 UWP 桌面应用程序 (包括 WPF、 Windows 窗体和C++Win32 应用程序) 可以使用*托管 API 的 UWP XAML*到任何与之关联的 UI 元素中的主机 UWP 控件窗口句柄 (HWND)。 此 API 允许非 UWP 桌面应用程序使用的最新的 Windows 10 UI 功能，才可通过 UWP 控件。 例如，非 UWP 桌面应用程序可以使用此 API 使用的主机 UWP 控件[Fluent 设计系统](/windows/uwp/design/fluent-design-system/index)服务与支持[Windows 墨迹](/windows/uwp/design/input/pen-and-stylus-interactions)。

托管 API UWP XAML 为更广泛的一组控件，我们提供使开发人员能够引入到非 UWP 桌面应用程序的 Fluent UI 提供了基础。 此功能被称为*XAML 群岛*。 有关此功能的概述，请参阅[桌面应用程序中的 UWP 控件](xaml-islands.md)。

> [!NOTE]
> 如果您将得到 XAML 群岛反馈，创建中的一个新问题[Microsoft.Toolkit.Win32 存储库](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues)和那里留言。 如果想要私下提交您的反馈，您可以将其发送到XamlIslandsFeedback@microsoft.com。 你的见解和方案是对我们非常重要。

## <a name="should-you-use-the-uwp-xaml-hosting-api"></a>您应该使用托管 API UWP XAML？

托管 API UWP XAML 提供低级别基础结构托管桌面应用程序中的 UWP 控件。 某些类型的桌面应用程序可以使用替代、 更方便的 Api 来实现此目标的选择。  

* 如果您有C++Win32 桌面应用程序和你希望主机 UWP 控件在您的应用程序，必须使用托管 API UWP XAML。 没有为这些类型的应用程序的替代方法。

* 对于 WPF 和 Windows 窗体应用程序，我们强烈建议你使用[包装控件](xaml-islands.md#wrapped-controls)并[主机控件](xaml-islands.md#host-controls)而不是使用直接托管 API UWP XAML 的 Windows 社区工具包中。 这些控件使用在内部托管 API UWP XAML，并实施所有您需要自己进行处理，如果使用托管 API，包括键盘导航和布局更改 UWP XAML 的行为。 但是，可以使用托管 API 直接在这些类型的应用程序中，如果你选择 UWP XAML。

本文介绍如何使用托管 API，而不是由 Windows 社区工具包提供的控件在应用程序中直接 UWP XAML。

## <a name="prerequisites"></a>先决条件

托管 API UWP XAML 具有这些系统必备组件：

* Windows 10，版本 1903年 （或更高版本） 和相应构建的 Windows SDK。
* 将项目配置为使用 Windows 运行时 Api，并按照启用 XAML 群岛[这些说明](xaml-islands.md#requirements)。

## <a name="architecture-of-xaml-islands"></a>XAML 岛的体系结构

托管 API UWP XAML 包括这些主要的 Windows 运行时类型和 COM 接口：

* [**WindowsXamlManager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager). 此类表示的 UWP XAML 框架。 此类提供一个静态[ **InitializeForCurrentThread** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread)初始化 UWP XAML 框架中的桌面应用程序的当前线程上的方法。

* [**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)。 此类表示 UWP XAML 内容托管在桌面应用程序中的一个实例。 此类的最重要成员是[**内容**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content)属性。 分配到此属性[ **Windows.UI.Xaml.UIElement** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)你想要托管。 此类还具有其他成员进行路由的键盘焦点导航执行和跳出执行 XAML 数据岛。

* **IDesktopWindowXamlSourceNative**并**IDesktopWindowXamlSourceNative2** COM 接口。 **IDesktopWindowXamlSourceNative**提供了**AttachToWindow**方法，用于将附加到父 UI 元素在应用程序中 XAML 岛。 **IDesktopWindowXamlSourceNative2**提供了**PreTranslateMessage**方法，使 UWP XAML 框架来正确处理特定 Windows 消息。
    > [!NOTE]
    > 这些接口中声明**windows.ui.xaml.hosting.desktopwindowxamlsource.h** Windows SDK 中的标头文件。 默认情况下，此文件位于 %programfiles (x86) %\Windows Kits\10\Include\\< 内部版本号\>\um。 在C++Win32 项目，您可以直接引用此标头文件。 在 WPF 或 Windows 窗体项目中，你将需要声明在您应用程序代码中使用的接口[ **ComImport** ](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.comimportattribute)属性。 请确保与完全匹配接口声明中的声明**windows.ui.xaml.hosting.desktopwindowxamlsource.h**。

下图说明了托管桌面应用程序中 XAML 岛中的对象的层次结构。

![DesktopWindowXamlSource 体系结构](images/xaml-islands/xaml-hosting-api-rev2.png)

* 在基本级别是你想要承载 XAML 岛中你的应用程序的 UI 元素。 此 UI 元素必须具有窗口句柄 (HWND)。 可以在其中托管 XAML 岛的 UI 元素的示例包括[ **System.Windows.Interop.HwndHost** ](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)用于 WPF 应用程序[ **System.Windows.Forms.Control**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) Windows 窗体应用程序，和一个[窗口](https://docs.microsoft.com/windows/desktop/winmsg/about-windows)为C++Win32 应用程序。

* 在下一级别是**DesktopWindowXamlSource**对象。 此对象提供用于承载 XAML 岛的基础结构。 你的代码负责创建此对象并将其附加到父 UI 元素。

* 当您创建**DesktopWindowXamlSource**，此对象会自动创建一个本机子窗口来托管 UWP 控件。 此本机子窗口主要抽象对象远离你的代码，但是您可以访问其句柄 (HWND) 如有必要。

* 最后，最高级别是你要在桌面应用程序中托管的 UWP 控件。 这可以是任何 UWP 对象派生自[ **Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)，包括任何由 Windows SDK 提供的 UWP 控件以及自定义用户控件。

## <a name="related-samples"></a>相关示例

使用在代码中托管 API UWP XAML 的方式取决于您的应用程序类型，应用程序和其他因素的设计。 为了演示了如何使用此 API 的完整应用程序上下文中，此篇文章介绍了代码从下面的示例。

### <a name="c-win32"></a>C++ Win32

[C++Win32 示例](https://github.com/marb2000/XamlIslands/tree/master/19H1_Insider_Samples/CppWin32App_With_Island)。 此示例演示如何承载 UWP 用户控件中未打包的完整实现C++Win32 应用程序 （即，没有内置在 MSIX 包应用程序）。

### <a name="wpf-and-windows-forms"></a>WPF 和 Windows 窗体

[WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) Windows 社区工具包中的控件充当一个关于使用 UWP WPF 和 Windows 窗体应用程序中托管 API 参考示例。 在以下位置提供了源代码：

  * 该控件的 WPF 版本[转到此处](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Wpf.UI.XamlHost)。 WPF 版本派生[ **System.Windows.Interop.HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)。

  * Windows 窗体控件，新版[转到此处](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Forms.UI.XamlHost)。 Windows 窗体版本派生[ **System.Windows.Forms.Control**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)。

## <a name="host-uwp-xaml-controls"></a>主机 UWP XAML 控件

以下是使用托管 API UWP XAML 承载你的应用程序中的 UWP 控件的主要步骤。

1. 你的应用程序创建的任何之前初始化当前线程的 UWP XAML 框架[ **Windows.UI.Xaml.UIElement** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)对象，它将在承载[ **DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)。

    * 如果应用程序创建**DesktopWindowXamlSource**对象创建的任何之前**Windows.UI.Xaml.UIElement**对象，在实例化时，此框架将为您初始化**DesktopWindowXamlSource**对象。 在此方案中，不需要添加您自己来初始化框架的任何代码。

    * 但是，如果应用程序创建**Windows.UI.Xaml.UIElement**对象创建之前**DesktopWindowXamlSource**对象，它将承载它们，你的应用程序必须调用静态[**WindowsXamlManager.InitializeForCurrentThread** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread)方法来显式初始化之前的 UWP XAML framework **Windows.UI.Xaml.UIElement**对象实例化。 你的应用程序通常应调用此方法时，承载父 UI 元素**DesktopWindowXamlSource**实例化。

    ```cppwinrt
    Windows::UI::Xaml::Hosting::WindowsXamlManager windowsXamlManager =
        Windows::UI::Xaml::Hosting::WindowsXamlManager::InitializeForCurrentThread();
    ```

    ```csharp
    global::Windows.UI.Xaml.Hosting.WindowsXamlManager windowsXamlManager =
        global::Windows.UI.Xaml.Hosting.WindowsXamlManager.InitializeForCurrentThread();
    ```

    > [!NOTE]
    > 此方法返回[ **WindowsXamlManager** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager)对象，其中包含对 UWP XAML 框架的引用。 您可以创建任意多个**WindowsXamlManager**对象根据需要在给定的线程上。 但是，因为每个对象保留对 UWP XAML 框架的引用，您应释放的对象，以确保最终发布 XAML 资源。

2. 创建[ **DesktopWindowXamlSource** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)对象，并将其附加到与窗口句柄关联应用程序中的父 UI 元素。

    若要执行此操作，您将需要按照以下步骤：

    1. 创建**DesktopWindowXamlSource**对象，并将其转换为**IDesktopWindowXamlSourceNative**或**IDesktopWindowXamlSourceNative2** COM 接口。

    2. 调用**AttachToWindow**方法**IDesktopWindowXamlSourceNative**或**IDesktopWindowXamlSourceNative2**接口，并传入的窗口句柄在应用程序中的父 UI 元素。

    3. 设置中包含的内部子窗口的初始大小**DesktopWindowXamlSource**。 默认情况下，此内部子窗口设置为宽度和高度均为 0。 如果未设置窗口，您将添加到任何 UWP 控件的大小**DesktopWindowXamlSource**不会显示。 若要访问在内部子窗口**DesktopWindowXamlSource**，使用**WindowHandle**属性**IDesktopWindowXamlSourceNative**或**IDesktopWindowXamlSourceNative2**接口。 下面的示例使用[SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos)函数设置窗口的大小。

    下面是一些代码示例演示此过程。

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

3. 设置**Windows.UI.Xaml.UIElement**你想要为托管[**内容**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content)的属性在**DesktopWindowXamlSource**对象。 下面的示例设置[ **Windows.UI.Xaml.Controls.Grid** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid)名为```myGrid```到**内容**属性。

   ```cppwinrt
   desktopWindowXamlSource.Content(myGrid);
   ```

   ```csharp
   desktopWindowXamlSource.Content = myGrid;
   ```

有关演示这些任务的工作示例应用程序上下文中的完整示例，请参阅以下的代码文件：

  * **C++Win32:** 请参阅[XamlBridge.cpp](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island/SampleCppApp/XamlBridge.cpp)中的文件[ C++ Win32 示例](https://github.com/marb2000/XamlIslands/tree/master/19H1_Insider_Samples/CppWin32App_With_Island)。

  * **WPF:** 请参阅[WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.cs)并[WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHost.cs) Windows 社区工具包中的文件。  

  * **Windows 窗体：** 请参阅[WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.cs)并[WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHost.cs) Windows 社区工具包中的文件。

## <a name="handle-keyboard-input-and-focus-navigation"></a>处理键盘输入和焦点导航

有几种方法，您的应用程序需要执行的操作时它可以承载 XAML 群岛正确处理键盘输入和焦点导航。

### <a name="keyboard-input"></a>键盘输入

若要正确处理键盘输入的每个 XAML 岛，你的应用程序必须将传递所有 Windows 消息给 UWP XAML 框架，以便可以正确处理特定消息。 若要执行此操作，在你的应用程序可以访问的消息循环中的某处中强制转换**DesktopWindowXamlSource**对象对每个 XAML 岛**IDesktopWindowXamlSourceNative2** COM 接口。 然后，调用**PreTranslateMessage**此接口并传入当前消息的方法。

  * 一个C++Win32 应用程序可以调用**PreTranslateMessage**直接在其主消息循环中。 有关示例，请参阅[SampleApp.cpp](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island/SampleCppApp/SampleApp.cpp#L61)代码文件中的[ C++ Win32 示例](https://github.com/marb2000/XamlIslands/tree/master/19H1_Insider_Samples/CppWin32App_With_Island)。

  * WPF 应用程序可以调用**PreTranslateMessage**的事件处理程序从[ **ComponentDispatcher.ThreadFilterMessage** ](https://docs.microsoft.com/dotnet/api/system.windows.interop.componentdispatcher.threadfiltermessage?view=netframework-4.7.2)事件。 有关示例，请参阅[WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs#L177) Windows 社区工具包中的文件。

  * Windows 窗体应用程序可以调用**PreTranslateMessage**中的替代[ **Control.PreprocessMessage** ](https://docs.microsoft.com/en-us/dotnet/api/system.windows.forms.control.preprocessmessage?view=netframework-4.7.2)方法。 有关示例，请参阅[WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs#L100) Windows 社区工具包中的文件。

### <a name="keyboard-focus-navigation"></a>键盘焦点导航

当用户导航中使用键盘对应用程序的 UI 元素通过 (例如，通过按**选项卡**或方向/箭头键)，你将需要以编程方式将焦点移到和共**DesktopWindowXamlSource**对象。 当用户的键盘导航到达**DesktopWindowXamlSource**，将焦点移到第一个[ **Windows.UI.Xaml.UIElement** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)导航顺序中的对象为你的 UI，继续将焦点移到以下**Windows.UI.Xaml.UIElement**对象作为用户循环浏览元素，然后再移动焦点返回的**DesktopWindowXamlSource**到父 UI 元素。  

托管 API UWP XAML 提供多种类型和成员，以帮助您完成这些任务。

* 键盘导航进入时你**DesktopWindowXamlSource**，则[ **GotFocus** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.gotfocus)引发事件。 处理此事件，并以编程方式将焦点移到第一个托管**Windows.UI.Xaml.UIElement**通过使用[ **NavigateFocus** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.navigatefocus)方法。

* 当用户位于中最后一个可获得焦点的元素在**DesktopWindowXamlSource**并按下**选项卡**键或箭头键时， [ **TakeFocusRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.takefocusrequested)引发事件。 处理此事件，并以编程方式将焦点移到主机应用程序中下一步获得焦点的元素。 例如，在一个 WPF 应用程序，其中**DesktopWindowXamlSource**中托管[ **System.Windows.Interop.HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)，可以使用[ **MoveFocus** ](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.movefocus)方法可以将焦点转移到主机应用程序中下一步获得焦点的元素。

有关演示如何执行此操作的工作示例应用程序上下文中的示例，请参阅以下的代码文件：

  * **WPF:** 请参阅[WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs) Windows 社区工具包中的文件。  

  * **Windows 窗体：** 请参阅[WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs) Windows 社区工具包中的文件。

## <a name="handle-layout-changes"></a>句柄的布局更改

当用户更改父 UI 元素的大小时，你将需要处理任何必要的布局更改，以确保按预期显示在 UWP 控件。 以下是一些要考虑的重要方案。

* 在C++Win32 应用程序，当你的应用程序处理的 WM_SIZE 消息，它可以通过使用重新定位承载的 XAML 岛[SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos)函数。 有关示例，请参阅[SampleApp.cpp](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island/SampleCppApp/SampleApp.cpp#L191)代码文件中的[ C++ Win32 示例](https://github.com/marb2000/XamlIslands/tree/master/19H1_Insider_Samples/CppWin32App_With_Island)。

* 当父 UI 元素需要获得适合所需的矩形区域的大小**Windows.UI.Xaml.UIElement**承载上的**DesktopWindowXamlSource**，调用[**度量值**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.measure)方法**Windows.UI.Xaml.UIElement**。 例如：

    * 在 WPF 应用程序可能会执行此项从[ **MeasureOverride** ](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.measureoverride)方法[ **HwndHost** ](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)承载**DesktopWindowXamlSource**。 有关示例，请参阅[WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) Windows 社区工具包中的文件。

    * 在 Windows 窗体应用程序中可能会执行此项从[ **GetPreferredSize** ](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.getpreferredsize)方法[**控制**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)承载**DesktopWindowXamlSource**。 有关示例，请参阅[WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) Windows 社区工具包中的文件。

* 当父 UI 元素的大小更改时，调用[**排列**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.arrange)根方法**Windows.UI.Xaml.UIElement**承载上**DesktopWindowXamlSource**。 例如：

    * 在 WPF 应用程序可能会执行此项从[ **ArrangeOverride** ](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.arrangeoverride)方法[ **HwndHost** ](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)承载对象**DesktopWindowXamlSource**。 有关示例，请参阅[WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) Windows 社区工具包中的文件。

    * 在 Windows 窗体应用程序中您可能会执行此操作的处理程序从[ **SizeChanged** ](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.sizechanged)的事件[**控制**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)承载**DesktopWindowXamlSource**。 有关示例，请参阅[WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) Windows 社区工具包中的文件。

## <a name="handle-dpi-changes"></a>处理 DPI 更改

（例如，当用户使用不同的屏幕 DPI 的监视器之间拖动窗口），UWP XAML 框架会自动处理托管 UWP 控件的 DPI 的更改。 为获得最佳体验，我们建议在 Windows 窗体，WPF 中，或C++Win32 应用程序配置为按监视器 DPI 感知。

若要配置为按监视器 DPI 感知应用程序，添加[-并行程序集清单](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests)到你的项目和集```<dpiAwareness>```元素```PerMonitorV2```。 有关此值的详细信息，请参阅的说明[ **DPI_AWARENESS_CONTEXT_PER_MONITOR_AWARE_V2**](https://docs.microsoft.com/windows/desktop/hidpi/dpi-awareness-context)。

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

## <a name="host-custom-uwp-xaml-controls"></a>主机自定义 UWP XAML 控件

> [!IMPORTANT]
> 若要托管的自定义的 UWP XAML 控件，必须具有该控件的源代码，因此可以对其在你的应用程序进行编译。

如果你想要承载自定义的 UWP XAML 控件 （定义自己的控件或控件提供的第三方），则必须执行以下额外任务中所述的过程除了[上一节](#host-uwp-xaml-controls)。

1. 定义派生的自定义类型[ **Windows.UI.Xaml.Application** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application)也能实现[ **IXamlMetadataProvider**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.markup.ixamlmetadataprovider)。 此类型的自定义在当前目录中的程序集的应用程序的 UWP XAML 类型的元数据加载到充当根元数据提供程序。

2. 调用[ **GetXamlType** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.markup.ixamlmetadataprovider.getxamltype)根元数据提供程序时分配 UWP XAML 控件的类型名称的方法 （这可能在代码中在运行时分配，或您可以选择启用此选项以将分配的 Visual Studio 属性窗口中）。

    有关示例，请参阅以下的代码文件：
      * **C++Win32:** 请参阅[XamlApplication.cpp](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island/Microsoft.UI.Xaml.Markup/XamlApplication.cpp)代码文件中的[ C++ Win32 示例](https://github.com/marb2000/XamlIslands/tree/master/19H1_Insider_Samples/CppWin32App_With_Island)。

      * **WPF 和 Windows 窗体**:请参阅[XamlApplication.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Win32.UI.XamlHost/XamlApplication.cs)并[UWPTypeFactory.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Win32.UI.XamlHost/UWPTypeFactory.cs)代码在 Windows 社区工具包中的文件。 这些文件是共享的实现的一部分**WindowsXamlHost** WPF 和 Windows 窗体，可帮助演示了如何使用托管在这些类型的应用程序中的 API UWP XAML 类。

3. 将自定义的 UWP XAML 控件的源代码集成到主机应用程序解决方案生成自定义控件，然后在你的应用程序中使用它。 对于 WPF 或 Windows 窗体应用程序的说明，请参阅[这些说明](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost#add-a-custom-uwp-control)。 例如对于C++Win32 应用程序，请参阅[Microsoft.UI.Xaml.Markup](https://github.com/marb2000/XamlIslands/tree/master/19H1_Insider_Samples/CppWin32App_With_Island/Microsoft.UI.Xaml.Markup)并[MyApp](https://github.com/marb2000/XamlIslands/tree/master/19H1_Insider_Samples/CppWin32App_With_Island/MyApp)中的项目[ C++ Win32 示例](https://github.com/marb2000/XamlIslands/tree/master/19H1_Insider_Samples/CppWin32App_With_Island)。

## <a name="troubleshooting"></a>疑难解答

### <a name="error-using-uwp-xaml-hosting-api-in-a-uwp-app"></a>使用 UWP XAML 的 UWP 应用中托管 API 时出错

| 问题 | 分辨率 |
|-------|------------|
| 您的应用程序接收**COMException**并显示以下消息："不能激活 DesktopWindowXamlSource。 此类型不能使用 UWP 应用中。" 或者"不能激活 WindowsXamlManager。 此类型不能使用 UWP 应用中。" | 此错误表示尝试使用托管 API UWP XAML (具体而言，要实例化[ **DesktopWindowXamlSource** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)或[ **WindowsXamlManager** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager)类型) 中的 UWP 应用。 托管 API UWP XAML 只是在非 UWP 桌面的应用程序，如 WPF、 Windows 窗体中使用和C++Win32 应用程序。 |

### <a name="error-attaching-to-a-window-on-a-different-thread"></a>连接到另一个线程上窗口时出错

| 问题 | 分辨率 |
|-------|------------|
| 您的应用程序接收**COMException**并显示以下消息："AttachToWindow 方法失败，因为不同的线程上创建指定的 HWND。" | 此错误指示您的应用程序调用**IDesktopWindowXamlSourceNative::AttachToWindow**方法并将其传递不同的线程创建了窗口的 HWND。 必须通过此方法在调用该方法的代码所在的线程创建一个窗口的 HWND。 |

### <a name="error-attaching-to-a-window-on-a-different-top-level-window"></a>连接到在不同的顶级窗口的窗口时出错

| 问题 | 分辨率 |
|-------|------------|
| 您的应用程序接收**COMException**并显示以下消息："AttachToWindow 方法失败，因为指定的 HWND 源自不同的顶级窗口之前已传递给 AttachToWindow 同一线程的 HWND。" | 此错误指示您的应用程序调用**IDesktopWindowXamlSourceNative::AttachToWindow**方法并将其传递源自不同的顶级窗口中指定的窗口的窗口的 HWND此方法在同一线程上以前调用。</p></p>应用程序应调用后**IDesktopWindowXamlSourceNative::AttachToWindow**在特定线程上所有其他[ **DesktopWindowXamlSource** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)上相同的对象只能将线程附加到相同的顶级窗口首次调用中传递的后代的 windows **IDesktopWindowXamlSourceNative::AttachToWindow**。 当所有**DesktopWindowXamlSource**对象已关闭的特定线程，下一步**DesktopWindowXamlSource**就可以随意将再次附加到任何窗口。</p></p>若要解决此问题，请关闭所有**DesktopWindowXamlSource**对象，此线程绑定到其他顶级窗口，或为此创建一个新线程**DesktopWindowXamlSource**。 |

## <a name="related-topics"></a>相关主题

* [桌面应用程序中的 UWP 控件](xaml-islands.md)
* [C++Win32 XAML 群岛示例](https://github.com/marb2000/XamlIslands/tree/master/19H1_Insider_Samples/CppWin32App_With_Island)
