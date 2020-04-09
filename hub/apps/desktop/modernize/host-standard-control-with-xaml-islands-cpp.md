---
description: 本文演示如何使用 XAML 托管 API 在 C++ Win32 应用中托管标准 UWP 控件。
title: 使用 XAML 岛在 C++ Win32 应用中托管标准 UWP 控件
ms.date: 03/23/2020
ms.topic: article
keywords: windows 10, uwp, cpp, win32, xaml 岛, 包装控件, 标准控件
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 08308c7bca3cd7f39b08c836e43d791a3fda048f
ms.sourcegitcommit: c660def841abc742600fbcf6ed98e1f4f7beb8cc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/24/2020
ms.locfileid: "80226271"
---
# <a name="host-a-standard-uwp-control-in-a-c-win32-app"></a>在 C++ Win32 应用中托管标准 UWP 控件

本文演示如何使用 [UWP XAML 托管 API](using-the-xaml-hosting-api.md) 在新 C++ Win32 应用中托管标准 UWP 控件（即，由 Windows SDK 提供的控件）。 此代码基于[简单的 XAML 岛示例](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Standalone_Samples/CppWinRT_Basic_Win32App)，本节讨论了代码的一些最重要的部分。 如果你有现有的 C++ Win32 应用项目，则可以为你的项目使用这些步骤和代码示例。

> [!NOTE]
> 本文中演示的方案不支持直接编辑应用中托管的 UWP 控件的 XAML 标记。 此方案限制你通过代码修改托管 UWP 控件的外观和行为。 有关使你可以在托管 UWP 控件时直接编辑 XAML 标记的说明，请参阅[在C++ Win32 应用中托管自定义 UWP 控件](host-custom-control-with-xaml-islands-cpp.md)。

## <a name="create-a-desktop-application-project"></a>创建桌面应用程序项目

1. 在安装了 Windows 10 版本 1903 SDK（版本10.0.18362）或更高版本的 Visual Studio 2019 中，创建一个新“Windows 桌面应用程序”  项目，并将其命名为“MyDesktopWin32App”  。 此项目类型在“C++”  、“Windows”  和“桌面”  项目筛选器下提供。

2. 在“解决方案资源管理器”  中，右键单击解决方案节点，单击“重定向解决方案”  ，选择“10.0.18362.0”  或更高版本的 SDK，然后单击“确定”  。

3. 在项目中安装 [Microsoft.Windows.CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) NuGet 包，以启用对 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis) 的支持：

    1. 在“解决方案资源管理器”  中，右键单击你的项目并选择“管理 NuGet 包”  。
    2. 选择“浏览”  选项卡，搜索 [Microsoft.Windows.CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) 包，并安装此包的最新版本。

    > [!NOTE]
    > 对于新项目，你可以选择安装 [C++/WinRT Visual Studio 扩展 (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) 并使用该扩展中包含的 C++/WinRT 项目模板之一。 有关更多详细信息，请参阅[此文](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

4. 安装 [Microsoft.Toolkit.Win32.UI.SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) NuGet 包：

    1. 在“NuGet 程序包管理器”窗口中，确保已选中“包括预发行版”   。
    2. 选择“浏览”  选项卡，搜索“Microsoft.Toolkit.Win32.UI.SDK”  包，并安装此包的版本 v6.0.0（或更高版本）。 此包提供了多个版本和运行时资产，可使 XAML 岛在你的应用中正常工作。

5. 在[应用程序清单](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests)中设置 `maxVersionTested` 值，以指定应用程序与 Windows 10 版本 1903 或更高版本兼容。

    1. 如果项目中还没有应用程序清单，请将新的 XML 文件添加到项目中，并将其命名为“app.config”  。
    2. 在应用程序清单中，包括下面的示例中所示的“compatibility”  元素和子元素。 将“maxVersionTested”  元素的“Id”  属性替换为要面向的 Windows 10 的版本号（此值必须为 Windows 10 版本 1903 或更高版本）。

        ```xml
        <?xml version="1.0" encoding="UTF-8"?>
        <assembly xmlns="urn:schemas-microsoft-com:asm.v1" manifestVersion="1.0">
            <compatibility xmlns="urn:schemas-microsoft-com:compatibility.v1">
                <application>
                    <!-- Windows 10 -->
                    <maxversiontested Id="10.0.18362.0"/>
                    <supportedOS Id="{8e0f7a12-bfb3-4fe8-b9a5-48fd50a15a9a}" />
                </application>
            </compatibility>
        </assembly>
        ```

## <a name="use-the-xaml-hosting-api-to-host-a-uwp-control"></a>使用 XAML 托管 API 托管 UWP 控件

使用 XAML 托管 API 托管 UWP 控件的基本过程遵循以下常规步骤：

1. 在应用创建它将托管的任何 [Windows.UI.Xaml.UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) 对象之前，为当前线程初始化 UWP XAML 框架。 有多种方法可以执行此操作，具体取决于你计划创建的将托管控件的 [DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) 对象。

    * 如果你的应用程序在创建将托管的任何“Windows.UI.Xaml.UIElement”  对象之前创建“DesktopWindowXamlSource”  对象，则在实例化“DesktopWindowXamlSource”  对象时，会为你初始化此框架。 在这种情况下，你无需添加自己的任何代码来初始化该框架。

    * 但是，如果你的应用程序先创建“Windows.UI.Xaml.UIElement”  对象，然后创建将托管这些对象的“DesktopWindowXamlSource”  对象，则应用程序必须调用静态 [WindowsXamlManager.InitializeForCurrentThread](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) 方法来显式初始化 UWP XAML 框架，然后才能实例化“Windows.UI.Xaml.UIElement”  对象。 在实例化托管“DesktopWindowXamlSource”  的父 UI 元素时，应用程序通常应调用此方法。

    > [!NOTE]
    > 此方法返回 [WindowsXamlManager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) 对象，该对象包含对 UWP XAML 框架的引用。 可以根据需要在给定线程上创建任意数量的“WindowsXamlManager”  对象。 但是，因为每个对象都包含对 UWP XAML 框架的引用，所以你应该释放这些对象以确保最终释放 XAML 资源。

2. 创建 [DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) 对象并将其附加到应用程序中与窗口句柄关联的父 UI 元素。

    为此，你需要执行以下步骤：

    1. 创建一个“DesktopWindowXamlSource”  对象，并将其转换为“IDesktopWindowXamlSourceNative”  或“IDesktopWindowXamlSourceNative2”  COM 接口。
        > [!NOTE]
        > 这些接口是在 Windows SDK 中的“windows.ui.xaml.hosting.desktopwindowxamlsource.h”  头文件中声明的。 默认情况下，此文件位于 %programfiles(x86)%\Windows Kits\10\Include\\<build number\>\um。

    2. 调用 IDesktopWindowXamlSourceNative  或 IDesktopWindowXamlSourceNative2  接口的 AttachToWindow  方法，并传入应用程序中父 UI 元素的窗口句柄。

    3. 设置“DesktopWindowXamlSource”  中包含的内部子窗口的初始大小。 默认情况下，此内部子窗口的宽度和高度均设置为 0。 如果未设置窗口大小，则添加到 DesktopWindowXamlSource  的任何 UWP 控件都将不可见。 若要访问“DesktopWindowXamlSource”  中的内部子窗口，请使用“IDesktopWindowXamlSourceNative”  或“IDesktopWindowXamlSourceNative2”  接口的“WindowHandle”  属性。

3. 最后，分配要托管到“DesktopWindowXamlSource”  对象的[Content](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content)属性的“Windows.UI.Xaml.UIElement”  。

以下步骤和代码示例演示如何实现上述过程：

1. 在项目“源文件”  文件夹中，打开默认的“MyDesktopWin32App.com”  文件。 删除文件的全部内容并添加以下 `include` 和 `using` 语句。 除了标准 C++ 和 UWP 标头和命名空间，这些语句还包括特定于 XAML 岛的几个项。

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

3. 在上一部分之后复制以下代码。 此代码为应用定义 WinMain  函数。 此函数初始化一个基本窗口，并使用 XAML 托管 API 在窗口中托管简单的 UWP TextBlock  控件。

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

4. 在上一部分之后复制以下代码。 此代码定义窗口的[窗口过程](https://docs.microsoft.com/windows/win32/learnwin32/writing-the-window-procedure)。

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

5. 保存代码文件，然后生成并运行应用。 确认在应用窗口中看到 UWP TextBlock  控件。
    > [!NOTE]
    > 你可能会看到几个生成警告，包括 `warning C4002:  too many arguments for function-like macro invocation 'GetCurrentTime'` 和 `manifest authoring warning 81010002: Unrecognized Element "maxversiontested" in namespace "urn:schemas-microsoft-com:compatibility.v1"`。 这些警告是当前工具和 NuGet 包的已知问题，可将其忽略。

有关演示这些任务的完整示例，请参阅以下代码文件：

* **C++ Win32：**
  * 请参阅 [HelloWindowsDesktop.cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Standalone_Samples/CppWinRT_Basic_Win32App/Win32DesktopApp/HelloWindowsDesktop.cpp) 文件。
  * 请参阅 [XamlBridge.cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/XamlBridge.cpp) 文件。
* **WPF：** 请参阅 Windows 社区工具包中的 [WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.cs) 和 [WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHost.cs) 文件。  
* **Windows 窗体：** 请参阅 Windows 社区工具包中的 [WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.cs) 和 [WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHost.cs) 文件。

## <a name="package-the-app"></a>打包应用

可以选择在 [MSIX 包](https://docs.microsoft.com/windows/msix)中打包应用以供部署。 MSIX 是适用于 Windows 的新式应用打包技术，并且基于 MSI、.appx、App-V 和 ClickOnce 安装技术的组合。

下面的说明介绍了如何在 Visual Studio 2019 中使用 [Windows 应用程序打包项目](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)将解决方案中的所有组件打包到 MSIX 包。 只有在需要将应用打包到 MSIX 包时，才需要使用这些步骤。

> [!NOTE]
> 如果选择不在 [MSIX 包](https://docs.microsoft.com/windows/msix)中打包应用程序以供部署，则运行应用的计算机必须安装有 [Visual C++ 运行时](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads)。

1. 向解决方案中添加一个新的 [Windows 应用程序打包项目](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)。 创建项目时，针对“目标版本”和“最低版本”选择“Windows 10 版本 1903 (10.0；版本 18362)”    。

2. 在打包项目中，右键单击“应用程序”节点，然后选择“添加引用”   。 在项目列表中，选择解决方案中的 C++/Win32 桌面应用程序，然后单击“确认”  。

3. 生成并运行打包项目。 确认应用运行并按预期显示 UWP 控件。

## <a name="next-steps"></a>后续步骤

本文中的代码示例可帮助你熟悉在 C++ Win32 应用中托管标准 UWP 控件的基本方案。 以下部分介绍应用程序可能需要支持的其他方案。

### <a name="host-a-custom-uwp-control"></a>托管自定义 UWP 控件

在许多情况下，你可能需要托管一个自定义 UWP XAML 控件，其中包含多个协同工作的单独控件。 在 C++ Win32 应用程序中托管自定义 UWP 控件（你自己定义的控件或第三方提供的控件）的过程比托管标准控件更复杂，需要其他代码。

有关完整的演练，请参阅[使用 XAML 托管 API 在 C++ Win32 应用中托管自定义 UWP 控件](host-custom-control-with-xaml-islands-cpp.md)。

### <a name="advanced-scenarios"></a>高级方案

许多托管 XAML 岛的桌面应用程序需要处理其他方案以提供流畅的用户体验。 例如，桌面应用程序可能需要处理 XAML 岛中的键盘输入、XAML 岛与其他 UI 元素之间的焦点导航以及布局更改。

有关处理这些方案和指向相关代码示例的指针的详细信息，请参阅 [C++ Win32 应用中 XAML 岛的高级方案](advanced-scenarios-xaml-islands-cpp.md)。

## <a name="related-topics"></a>相关主题

* [在桌面应用中托管 UWP XAML 控件（XAML 岛）](xaml-islands.md)
* [在 C++ Win32 应用中使用 UWP XAML 托管 API](using-the-xaml-hosting-api.md)
* [在 C++ Win32 应用中托管自定义 UWP 控件](host-custom-control-with-xaml-islands-cpp.md)
* [C++ Win32 应用中 XAML 岛的高级方案](advanced-scenarios-xaml-islands-cpp.md)
* [XAML 岛代码示例](https://github.com/microsoft/Xaml-Islands-Samples)
