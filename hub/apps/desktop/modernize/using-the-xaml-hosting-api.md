---
description: 本文介绍如何在桌面C++ Win32 应用程序中托管 UWP XAML UI。
title: 在 C++ Win32 应用中使用 UWP XAML 托管 API
ms.date: 08/20/2019
ms.topic: article
keywords: windows 10、uwp、windows 窗体、wpf、win32、xaml 孤岛
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 9e4fdc8366e26bcd7e106bf070cb42ed2cd1a49f
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/06/2020
ms.locfileid: "75683680"
---
# <a name="using-the-uwp-xaml-hosting-api-in-a-c-win32-app"></a>在 C++ Win32 应用中使用 UWP XAML 托管 API

从 Windows 10 版本1903开始，非 UWP 桌面应用（包括C++ WIN32、WPF 和 Windows 窗体应用）可以使用*UWP XAML 宿主 API*在与窗口句柄（HWND）关联的任何 UI 元素中托管 uwp 控件。 此 API 使非 UWP 桌面应用可以使用仅可通过 UWP 控件获取的最新 Windows 10 UI 功能。 例如，非 UWP 桌面应用程序可使用此 API 来托管使用[熟知设计系统](/windows/uwp/design/fluent-design-system/index)并支持[WINDOWS 墨迹](/windows/uwp/design/input/pen-and-stylus-interactions)的 UWP 控件。

UWP XAML 宿主 API 为一组更广泛的控件提供基础，使开发人员能够将熟知的 UI 引入非 UWP 桌面应用程序。 此功能称为*XAML 孤岛*。 有关此功能的概述，请参阅[在桌面应用中宿主 UWP XAML 控件（XAML 孤岛）](xaml-islands.md)。

> [!NOTE]
> 如果你有关于 XAML 孤岛的反馈，请在[Microsoft 工具包](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues)存储库中创建一个新问题，并在此处留下你的意见。 如果你想要私下提交反馈，你可以将其发送到 XamlIslandsFeedback@microsoft.com。 你的见解和方案对我们至关重要。

## <a name="should-you-use-the-uwp-xaml-hosting-api"></a>是否应使用 UWP XAML 宿主 API？

UWP XAML 宿主 API 提供低级别的基础结构，用于在桌面应用中承载 UWP 控件。 某些类型的桌面应用可以选择使用替代的更方便的 Api 来实现此目标。  

* 如果你有C++ Win32 桌面应用，并且想要在应用中托管 UWP 控件，则必须使用 UWP XAML 宿主 API。 此类应用没有替代项。

* 对于 WPF 和 Windows 窗体应用程序，我们强烈建议你在 Windows 社区工具包中使用[XAML 岛 .net 控件](xaml-islands.md#wpf-and-windows-forms-applications)，而不是直接使用 UWP XAML 托管 API。 这些控件在内部使用 UWP XAML 宿主 API，并实现你在自行使用 UWP XAML 托管 API 时需要自行处理的所有行为，包括键盘导航和布局更改。

因为我们建议只有C++ win32 应用使用 UWP XAML 托管 API，但本文主要介绍 win32 应用的C++说明和示例。 但是，如果您选择，可以在 WPF 中使用 UWP XAML 宿主 API，并 Windows 窗体应用。 本文指向适用于 WPF 的[主机控件](xaml-islands.md#host-controls)和 Windows 社区工具包中 Windows 窗体的相关源代码，以便您可以看到这些控件如何使用 UWP XAML 宿主 API。

## <a name="prerequisites"></a>先决条件

XAML 孤岛需要 Windows 10 版本1903（或更高版本）以及相应的 Windows SDK 版本。 若要在C++ Win32 应用中使用 XAML 孤岛，必须先设置项目。

### <a name="support-for-cwinrt"></a>对/WinRT C++的支持

确保你的项目支持[ C++/WinRT](/windows/uwp/cpp-and-winrt-apis)：

* 对于新项目，可以安装[ C++/WinRT Visual Studio Extension （VSIX）](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) ，并使用该扩展中包含C++的/WinRT 项目模板之一。
* 对于现有项目，可以在项目中安装[CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) NuGet 包。

有关这些选项的更多详细信息，请参阅[此文](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

### <a name="configure-your-project-for-app-deployment"></a>为应用部署配置项目

选择以下选项之一以准备要部署的项目：

* **在 .msix 包中打包你的应用程序**。 在[.msix 包](https://docs.microsoft.com/windows/msix/)中打包应用程序提供了许多部署和运行时权益。
    1. 安装 Windows 10 版本 1903 SDK （版本10.0.18362）或更高版本。
    2. 通过向解决方案添加[Windows 应用程序打包项目](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)并添加对C++/Win32 项目的引用，将应用打包在 .msix 包中。

* **安装 Microsoft 工具包. Win32**。 如果不想在 .msix 包中打包应用程序，可以安装[（版本](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK)v 6.0.0-preview7 或更高版本）。 此包提供了多个生成和运行时资产，使 XAML 孤岛可以在应用中工作。 请确保已选中 "**包括预发行**版" 选项，以便你可以查看此包的最新预览版本。

> [!NOTE]
> 这些说明的早期版本已经将 `maxversiontested` 元素添加到项目中的应用程序清单。 只要你使用上面列出的选项之一，就不再需要将此元素添加到清单中。

### <a name="additional-requirements-for-custom-uwp-controls"></a>自定义 UWP 控件的其他要求

如果承载的是自定义 UWP 控件（例如，包含多个协同工作的 UWP 控件的用户控件），则需要按照[此部分](#host-a-custom-uwp-control)中的其他说明进行操作。 您还必须具有自定义控件的源代码，以便可以使用您的应用程序进行编译。

## <a name="architecture-of-the-api"></a>API 的体系结构

UWP XAML 宿主 API 包含这些主要 Windows 运行时类型和 COM 接口。

|  类型或接口 | 描述 |
|--------------------|-------------|
| [WindowsXamlManager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) | 此类表示 UWP XAML framework。 此类提供单个静态[InitializeForCurrentThread](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread)方法，该方法可初始化桌面应用程序中当前线程上的 UWP XAML 框架。 |
| [DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) | 此类表示你在桌面应用中托管的 UWP XAML 内容的实例。 此类中最重要的成员是[Content](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content)属性。 将此属性分配给要承载的[WINDOWS UI。](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) 此类还具有其他成员，用于将键盘焦点导航入和移出 XAML 孤岛。 |
| IDesktopWindowXamlSourceNative | 此 COM 接口提供**AttachToWindow**方法，可用于将应用中的 XAML 岛附加到父 UI 元素。 每个**DesktopWindowXamlSource**对象都实现此接口。 |
| IDesktopWindowXamlSourceNative2 | 此 COM 接口提供**PreTranslateMessage**方法，该方法可使 UWP XAML 框架正确处理某些 Windows 消息。 每个**DesktopWindowXamlSource**对象都实现此接口。 |

下图说明了在桌面应用中托管的 XAML 岛中对象的层次结构。

* 在基本级别上，是你要在其中托管 XAML 岛的应用程序中的 UI 元素。 此 UI 元素必须具有一个窗口句柄（HWND）。 可在其中承载 XAML 岛的 UI 元素的示例包括C++适用于 Win32 应用的[窗口](https://docs.microsoft.com/windows/desktop/winmsg/about-windows)、[system.windows.interop.hwndhost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)> for WPF apps，以及用于 Windows 窗体应用的 [System.Windows.Forms.Control](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)。

* 下一级别是**DesktopWindowXamlSource**对象。 此对象提供用于托管 XAML 岛的基础结构。 你的代码负责创建此对象并将其附加到父 UI 元素。

* 当您创建**DesktopWindowXamlSource**时，此对象将自动创建一个本机子窗口以承载 UWP 控件。 此本机子窗口主要是从代码中提取出来的，但如果需要，可以访问其句柄（HWND）。

* 最后，最上层是要在桌面应用中托管的 UWP 控件。 这可以是从[Windows](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)派生的任何 uwp 对象，包括 Windows SDK 提供的任何 uwp 控件以及自定义用户控件。

![DesktopWindowXamlSource 体系结构](images/xaml-islands/xaml-hosting-api-rev2.png)

## <a name="related-samples"></a>相关示例

您在代码中使用 UWP XAML 宿主 API 的方式取决于您的应用程序类型、应用程序的设计和其他因素。 为了帮助说明如何在完整应用程序的上下文中使用此 API，本文引用了以下示例中的代码。

### <a name="c-win32"></a>C++Win32

下面的示例演示如何在C++ Win32 应用程序中使用 UWP XAML 宿主 API：

* [简单的 XAML 岛示例](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_SimpleApp)。 此示例演示了在未打包C++的 Win32 应用程序（即，未内置到 .msix 包中的应用程序）中承载 UWP 控件的基本实现。

* [带有自定义控件示例的 XAML 岛](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App)。 此示例演示了在未打包C++的 Win32 应用程序中托管自定义 UWP 控件以及处理其他行为（如键盘输入和焦点导航）的完整实现。 

### <a name="wpf-and-windows-forms"></a>WPF 和 Windows 窗体

Windows 社区工具包中的[WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)控件作为参考示例，用于在 WPF 和 Windows 窗体应用中使用 UWP 宿主 API。 源代码位于以下位置：

* 对于控件的 WPF 版本，请[参阅此处](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Wpf.UI.XamlHost)。 WPF 版本派生自[system.windows.interop.hwndhost>](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)。

* 对于控件的 Windows 窗体版本，请[参阅此处](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Forms.UI.XamlHost)。 Windows 窗体版本派生自[system.object](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)。

## <a name="host-a-standard-uwp-control"></a>承载标准 UWP 控件

本部分将指导你完成使用 UWP XAML 宿主 API 在新C++的 Win32 应用程序中承载标准 UWP 控件（即，Windows SDK 或 WinUI 库提供的控件）的过程。 此代码基于[简单的 XAML 岛示例](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_SimpleApp)，本节讨论了代码的一些最重要的部分。 如果你有现有C++的 Win32 应用项目，则可以修改项目的这些步骤和代码示例。

### <a name="configure-the-project"></a>配置项目

1. 在安装了 Windows 10、版本 1903 SDK （版本10.0.18362）或更高版本的 Visual Studio 2019 中，创建一个新的**Windows 桌面应用程序**项目。 此项目可用于**C++** 、 **Windows**和**桌面**项目筛选器。

2. 在**解决方案资源管理器**中，右键单击解决方案节点，单击 "重**定目标解决方案**"，选择**10.0.18362.0**或更高版本的 SDK 版本，然后单击 **"确定"** 。

3. 安装[CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) NuGet 包：

    1. 在“解决方案资源管理器”中，右键单击你的项目并选择“管理 NuGet 包”。
    2. 选择 "**浏览**" 选项卡，搜索[CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/)包，然后安装此包的最新版本。

4. 安装[Microsoft 工具包. Win32](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) NuGet 包：

    1. 在 " **NuGet 包管理器**" 窗口中，确保选择 "**包括预发行**版"。
    2. 选择 "**浏览**" 选项卡，搜索["6.0.0" 包](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK)，并安装此包的版本 v preview7 （或更高版本）。

### <a name="use-the-xaml-hosting-api-to-host-a-uwp-control"></a>使用 XAML 宿主 API 承载 UWP 控件

使用 XAML 宿主 API 承载 UWP 控件的基本过程遵循以下常规步骤：

1. 在您的应用程序创建它将承载的任何[WINDOWS UI. UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)对象之前，为当前线程初始化 UWP XAML framework。 有多种方法可以执行此操作，具体取决于计划创建将承载控件的[DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)对象的时间。

    * 如果你的应用程序在创建它将**承载的任何** **DesktopWindowXamlSource**对象之前创建了该对象，则在实例化**DesktopWindowXamlSource**对象时，会为你初始化此框架。 在这种情况下，你无需添加自己的任何代码来初始化该框架。

    * 但是，如果您的应用程序在创建将承载这些对象的**DesktopWindowXamlSource**对象之前创建**了这些对象**，则您的应用程序必须调用静态[WindowsXamlManager InitializeForCurrentThread](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread)方法，以便在实例化**WINDOWS**对象之前显式初始化 UWP xaml framework。 在实例化承载**DesktopWindowXamlSource**的父 UI 元素时，应用程序通常应调用此方法。

    > [!NOTE]
    > 此方法返回一个[WindowsXamlManager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager)对象，该对象包含对 UWP XAML 框架的引用。 您可以根据需要在给定线程上创建任意多个**WindowsXamlManager**对象。 但是，因为每个对象都包含对 UWP XAML framework 的引用，所以你应该释放这些对象以确保最终释放 XAML 资源。

2. 创建一个[DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)对象，并将其附加到应用程序中与窗口句柄关联的父 UI 元素。

    为此，您需要执行以下步骤：

    1. 创建一个**DesktopWindowXamlSource**对象，并将其转换为**IDesktopWindowXamlSourceNative**或**IDesktopWindowXamlSourceNative2** COM 接口。
        > [!NOTE]
        > 这些接口是在 Windows SDK 中的**desktopwindowxamlsource**头文件中声明的。 默认情况下，此文件位于% programfiles （x86）% \ Windows Kits\10\Include\\\>\um. < 生成号

    2. 调用**IDesktopWindowXamlSourceNative**或**IDesktopWindowXamlSourceNative2**接口的**AttachToWindow**方法，并传入应用程序中父 UI 元素的窗口句柄。

    3. 设置**DesktopWindowXamlSource**中包含的内部子窗口的初始大小。 默认情况下，此内部子窗口的宽度和高度均设置为0。 如果未设置窗口的大小，则添加到**DesktopWindowXamlSource**的任何 UWP 控件都将不可见。 若要访问**DesktopWindowXamlSource**中的内部子窗口，请使用**IDesktopWindowXamlSourceNative**或**IDesktopWindowXamlSourceNative2**接口的**WindowHandle**属性。

3. 最后，分配要承载到**DesktopWindowXamlSource**对象的[Content](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content)属性中的**Windows。**

以下步骤和代码示例演示如何实现上述过程：

1. 在项目的 "**源文件**" 文件夹中，打开默认的**WindowsProject**文件。 删除文件的全部内容并添加以下 `include` 和 `using` 语句。 除了标准C++和 UWP 标头和命名空间，这些语句还包括特定于 XAML 孤岛的几个项。

    ```cppwinrt
    #include <windows.h>
    #include <stdlib.h>
    #include <string.h>

    #include <winrt/Windows.Foundation.Collections.h>
    #include <winrt/Windows.system.h>
    #include <winrt/windows.ui.xaml.hosting.h>
    #include <windows.ui.xaml.hosting.desktopwindowxamlsource.h>
    #include <winrt/windows.ui.xaml.controls.h>
    #include <winrt/Windows.ui.xaml.media.h>

    using namespace winrt;
    using namespace Windows::UI;
    using namespace Windows::UI::Composition;
    using namespace Windows::UI::Xaml::Hosting;
    using namespace Windows::Foundation::Numerics;
    ```

3. 在上一部分中复制以下代码。 此代码定义应用程序的**WinMain**函数。 此函数初始化一个基本窗口，并使用 XAML 宿主 API 在窗口中承载简单的 UWP **TextBlock**控件。

    ```cppwinrt
    LRESULT CALLBACK WindowProc(HWND, UINT, WPARAM, LPARAM);

    HWND _hWnd;
    HWND _childhWnd;
    HINSTANCE _hInstance;

    int CALLBACK WinMain(_In_ HINSTANCE hInstance, _In_opt_ HINSTANCE hPrevInstance, _In_ LPSTR lpCmdLine, _In_ int nCmdShow)
    {
        _hInstance = hInstance;

        // The main window class name.
        const wchar_t szWindowClass[] = L"Win32DesktopApp";
        WNDCLASSEX windowClass = { };

        windowClass.cbSize = sizeof(WNDCLASSEX);
        windowClass.lpfnWndProc = WindowProc;
        windowClass.hInstance = hInstance;
        windowClass.lpszClassName = szWindowClass;
        windowClass.hbrBackground = (HBRUSH)(COLOR_WINDOW + 1);

        windowClass.hIconSm = LoadIcon(windowClass.hInstance, IDI_APPLICATION);

        if (RegisterClassEx(&windowClass) == NULL)
        {
            MessageBox(NULL, L"Windows registration failed!", L"Error", NULL);
            return 0;
        }

        _hWnd = CreateWindow(
            szWindowClass,
            L"Windows c++ Win32 Desktop App",
            WS_OVERLAPPEDWINDOW | WS_VISIBLE,
            CW_USEDEFAULT, CW_USEDEFAULT, CW_USEDEFAULT, CW_USEDEFAULT,
            NULL,
            NULL,
            hInstance,
            NULL
        );
        if (_hWnd == NULL)
        {
            MessageBox(NULL, L"Call to CreateWindow failed!", L"Error", NULL);
            return 0;
        }


        // Begin XAML Island section.

        // The call to winrt::init_apartment initializes COM; by default, in a multithreaded apartment.
        winrt::init_apartment(apartment_type::single_threaded);

        // Initialize the XAML framework's core window for the current thread.
        WindowsXamlManager winxamlmanager = WindowsXamlManager::InitializeForCurrentThread();

        // This DesktopWindowXamlSource is the object that enables a non-UWP desktop application 
        // to host UWP controls in any UI element that is associated with a window handle (HWND).
        DesktopWindowXamlSource desktopSource;

        // Get handle to the core window.
        auto interop = desktopSource.as<IDesktopWindowXamlSourceNative>();

        // Parent the DesktopWindowXamlSource object to the current window.
        check_hresult(interop->AttachToWindow(_hWnd));

        // This HWND will be the window handler for the XAML Island: A child window that contains XAML.  
        HWND hWndXamlIsland = nullptr;

        // Get the new child window's HWND. 
        interop->get_WindowHandle(&hWndXamlIsland);

        // Update the XAML Island window size because initially it is 0,0.
        SetWindowPos(hWndXamlIsland, 0, 200, 100, 800, 200, SWP_SHOWWINDOW);

        // Create the XAML content.
        Windows::UI::Xaml::Controls::StackPanel xamlContainer;
        xamlContainer.Background(Windows::UI::Xaml::Media::SolidColorBrush{ Windows::UI::Colors::LightGray() });

        Windows::UI::Xaml::Controls::TextBlock tb;
        tb.Text(L"Hello World from Xaml Islands!");
        tb.VerticalAlignment(Windows::UI::Xaml::VerticalAlignment::Center);
        tb.HorizontalAlignment(Windows::UI::Xaml::HorizontalAlignment::Center);
        tb.FontSize(48);

        xamlContainer.Children().Append(tb);
        xamlContainer.UpdateLayout();
        desktopSource.Content(xamlContainer);

        // End XAML Island section.

        ShowWindow(_hWnd, nCmdShow);
        UpdateWindow(_hWnd);

        //Message loop:
        MSG msg = { };
        while (GetMessage(&msg, NULL, 0, 0))
        {
            TranslateMessage(&msg);
            DispatchMessage(&msg);
        }

        return 0;
    }
    ```

4. 在上一部分中复制以下代码。 此代码定义窗口的[窗口过程](https://docs.microsoft.com/windows/win32/learnwin32/writing-the-window-procedure)。

    ```cppwinrt
    LRESULT CALLBACK WindowProc(HWND hWnd, UINT messageCode, WPARAM wParam, LPARAM lParam)
    {
        PAINTSTRUCT ps;
        HDC hdc;
        wchar_t greeting[] = L"Hello World in Win32!";
        RECT rcClient;

        switch (messageCode)
        {
            case WM_PAINT:
                if (hWnd == _hWnd)
                {
                    hdc = BeginPaint(hWnd, &ps);
                    TextOut(hdc, 300, 5, greeting, wcslen(greeting));
                    EndPaint(hWnd, &ps);
                }
                break;
            case WM_DESTROY:
                PostQuitMessage(0);
                break;

            // Create main window
            case WM_CREATE:
                _childhWnd = CreateWindowEx(0, L"ChildWClass", NULL, WS_CHILD | WS_BORDER, 0, 0, 0, 0, hWnd, NULL, _hInstance, NULL);
                return 0;

            // Main window changed size
            case WM_SIZE:
                // Get the dimensions of the main window's client
                // area, and enumerate the child windows. Pass the
                // dimensions to the child windows during enumeration.
                GetClientRect(hWnd, &rcClient);
                MoveWindow(_childhWnd, 200, 200, 400, 500, TRUE);
                ShowWindow(_childhWnd, SW_SHOW);

                return 0;

                // Process other messages.

            default:
                return DefWindowProc(hWnd, messageCode, wParam, lParam);
                break;
        }

        return 0;
    }
    ```

5. 保存代码文件，然后生成并运行应用。 确认在应用窗口中看到 UWP **TextBlock**控件。
    > [!NOTE]
    > 你可能会看到几个生成警告，包括 `warning C4002:  too many arguments for function-like macro invocation 'GetCurrentTime'` 和 `manifest authoring warning 81010002: Unrecognized Element "maxversiontested" in namespace "urn:schemas-microsoft-com:compatibility.v1"`。 这些警告是当前工具和 NuGet 包的已知问题，可将其忽略。

有关演示这些任务的完整示例，请参阅以下代码文件：

* **C++Win32**
  * 请参见[简单的 XAML 岛示例](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_SimpleApp)中的[HelloWindowsDesktop](https://github.com/marb2000/XamlIslands/blob/master/1903_Samples/CppWinRT_Win32_SimpleApp/Win32DesktopApp/HelloWindowsDesktop.cpp)文件。
  * 请参阅[带有自定义控件示例的 XAML 岛](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App)中的[XamlBridge](https://github.com/marb2000/XamlIslands/blob/master/1903_Samples/CppWinRT_Win32_App/SampleCppApp/XamlBridge.cpp)文件。

* **WPF：** 请参阅 Windows 社区工具包中的[WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.cs)和[WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHost.cs)文件。  

* **Windows 窗体：** 请参阅 Windows 社区工具包中的[WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.cs)和[WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHost.cs)文件。

## <a name="host-a-custom-uwp-control"></a>承载自定义 UWP 控件

在C++ Win32 应用程序中承载自定义 UWP 控件（您自己定义的控件或第三方提供的控件）的过程从[承载标准控件](#host-a-standard-uwp-control)的步骤开始。 但是，承载自定义 UWP 控件需要额外的项目和步骤。

若要承载自定义 UWP 控件，你将需要以下项目和组件：

* **应用程序的项目和源代码**。 请确保已将项目配置为满足托管 XAML 孤岛的[先决条件](#prerequisites)。

* **自定义 UWP 控件**。 你需要承载自定义 UWP 控件的源代码，以便可以将其与你的应用进行编译。 通常，自定义控件在与C++ Win32 项目相同的解决方案中引用的 UWP 类库项目中定义。

* **定义 XamlApplication 对象的 UWP 应用项目**。 您C++的 Win32 项目必须有权访问 Windows 社区工具包提供的 `Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication` 类的实例。 此类型充当根元数据提供程序，用于为应用程序的当前目录中的程序集中的自定义 UWP XAML 类型加载元数据。 执行此操作的建议方法是将**空白应用（通用 Windows）** 项目添加到与C++ Win32 项目相同的解决方案，并修改此项目中的默认 `App` 类。
  > [!NOTE]
  > 你的解决方案只能包含一个定义 `XamlApplication` 对象的项目。 应用中的所有自定义 UWP 控件共享相同的 `XamlApplication` 对象。 定义 `XamlApplication` 对象的项目必须包含对在 XAML 岛中承载 UWP 控件的所有其他 UWP 库和项目的引用。

若要在C++ Win32 应用程序中托管自定义 UWP 控件，请执行下列常规步骤。

1. 在包含C++ Win32 桌面应用程序项目的解决方案中，添加一个**空白应用（通用 Windows）** 项目，并按照[此部分](host-custom-control-with-xaml-islands.md#create-a-xamlapplication-object-in-a-uwp-app-project)的相关 WPF 演练中的详细说明进行配置。

2. 在同一解决方案中，添加包含自定义 UWP XAML 控件（通常是 UWP 类库项目）源代码的项目，并生成项目。

3. 在 UWP 应用项目中，添加对 UWP 类库项目的引用。

4. 在C++ Win32 项目中，添加对你的解决方案中的 uwp 应用项目和 uwp 类库项目的引用。

5. 按照[使用 xaml 宿主 API 来承载 UWP 控件](#use-the-xaml-hosting-api-to-host-a-uwp-control)部分中所述的过程，在应用程序的 XAML 岛中承载自定义控件。 将自定义控件的实例分配给代码中**DesktopWindowXamlSource**对象的[Content](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content)属性。

有关C++ Win32 应用程序的完整示例，请参阅[使用自定义控件的 XAML 岛](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App)中的以下项目示例：

* [SampleUserControl](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App/SampleUserControl)：此项目实现一个名为 `MyUserControl` 的自定义 UWP XAML 控件，该控件包含文本框、多个按钮和组合框。
* [MyApp](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App/MyApp)：这是一个 UWP 应用项目，其中包含上述更改。
* [SampleCppApp](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App/SampleCppApp)：这是在C++ XAML 岛中承载自定义 UWP XAML 控件的 Win32 应用程序项目。

## <a name="handle-keyboard-layout-and-dpi"></a>处理键盘、布局和 DPI

以下各节提供了有关如何处理键盘和布局方案的代码示例的指南和链接。

* [键盘输入](#keyboard-input)
* [键盘焦点导航](#keyboard-focus-navigation)
* [处理布局更改](#handle-layout-changes)
* [处理 DPI 更改](#handle-dpi-changes)

### <a name="keyboard-input"></a>键盘输入

若要正确处理每个 XAML 岛的键盘输入，应用程序必须将所有 Windows 消息传递给 UWP XAML 框架，以便能够正确处理某些消息。 为此，可以在应用程序中访问消息循环的某个位置，将每个 XAML 岛的**DesktopWindowXamlSource**对象转换为**IDesktopWindowXamlSourceNative2** COM 接口。 然后，调用此接口的**PreTranslateMessage**方法，并传入当前消息。

  * Win32：：应用可以直接在其主消息循环中调用**PreTranslateMessage** 。 **C++** 有关示例，请参阅[ C++ Win32 示例](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App)中的[XamlBridge](https://github.com/marb2000/XamlIslands/blob/master/1903_Samples/CppWinRT_Win32_App/SampleCppApp/XamlBridge.cpp#L6)文件。

  * **WPF：** 应用可以从[ComponentDispatcher](https://docs.microsoft.com/dotnet/api/system.windows.interop.componentdispatcher.threadfiltermessage)事件的事件处理程序调用**PreTranslateMessage** 。 有关示例，请参阅 Windows 社区工具包中的[WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs#L177)文件。

  * **Windows 窗体：** 应用可以通过[system.windows.forms.control.preprocessmessage](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.preprocessmessage)方法的替代调用**PreTranslateMessage** 。 有关示例，请参阅 Windows 社区工具包中的[WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs#L100)文件。

### <a name="keyboard-focus-navigation"></a>键盘焦点导航

当用户使用键盘在应用程序中浏览 UI 元素（例如，按**tab**或方向/箭头键）时，需要以编程方式将焦点移入和移出**DesktopWindowXamlSource**对象。 当用户的键盘导航到达**DesktopWindowXamlSource**时，将焦点移到你的 UI  的导航顺序中的第一个 [Windows.UI.Xaml.UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) 中。当用户循环访问元素，然后**Windows.UI.Xaml.UIElement**将焦点移出**DesktopWindowXamlSource并**移入父 UI 元素时，则为  

UWP XAML 宿主 API 提供若干类型和成员，以帮助你完成这些任务。

* 当键盘导航进入**DesktopWindowXamlSource**时，将引发[GotFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.gotfocus)事件。 处理此事件，并使用[NavigateFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.navigatefocus)方法以编程方式将焦点移到第一个承载的**Windows。**

* 当用户在**DesktopWindowXamlSource**中的最后一个可获得焦点的元素上并按**Tab**键或箭头键时，将引发[TakeFocusRequested](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.takefocusrequested)事件。 处理此事件并以编程方式将焦点移到主机应用程序中的下一个可获得焦点的元素。 例如，在 WPF 应用程序中， **DesktopWindowXamlSource**托管在[system.windows.interop.hwndhost>](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)中，可以使用[system.windows.frameworkelement.movefocus](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.movefocus)方法将焦点转移到主机应用程序中下一个可设定焦点的元素。

有关演示如何在运行的示例应用程序的上下文中执行此操作的示例，请参阅以下代码文件：

  * /Win32：请参阅[ C++ Win32 示例](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App)中的[XamlBridge](https://github.com/marb2000/XamlIslands/blob/master/1903_Samples/CppWinRT_Win32_App/SampleCppApp/XamlBridge.cpp)文件。  **C++**

  * **WPF：** 请参阅 Windows 社区工具包中的[WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs)文件。  

  * **Windows 窗体：** 请参阅 Windows 社区工具包中的[WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs)文件。

### <a name="handle-layout-changes"></a>处理布局更改

当用户更改父 UI 元素的大小时，需要处理任何必要的布局更改，以确保 UWP 控件按预期显示。 下面是一些需要考虑的重要方案。

* 在C++ Win32 应用程序中，当应用程序处理 WM_SIZE 消息时，它可以使用[SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos)函数重新定位托管的 XAML 岛。 有关示例，请参阅[ C++ Win32 示例](https://github.com/marb2000/XamlIslands/tree/master/19H1_Insider_Samples/CppWin32App_With_Island)中的[sampleapp.exe](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island/SampleCppApp/SampleApp.cpp#L191)代码文件。

* 当父 UI 元素需要获取适合你在**DesktopWindowXamlSource**上**承载的所**需的矩形区域的大小时，请调用**Windows**的[Measure](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.measure)方法，然后再调用方法。 例如：

    * 在 WPF 应用程序中，你可以从承载**DesktopWindowXamlSource**的[system.windows.interop.hwndhost>](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)的[system.windows.frameworkelement.measureoverride](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.measureoverride)方法执行此操作。 有关示例，请参阅 Windows 社区工具包中的[WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs)文件。

    * 在 Windows 窗体应用程序中，你可以从承载**DesktopWindowXamlSource**的[控件](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)的[GetPreferredSize](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.getpreferredsize)方法执行此操作。 有关示例，请参阅 Windows 社区工具包中的[WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs)文件。

* 父 UI 元素的大小发生更改时，调用在**DesktopWindowXamlSource**上托管的根**Windows Ui**的 "[排列](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.arrange)" 方法。 例如：

    * 在 WPF 应用程序中，你可以从承载**DesktopWindowXamlSource**的[System.windows.interop.hwndhost>](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)对象的[system.windows.frameworkelement.arrangeoverride](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.arrangeoverride)方法执行此操作。 有关示例，请参阅 Windows 社区工具包中的[WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs)文件。

    * 在 Windows 窗体应用程序中，你可以从承载**DesktopWindowXamlSource**的[控件](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)的[SizeChanged](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.sizechanged)事件的处理程序中执行此操作。 有关示例，请参阅 Windows 社区工具包中的[WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs)文件。

### <a name="handle-dpi-changes"></a>处理 DPI 更改

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

## <a name="troubleshooting"></a>“疑难解答”

### <a name="error-using-uwp-xaml-hosting-api-in-a-uwp-app"></a>在 UWP 应用中使用 UWP XAML 托管 API 时出错

| 问题 | 分辨率 |
|-------|------------|
| 应用收到**COMException** ，其中包含以下消息： "无法激活 DesktopWindowXamlSource。 此类型不能在 UWP 应用中使用。 或 "无法激活 WindowsXamlManager。 此类型不能在 UWP 应用中使用。 | 此错误指示你正在尝试使用 uwp 应用中的 UWP XAML 宿主 API （具体而言，是尝试实例化[DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)或[WindowsXamlManager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager)类型）。 UWP XAML 宿主 API 仅适用于非 UWP 桌面应用，如 WPF、Windows 窗体和C++ Win32 应用程序。 |

### <a name="error-trying-to-use-the-windowsxamlmanager-or-desktopwindowxamlsource-types"></a>尝试使用 WindowsXamlManager 或 DesktopWindowXamlSource 类型时出错

| 问题 | 分辨率 |
|-------|------------|
| 您的应用程序收到一个异常，其中包含以下消息： "WindowsXamlManager 和 DesktopWindowXamlSource 支持面向 Windows 版本10.0.18226.0 和更高版本的应用程序。 请检查应用程序清单或包清单，确保 MaxTestedVersion 属性已更新。 " | 此错误表示你的应用程序尝试在 UWP XAML 宿主 API 中使用**WindowsXamlManager**或**DesktopWindowXamlSource**类型，但 OS 无法确定此应用是否构建为面向 Windows 10 1903 版或更高版本。 在早期版本的 Windows 10 中，UWP XAML 宿主 API 首先作为预览引入，但仅支持从 Windows 10 版本1903开始。</p></p>若要解决此问题，请为应用程序创建 .msix 包，并从包中运行它，或在项目中安装[NuGet 包。](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) 有关详情，请参阅[本部分](#configure-your-project-for-app-deployment)。 |

### <a name="error-attaching-to-a-window-on-a-different-thread"></a>附加到不同线程的窗口时出错

| 问题 | 分辨率 |
|-------|------------|
| 应用收到**COMException** ，其中包含以下消息： "AttachToWindow 方法失败，因为指定的 HWND 是在另一个线程上创建的。" | 此错误表示你的应用程序调用了**IDesktopWindowXamlSourceNative：： AttachToWindow**方法，并向其传递了在不同线程上创建的窗口的 HWND。 您必须传递此方法，它是在与您从中调用方法的代码相同的线程上创建的窗口的 HWND。 |

### <a name="error-attaching-to-a-window-on-a-different-top-level-window"></a>附加到不同顶级窗口的窗口时出错

| 问题 | 分辨率 |
|-------|------------|
| 你的应用收到**COMException** ，其中包含以下消息： "AttachToWindow 方法失败，因为指定的 HWND 从与以前传递到同一线程上的 ATTACHTOWINDOW 的 HWND 更不同的顶级窗口。" | 此错误表示你的应用程序调用了**IDesktopWindowXamlSourceNative：： AttachToWindow**方法，并向其传递了一个窗口的 HWND，该窗口与你先前在同一线程上对此方法的调用中指定的窗口相比，</p></p>在应用程序调用特定线程上的**AttachToWindow**后，同一线程上的所有其他[DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)对象只能附加到作为首次调用**AttachToWindow**时传递的相同顶级窗口的后代的 windows。 当关闭特定线程的所有**DesktopWindowXamlSource**对象时，接下来的**DesktopWindowXamlSource**可以自由地附加到任何窗口。</p></p>若要解决此问题，请关闭绑定到此线程上其他顶级窗口的所有**DesktopWindowXamlSource**对象，或为此**DesktopWindowXamlSource**创建新线程。 |

## <a name="related-topics"></a>相关主题

* [桌面应用程序中的 UWP 控件](xaml-islands.md)
* [C++Win32 XAML 孤岛示例](https://github.com/marb2000/XamlIslands/tree/master/19H1_Insider_Samples/CppWin32App_With_Island)
