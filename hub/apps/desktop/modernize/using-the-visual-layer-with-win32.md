---
title: 将视觉层与 Win32 结合使用
description: 使用视觉层增强 Win32 桌面应用的 UI。
template: detail.hbs
ms.date: 03/18/2019
ms.topic: article
keywords: UWP, 呈现, 合成, win32
ms.author: jimwalk
author: jwmsft
ms.localizationpriority: medium
ms.openlocfilehash: c9b4ec38b0dd1f6eca3f43cfded74c6292c08100
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "66215191"
---
# <a name="using-the-visual-layer-with-win32"></a>将视觉层与 Win32 结合使用

可以在 Win32 应用中使用 Windows 运行时 Composition API（也称为[视觉层](/windows/uwp/composition/visual-layer)）来创建面向 Windows 10 用户的现代特色体验。

GitHub 上提供了本教程的完整代码：[Win32 HelloComposition 示例](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloComposition)。

需要精确控制其 UI 合成的通用 Windows 应用程序可以访问 [Windows.UI.Composition](/uwp/api/windows.ui.composition) 命名空间，以便对其 UI 的合成和呈现方式进行精细控制。 但是，此 Composition API 并不局限于 UWP 应用。 Win32 桌面应用程序可以利用 UWP 和 Windows 10 中的现代合成系统。

## <a name="prerequisites"></a>必备条件

UWP 宿主 API 的先决条件如下。

- 假设你对使用 Win32 和 UWP 进行应用开发有一定的了解。 有关详细信息，请参阅：
  - [Win32 和 C++ 入门](/windows/desktop/learnwin32/learn-to-program-for-windows)
  - [Windows 10 应用入门](/windows/uwp/get-started/)
  - [增强适用于 Windows 10 的桌面应用程序](/windows/uwp/porting/desktop-to-uwp-enhance)
- Windows 10 版本 1803 或更高版本
- Windows 10 SDK 17134 或更高版本

## <a name="how-to-use-composition-apis-from-a-win32-desktop-application"></a>如何从 Win32 桌面应用程序使用 Composition API

在本教程中，你将创建一个简单的 Win32 C++ 应用，并在其中添加 UWP 合成元素。 重点在于正确配置项目、创建互操作代码，并使用 Windows Composition API 绘制一些简单内容。 完成的应用如下所示。

![正在运行的应用 UI](images/visual-layer-interop/win32-comp-interop-app-ui.png)

## <a name="create-a-c-win32-project-in-visual-studio"></a>在 Visual Studio 中创建 C++ Win32 项目

第一步是在 Visual Studio 中创建 Win32 应用项目。

若要在 C++ 中创建名为 HelloComposition 的新 Win32 应用程序项目： 

1. 打开 Visual Studio 并选择“文件” > “新建” > “项目”。   

    此时会打开“新建项目”对话框。 
1. 在“安装”类别下展开“Visual C++”节点，然后选择“Windows 桌面”。   
1. 选择“Windows 桌面应用程序”模板。 
1. 输入名称 HelloComposition，然后单击“确定”。  

    Visual Studio 将创建该项目，并打开主应用文件的编辑器。

## <a name="configure-the-project-to-use-windows-runtime-apis"></a>将项目配置为使用 Windows 运行时 API

为了在 Win32 应用中使用 Windows 运行时 (WinRT) API，我们将使用 C++/WinRT。 需要配置 Visual Studio 项目才能添加 C++/WinRT 支持。

（有关详细信息，请参阅 [C++/WinRT 入门 - 修改 Windows 桌面应用程序项目以添加 C++/WinRT 支持](/windows/uwp/cpp-and-winrt-apis/get-started.md#modify-a-windows-desktop-application-project-to-add-cwinrt-support)）。

1. 在“项目”菜单中，打开项目属性（HelloComposition 属性），并确保对以下设置使用指定值：  

    - 对于“配置”，请选择“所有配置”。   对于“平台”，请选择“所有平台”。  
    - “配置属性” > “常规” > “Windows SDK 版本” = “10.0.17763.0 或更高版本”    

    ![设置 SDK 版本](images/visual-layer-interop/sdk-version.png)

    - “C/C++” > “语言” > “C++ 语言标准” = “ISO C++ 17 标准(/stf:c++17)”    

    ![设置语言标准](images/visual-layer-interop/language-standard.png)

    - “链接器” > “输入” > “其他依赖项”必须包括“windowsapp.lib”。     如果列表中不包括此项，请添加它。

    ![添加链接器依赖项](images/visual-layer-interop/linker-dependencies.png)

1. 更新预编译的标头

    - 将 `stdafx.h` 和 `stdafx.cpp` 分别重命名为 `pch.h` 和 `pch.cpp`。
    - 将项目属性“C/C++” > “预编译的标头” > “预编译的标头文件”设置为 pch.h。    
    - 在所有文件中找到 `#include "stdafx.h"` 并将其替换为 `#include "pch.h"`。

        （“编辑” > “查找和替换” > “在文件中查找”）   

        ![查找并替换 stdafx.h](images/visual-layer-interop/replace-stdafx.png)

    - 在 `pch.h` 中包含 `winrt/base.h` 和 `unknwn.h`。

        ```cppwinrt
        // reference additional headers your program requires here
        #include <unknwn.h>
        #include <winrt/base.h>
        ```

此时最好是生成项目，以确保在继续操作之前不会出错。

## <a name="create-a-class-to-host-composition-elements"></a>创建一个类用于承载合成元素

若要使用视觉层承载创建的内容，请创建一个类 (CompositionHost) 用于管理互操作并创建合成元素。  将在此类中完成用于承载 Composition API 的大部分配置，包括：

- 获取用于创建和管理 [Windows.UI.Composition](/uwp/api/windows.ui.composition) 命名空间中的对象的 [Compositor](/uwp/api/windows.ui.composition.compositor)。
- 创建一个 [DispatcherQueueController](/uwp/api/windows.system.dispatcherqueuecontroller)/[DispatcherQueue](/uwp/api/windows.system.dispatcherqueue) 用于管理 WinRT API 的任务。
- 创建一个 [DesktopWindowTarget](/uwp/api/windows.ui.composition.desktop.desktopwindowtarget) 和合成容器用于显示合成对象。

我们将此类设为单一实例，以免出现线程问题。 例如，只能为每个线程创建一个调度程序队列，因此，在同一线程中实例化 CompositionHost 的另一个实例会导致出错。

> [!TIP]
> 如果需要，请检查本教程末尾的完整代码，确保在完成本教程的过程中，所有代码位于正确的位置。

1. 向你的项目中添加一个新类文件。
    - 在“解决方案资源管理器”中右键单击“HelloComposition”项目。  
    - 在上下文菜单中，选择“添加” > “类...”。  
    - 在“添加类”对话框中，将类命名为 CompositionHost.cs，然后单击“添加”。   

1. 包含合成互操作所需的标头和 using 语句。 
    - 在 CompositionHost.h 文件的顶部，添加这些 include 语句。 

    ```cppwinrt
    #pragma once
    #include <winrt/Windows.UI.Composition.Desktop.h>
    #include <windows.ui.composition.interop.h>
    #include <DispatcherQueue.h>
    ```

    - 在 CompositionHost.cpp 文件顶部的任何 include 语句后面添加这些 using 语句。  

    ```cppwinrt
    using namespace winrt;
    using namespace Windows::System;
    using namespace Windows::UI;
    using namespace Windows::UI::Composition;
    using namespace Windows::UI::Composition::Desktop;
    using namespace Windows::Foundation::Numerics;
    ```

1. 编辑该类以使用单一实例模式。
    - 在 CompositionHost.h 中，将构造函数设为专用构造函数。
    - 声明公共静态 GetInstance 方法。 

    ```cppwinrt
    class CompositionHost
    {
    public:
        ~CompositionHost();
        static CompositionHost* GetInstance();

    private:
        CompositionHost();
    };
    ```

    - 在 CompositionHost.cpp 中，添加 GetInstance 方法的定义。 

    ```cppwinrt
    CompositionHost* CompositionHost::GetInstance()
    {
        static CompositionHost instance;
        return &instance;
    }
    ```

1. 在 CompositionHost.h 中，声明 Compositor、DispatcherQueueController 和 DesktopWindowTarget 的专用成员变量。

    ```cppwinrt
    winrt::Windows::UI::Composition::Compositor m_compositor{ nullptr };
    winrt::Windows::System::DispatcherQueueController m_dispatcherQueueController{ nullptr };
    winrt::Windows::UI::Composition::Desktop::DesktopWindowTarget m_target{ nullptr };
    ```

1. 添加一个公共方法用于初始化合成互操作对象。
    > [!NOTE]
    > 在 Initialize 中，调用 EnsureDispatcherQueue、CreateDesktopWindowTarget 和 CreateCompositionRoot 方法。     将在后续步骤中创建这些方法。

    - 在 CompositionHost.h 中，声明采用 HWND 作为参数的、名为 Initialize 的公共方法。 

    ```cppwinrt
    void Initialize(HWND hwnd);
    ```

    - 在 CompositionHost.cpp 中，添加 Initialize 方法的定义。 

    ```cppwinrt
    void CompositionHost::Initialize(HWND hwnd)
    {
        EnsureDispatcherQueue();
        if (m_dispatcherQueueController) m_compositor = Compositor();

        CreateDesktopWindowTarget(hwnd);
        CreateCompositionRoot();
    }
    ```

1. 在要使用 Windows Composition 的线程中创建调度程序队列。

    必须在包含调度程序队列的线程中创建 Compositor，因此在初始化期间首先会调用此方法。

    - 在 CompositionHost.h 中，声明名为 EnsureDispatcherQueue 的专用方法。 

    ```cppwinrt
    void EnsureDispatcherQueue();
    ```

    - 在 CompositionHost.cpp 中，添加 EnsureDispatcherQueue 方法的定义。 

    ```cppwinrt
    void CompositionHost::EnsureDispatcherQueue()
    {
        namespace abi = ABI::Windows::System;

        if (m_dispatcherQueueController == nullptr)
        {
            DispatcherQueueOptions options
            {
                sizeof(DispatcherQueueOptions), /* dwSize */
                DQTYPE_THREAD_CURRENT,          /* threadType */
                DQTAT_COM_ASTA                  /* apartmentType */
            };

            Windows::System::DispatcherQueueController controller{ nullptr };
            check_hresult(CreateDispatcherQueueController(options, reinterpret_cast<abi::IDispatcherQueueController**>(put_abi(controller))));
            m_dispatcherQueueController = controller;
        }
    }
    ```

1. 将应用的窗口注册为合成目标。
    - 在 CompositionHost.h 中，声明采用 HWND 作为参数的、名为 CreateDesktopWindowTarget 的专用方法。 

    ```cppwinrt
    void CreateDesktopWindowTarget(HWND window);
    ```

    - 在 CompositionHost.cpp 中，添加 CreateDesktopWindowTarget 方法的定义。 

    ```cppwinrt
    void CompositionHost::CreateDesktopWindowTarget(HWND window)
    {
        namespace abi = ABI::Windows::UI::Composition::Desktop;

        auto interop = m_compositor.as<abi::ICompositorDesktopInterop>();
        DesktopWindowTarget target{ nullptr };
        check_hresult(interop->CreateDesktopWindowTarget(window, false, reinterpret_cast<abi::IDesktopWindowTarget**>(put_abi(target))));
        m_target = target;
    }
    ```

1. 创建一个根视觉容器用于保存视觉对象。
    - 在 CompositionHost.h 中，声明名为 CreateCompositionRoot 的专用方法。 

    ```cppwinrt
    void CreateCompositionRoot();
    ```

    - 在 CompositionHost.cpp 中，添加 CreateCompositionRoot 方法的定义。 

    ```cppwinrt
    void CompositionHost::CreateCompositionRoot()
    {
        auto root = m_compositor.CreateContainerVisual();
        root.RelativeSizeAdjustment({ 1.0f, 1.0f });
        root.Offset({ 124, 12, 0 });
        m_target.Root(root);
    }
    ```

立即生成项目以确保不会出错。

这些方法将设置 UWP 视觉层与 Win32 API 之间的互操作所需的组件。 现在，可将内容添加到应用。

### <a name="add-composition-elements"></a>添加合成元素

准备好基础结构后，接下来可以生成要显示的合成内容。

对于本示例，你将添加用于创建随机色彩的正方形 [SpriteVisual](/uwp/api/windows.ui.composition.spritevisual) 的代码，经过短暂的延迟后，该正方形将通过动画效果落下。

1. 添加合成元素。
    - 在 CompositionHost.h 中，声明采用 3 个浮点值作为参数的、名为 AddElement 的公共方法。  

    ```cppwinrt
    void AddElement(float size, float x, float y);
    ```

    - 在 CompositionHost.cpp 中，添加 AddElement 方法的定义。 

    ```cppwinrt
    void CompositionHost::AddElement(float size, float x, float y)
    {
        if (m_target.Root())
        {
            auto visuals = m_target.Root().as<ContainerVisual>().Children();
            auto visual = m_compositor.CreateSpriteVisual();

            auto element = m_compositor.CreateSpriteVisual();
            uint8_t r = (double)(double)(rand() % 255);;
            uint8_t g = (double)(double)(rand() % 255);;
            uint8_t b = (double)(double)(rand() % 255);;

            element.Brush(m_compositor.CreateColorBrush({ 255, r, g, b }));
            element.Size({ size, size });
            element.Offset({ x, y, 0.0f, });

            auto animation = m_compositor.CreateVector3KeyFrameAnimation();
            auto bottom = (float)600 - element.Size().y;
            animation.InsertKeyFrame(1, { element.Offset().x, bottom, 0 });

            using timeSpan = std::chrono::duration<int, std::ratio<1, 1>>;

            std::chrono::seconds duration(2);
            std::chrono::seconds delay(3);

            animation.Duration(timeSpan(duration));
            animation.DelayTime(timeSpan(delay));
            element.StartAnimation(L"Offset", animation);
            visuals.InsertAtTop(element);

            visuals.InsertAtTop(visual);
        }
    }
    ```

## <a name="create-and-show-the-window"></a>创建并显示窗口

现在，可以在 Win32 UI 中添加一个按钮和 UWP 合成内容。

1. 在 HelloComposition.cpp 文件的顶部包含 CompositionHost.h，定义 BTN_ADD，并获取 CompositionHost 的实例。 

    ```cppwinrt
    #include "CompositionHost.h"

    // #define MAX_LOADSTRING 100 // This is already in the file.
    #define BTN_ADD 1000

    CompositionHost* compHost = CompositionHost::GetInstance();
    ```

1. 在 `InitInstance` 方法中，更改创建的窗口的大小。 （在此行中，将 `CW_USEDEFAULT, 0` 更改为 `900, 672`。）

    ```cppwinrt
    HWND hWnd = CreateWindowW(szWindowClass, szTitle, WS_OVERLAPPEDWINDOW,
        CW_USEDEFAULT, 0, 900, 672, nullptr, nullptr, hInstance, nullptr);
    ```

1. 在 WndProc 函数中，将 `case WM_CREATE` 添加到 message 开关块。  在本例中，你将初始化 CompositionHost 并创建按钮。

    ```cppwinrt
    case WM_CREATE:
    {
        compHost->Initialize(hWnd);
        srand(time(nullptr));

        CreateWindow(TEXT("button"), TEXT("Add element"),
            WS_VISIBLE | WS_CHILD | BS_PUSHBUTTON,
            12, 12, 100, 50,
            hWnd, (HMENU)BTN_ADD, nullptr, nullptr);
    }
    break;
    ```

1. 另外，在 WndProc 函数中，处理按钮单击以将合成元素添加到 UI。 

    将 `case BTN_ADD` 添加到 WM_COMMAND 块中的 wmId 开关块。 

    ```cppwinrt
    case BTN_ADD: // addButton click
    {
        double size = (double)(rand() % 150 + 50);
        double x = (double)(rand() % 600);
        double y = (double)(rand() % 200);
        compHost->AddElement(size, x, y);
        break;
    }
    ```

现在，可以生成并运行你的应用。 如果需要，请检查本教程末尾的完整代码，确保所有代码位于正确的位置。

运行应用并单击按钮时，应会看到已添加到 UI 中的动画正方形。

## <a name="additional-resources"></a>其他资源

- [Win32 HelloComposition 示例 (GitHub)](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloComposition)
- [Win32 和 C++ 入门](/windows/desktop/learnwin32/learn-to-program-for-windows)
- [Windows 10 应用入门](/windows/uwp/get-started/) (UWP)
- [增强适用于 Windows 10 的桌面应用程序](/windows/uwp/porting/desktop-to-uwp-enhance) (UWP)
- [Windows.UI.Composition 命名空间](/uwp/api/windows.ui.composition) (UWP)

## <a name="complete-code"></a>完成代码

下面是 CompositionHost 类和 InitInstance 方法的完整代码。

### <a name="compositionhosth"></a>CompositionHost.h

```cppwinrt
#pragma once
#include <winrt/Windows.UI.Composition.Desktop.h>
#include <windows.ui.composition.interop.h>
#include <DispatcherQueue.h>

class CompositionHost
{
public:
    ~CompositionHost();
    static CompositionHost* GetInstance();

    void Initialize(HWND hwnd);
    void AddElement(float size, float x, float y);

private:
    CompositionHost();

    void CreateDesktopWindowTarget(HWND window);
    void EnsureDispatcherQueue();
    void CreateCompositionRoot();

    winrt::Windows::UI::Composition::Compositor m_compositor{ nullptr };
    winrt::Windows::UI::Composition::Desktop::DesktopWindowTarget m_target{ nullptr };
    winrt::Windows::System::DispatcherQueueController m_dispatcherQueueController{ nullptr };
};
```

### <a name="compositionhostcpp"></a>CompositionHost.cpp

```cppwinrt
#include "pch.h"
#include "CompositionHost.h"

using namespace winrt;
using namespace Windows::System;
using namespace Windows::UI;
using namespace Windows::UI::Composition;
using namespace Windows::UI::Composition::Desktop;
using namespace Windows::Foundation::Numerics;

CompositionHost::CompositionHost()
{
}

CompositionHost* CompositionHost::GetInstance()
{
    static CompositionHost instance;
    return &instance;
}

CompositionHost::~CompositionHost()
{
}

void CompositionHost::Initialize(HWND hwnd)
{
    EnsureDispatcherQueue();
    if (m_dispatcherQueueController) m_compositor = Compositor();

    if (m_compositor)
    {
        CreateDesktopWindowTarget(hwnd);
        CreateCompositionRoot();
    }
}

void CompositionHost::EnsureDispatcherQueue()
{
    namespace abi = ABI::Windows::System;

    if (m_dispatcherQueueController == nullptr)
    {
        DispatcherQueueOptions options
        {
            sizeof(DispatcherQueueOptions), /* dwSize */
            DQTYPE_THREAD_CURRENT,          /* threadType */
            DQTAT_COM_ASTA                  /* apartmentType */
        };

        Windows::System::DispatcherQueueController controller{ nullptr };
        check_hresult(CreateDispatcherQueueController(options, reinterpret_cast<abi::IDispatcherQueueController**>(put_abi(controller))));
        m_dispatcherQueueController = controller;
    }
}

void CompositionHost::CreateDesktopWindowTarget(HWND window)
{
    namespace abi = ABI::Windows::UI::Composition::Desktop;

    auto interop = m_compositor.as<abi::ICompositorDesktopInterop>();
    DesktopWindowTarget target{ nullptr };
    check_hresult(interop->CreateDesktopWindowTarget(window, false, reinterpret_cast<abi::IDesktopWindowTarget**>(put_abi(target))));
    m_target = target;
}

void CompositionHost::CreateCompositionRoot()
{
    auto root = m_compositor.CreateContainerVisual();
    root.RelativeSizeAdjustment({ 1.0f, 1.0f });
    root.Offset({ 124, 12, 0 });
    m_target.Root(root);
}

void CompositionHost::AddElement(float size, float x, float y)
{
    if (m_target.Root())
    {
        auto visuals = m_target.Root().as<ContainerVisual>().Children();
        auto visual = m_compositor.CreateSpriteVisual();

        auto element = m_compositor.CreateSpriteVisual();
        uint8_t r = (double)(double)(rand() % 255);;
        uint8_t g = (double)(double)(rand() % 255);;
        uint8_t b = (double)(double)(rand() % 255);;

        element.Brush(m_compositor.CreateColorBrush({ 255, r, g, b }));
        element.Size({ size, size });
        element.Offset({ x, y, 0.0f, });

        auto animation = m_compositor.CreateVector3KeyFrameAnimation();
        auto bottom = (float)600 - element.Size().y;
        animation.InsertKeyFrame(1, { element.Offset().x, bottom, 0 });

        using timeSpan = std::chrono::duration<int, std::ratio<1, 1>>;

        std::chrono::seconds duration(2);
        std::chrono::seconds delay(3);

        animation.Duration(timeSpan(duration));
        animation.DelayTime(timeSpan(delay));
        element.StartAnimation(L"Offset", animation);
        visuals.InsertAtTop(element);

        visuals.InsertAtTop(visual);
    }
}
```

### <a name="hellocompositioncpp-partial"></a>HelloComposition.cpp（部分）

```cppwinrt
#include "pch.h"
#include "HelloComposition.h"
#include "CompositionHost.h"

#define MAX_LOADSTRING 100
#define BTN_ADD 1000

CompositionHost* compHost = CompositionHost::GetInstance();

// Global Variables:

// ...
// ... code not shown ...
// ...

BOOL InitInstance(HINSTANCE hInstance, int nCmdShow)
{
   hInst = hInstance; // Store instance handle in our global variable

   HWND hWnd = CreateWindowW(szWindowClass, szTitle, WS_OVERLAPPEDWINDOW,
      CW_USEDEFAULT, 0, 900, 672, nullptr, nullptr, hInstance, nullptr);

// ...
// ... code not shown ...
// ...
}

// ...

LRESULT CALLBACK WndProc(HWND hWnd, UINT message, WPARAM wParam, LPARAM lParam)
{
    switch (message)
    {
// Add this...
    case WM_CREATE:
    {
        compHost->Initialize(hWnd);
        srand(time(nullptr));

        CreateWindow(TEXT("button"), TEXT("Add element"),
            WS_VISIBLE | WS_CHILD | BS_PUSHBUTTON,
            12, 12, 100, 50,
            hWnd, (HMENU)BTN_ADD, nullptr, nullptr);
    }
    break;
// ...
    case WM_COMMAND:
    {
        int wmId = LOWORD(wParam);
        // Parse the menu selections:
        switch (wmId)
        {
        case IDM_ABOUT:
            DialogBox(hInst, MAKEINTRESOURCE(IDD_ABOUTBOX), hWnd, About);
            break;
        case IDM_EXIT:
            DestroyWindow(hWnd);
            break;
// Add this...
        case BTN_ADD: // addButton click
        {
            double size = (double)(rand() % 150 + 50);
            double x = (double)(rand() % 600);
            double y = (double)(rand() % 200);
            compHost->AddElement(size, x, y);
            break;
        }
// ...
        default:
            return DefWindowProc(hWnd, message, wParam, lParam);
        }
    }
    break;
    case WM_PAINT:
    {
        PAINTSTRUCT ps;
        HDC hdc = BeginPaint(hWnd, &ps);
        // TODO: Add any drawing code that uses hdc here...
        EndPaint(hWnd, &ps);
    }
    break;
    case WM_DESTROY:
        PostQuitMessage(0);
        break;
    default:
        return DefWindowProc(hWnd, message, wParam, lParam);
    }
    return 0;
}

// ...
// ... code not shown ...
// ...
```