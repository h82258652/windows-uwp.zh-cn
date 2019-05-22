---
title: 可视化层中使用 Win32
description: 使用可视化层以增强您的 Win32 桌面应用程序的 UI。
template: detail.hbs
ms.date: 03/18/2019
ms.topic: article
keywords: UWP，呈现，组合，win32
ms.localizationpriority: medium
ms.openlocfilehash: 0cf4e45ac6214e714e2c73a73006654584f50799
ms.sourcegitcommit: f0f933d5cf0be734373a7b03e338e65000cc3d80
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/21/2019
ms.locfileid: "65985237"
---
# <a name="using-the-visual-layer-with-win32"></a>可视化层中使用 Win32

可以使用 Windows 运行时组合 Api (也称为[可视化层](/windows/uwp/composition/visual-layer)) 在 Win32 应用程序以创建现代的轻型注册 Windows 10 用户体验。

GitHub 上提供了本教程的完整代码：[Win32 HelloComposition 示例](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloComposition)。

需要精确地控制其 UI 复合的通用 Windows 应用程序有权访问[Windows.UI.Composition](/uwp/api/windows.ui.composition)命名空间进行细化的控制它们的 UI 是组成和呈现方式。 此组合 API 但是并不局限于 UWP 应用。 Win32 桌面应用程序可以充分利用 UWP 和 Windows 10 中的现代组合系统。

## <a name="prerequisites"></a>先决条件

托管 API UWP 具有这些系统必备组件。

- 我们假定您已具备一定的使用 Win32 和 UWP 应用开发。 有关详细信息，请参阅：
  - [开始使用 Win32 和C++](/windows/desktop/learnwin32/learn-to-program-for-windows)
  - [开始使用 Windows 10 应用](/windows/uwp/get-started/)
  - [增强适用于 Windows 10 桌面应用程序](/windows/uwp/porting/desktop-to-uwp-enhance)
- Windows 10 版本 1803 版或更高版本
- Windows 10 SDK 17134 或更高版本

## <a name="how-to-use-composition-apis-from-a-win32-desktop-application"></a>如何使用组合 Api 从 Win32 桌面应用程序

在本教程中，创建简单的 Win32C++应用程序并向其添加 UWP 组合元素。 重点是正确配置的项目、 创建互操作代码，以及绘制简单使用 Windows 组合 Api。 已完成的应用外观如下所示。

![在正在运行的应用程序 UI](images/visual-layer-interop/win32-comp-interop-app-ui.png)

## <a name="create-a-c-win32-project-in-visual-studio"></a>创建C++Visual Studio 中的 Win32 项目

第一步是在 Visual Studio 中创建 Win32 应用程序项目。

若要创建新的 Win32 应用程序项目中C++名为_HelloComposition_:

1. 打开 Visual Studio 并选择**文件** > **新建** > **项目**。

    **新的项目**对话框随即打开。
1. 下**已安装**类别中，展开**Visual C++** 节点，，然后选择**Windows Desktop**。
1. 选择**Windows 桌面应用程序**模板。
1. 输入的名称_HelloComposition_，然后单击**确定**。

    Visual Studio 创建项目并打开主应用程序文件的编辑器。

## <a name="configure-the-project-to-use-windows-runtime-apis"></a>将项目配置为使用 Windows 运行时 Api

若要使用 Windows 运行时 (WinRT) Api 在 Win32 应用程序中，我们使用C++/WinRT。 你需要配置 Visual Studio 项目添加C++/WinRT 支持。

(有关详细信息，请参阅[入门C++/WinRT-修改 Windows 桌面应用程序项目，以添加C++/WinRT 支持](/windows/uwp/cpp-and-winrt-apis/get-started.md#modify-a-windows-desktop-application-project-to-add-cwinrt-support))。

1. 从**项目**菜单中，打开项目属性 (_HelloComposition 属性_)，并确保以下设置是否设为指定的值：

    - 有关**配置**，选择_所有配置_。 有关**平台**，选择_所有平台_。
    - **配置属性** > **常规** > **Windows SDK 版本** = _10.0.17763.0_或更高版本

    ![设置 SDK 版本](images/visual-layer-interop/sdk-version.png)

    - **C /C++** > **语言** >   **C++语言标准** = _ISO C++ 17 标准 (/ stf:c + + 17)_

    ![设置的语言标准](images/visual-layer-interop/language-standard.png)

    - **链接器** > **输入** > **附加依赖项**必须包括"_windowsapp.lib_"。 如果它不包含在列表中，请将其添加。

    ![添加链接器依赖项](images/visual-layer-interop/linker-dependencies.png)

1. 更新预编译标头

    - 重命名`stdafx.h`并`stdafx.cpp`到`pch.h`和`pch.cpp`分别。
    - 设置项目属性**C /C++** > **预编译标头** > **预编译头文件**到*pch.h*.
    - 查找和替换`#include "stdafx.h"`与`#include "pch.h"`中的所有文件。

        (**编辑** > **查找和替换** > **在文件中查找**)

        ![查找和替换 stdafx.h](images/visual-layer-interop/replace-stdafx.png)

    - 在中`pch.h`，包括`winrt/base.h`和`unknwn.h`。

        ```cppwinrt
        // reference additional headers your program requires here
        #include <unknwn.h>
        #include <winrt/base.h>
        ```

它是一个好办法生成项目，在这里以确保在继续之前没有任何错误。

## <a name="create-a-class-to-host-composition-elements"></a>创建类中的，以便主机组合元素

承载内容创建和可视化层，创建一个类 (_CompositionHost_) 来管理互操作以及创建组合元素。 这是您可以执行操作的大多数托管组合 Api，包括配置：

- 获取[复合器](/uwp/api/windows.ui.composition.compositor)，其用于创建和管理中的对象[Windows.UI.Composition](/uwp/api/windows.ui.composition)命名空间。
- 创建[DispatcherQueueController](/uwp/api/windows.system.dispatcherqueuecontroller)/[DispatcherQueue](/uwp/api/windows.system.dispatcherqueue) WinRT Api 的管理任务。
- 创建[DesktopWindowTarget](/uwp/api/windows.ui.composition.desktop.desktopwindowtarget)和撰写容器以显示组合对象。

我们将此类的单一实例，以避免线程处理问题。 例如，你只能创建一个调度程序队列，每个线程，以便实例化 CompositionHost 同一线程上的第二个实例会导致错误。

> [!TIP]
> 如果需要检查的教程，请确保所有代码都都在正确位置放置在演练本教程末尾的完整代码。

1. 向你的项目中添加一个新类文件。
    - 在中**解决方案资源管理器**，右键单击_HelloComposition_项目。
    - 在上下文菜单中，选择**外** > **类...**.
    - 在中**添加类**对话框中，类命名_CompositionHost.cs_，然后单击**添加**。

1. 包含标头和_using_所需的组合互操作。
    - 在中 CompositionHost.h，添加以下_包括_文件的顶部。

    ```cppwinrt
    #pragma once
    #include <winrt/Windows.UI.Composition.Desktop.h>
    #include <windows.ui.composition.interop.h>
    #include <DispatcherQueue.h>
    ```

    - 在中 CompositionHost.cpp，添加以下_using_后任何文件的顶部_包括_。

    ```cppwinrt
    using namespace winrt;
    using namespace Windows::System;
    using namespace Windows::UI;
    using namespace Windows::UI::Composition;
    using namespace Windows::UI::Composition::Desktop;
    using namespace Windows::Foundation::Numerics;
    ```

1. 编辑要使用单一实例模式的类。
    - 在 CompositionHost.h，请在构造函数专用。
    - 声明为公共静态_GetInstance_方法。

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

    - 在 CompositionHost.cpp，添加的定义_GetInstance_方法。

    ```cppwinrt
    CompositionHost* CompositionHost::GetInstance()
    {
        static CompositionHost instance;
        return &instance;
    }
    ```

1. CompositionHost.h，在声明私有成员变量的复合器、 DispatcherQueueController，和 DesktopWindowTarget。

    ```cppwinrt
    winrt::Windows::UI::Composition::Compositor m_compositor{ nullptr };
    winrt::Windows::System::DispatcherQueueController m_dispatcherQueueController{ nullptr };
    winrt::Windows::UI::Composition::Desktop::DesktopWindowTarget m_target{ nullptr };
    ```

1. 添加一个公共方法来初始化组合互操作对象。
    > [!NOTE]
    > 在_初始化_，则调用_EnsureDispatcherQueue_， _CreateDesktopWindowTarget_，以及_CreateCompositionRoot_方法。 在下一步骤中创建这些方法。

    - 在 CompositionHost.h，声明一个名为的公共方法_初始化_采用 HWND 作为自变量。

    ```cppwinrt
    void Initialize(HWND hwnd);
    ```

    - 在 CompositionHost.cpp，添加的定义_初始化_方法。

    ```cppwinrt
    void CompositionHost::Initialize(HWND hwnd)
    {
        EnsureDispatcherQueue();
        if (m_dispatcherQueueController) m_compositor = Compositor();

        CreateDesktopWindowTarget(hwnd);
        CreateCompositionRoot();
    }
    ```

1. 将使用 Windows 组合的线程上创建一个调度程序队列。

    必须具有的调度程序队列，因此此方法在初始化期间调用第一个线程上创建排序器。

    - 在 CompositionHost.h，声明一个名为私有方法_EnsureDispatcherQueue_。

    ```cppwinrt
    void EnsureDispatcherQueue();
    ```

    - 在 CompositionHost.cpp，添加的定义_EnsureDispatcherQueue_方法。

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

1. 将在应用程序窗口注册为一个组合目标。
    - 在 CompositionHost.h，声明一个名为私有方法_CreateDesktopWindowTarget_采用 HWND 作为自变量。

    ```cppwinrt
    void CreateDesktopWindowTarget(HWND window);
    ```

    - 在 CompositionHost.cpp，添加的定义_CreateDesktopWindowTarget_方法。

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

1. 创建用于保存视觉对象的根 visual 容器。
    - 在 CompositionHost.h，声明一个名为私有方法_CreateCompositionRoot_。

    ```cppwinrt
    void CreateCompositionRoot();
    ```

    - 在 CompositionHost.cpp，添加的定义_CreateCompositionRoot_方法。

    ```cppwinrt
    void CompositionHost::CreateCompositionRoot()
    {
        auto root = m_compositor.CreateContainerVisual();
        root.RelativeSizeAdjustment({ 1.0f, 1.0f });
        root.Offset({ 24, 24, 0 });
        m_target.Root(root);
    }
    ```

生成项目现在以确保没有任何错误。

这些方法将设置为可视化层 UWP 和 Win32 Api 之间的互操作所需的组件。 现在可以将内容添加到您的应用程序。

### <a name="add-composition-elements"></a>添加组合元素

与基础结构，您现在可以生成你想要显示的组合内容。

对于此示例中，添加代码，创建一个简单的正方形[SpriteVisual](/uwp/api/windows.ui.composition.spritevisual)。

1. 添加组合元素。
    - 在 CompositionHost.h，声明一个名为的公共方法_AddElement_采用 3 **float**值作为参数。

    ```cppwinrt
    void AddElement(float size, float x, float y);
    ```

    - 在 CompositionHost.cpp，添加的定义_AddElement_方法。

    ```cppwinrt
    void CompositionHost::AddElement(float size, float x, float y)
    {
        if (m_target.Root())
        {
            auto visuals = m_target.Root().as<ContainerVisual>().Children();
            auto visual = m_compositor.CreateSpriteVisual();

            visual.Brush(m_compositor.CreateColorBrush({ 0xDC, 0x5B, 0x9B, 0xD5 }));
            visual.Size({ size, size });
            visual.Offset({ x, y, 0.0f, });

            visuals.InsertAtTop(visual);
        }
    }
    ```

## <a name="create-and-show-the-window"></a>创建和显示窗口

现在，可以将 UWP 组合内容添加到你 Win32 的 UI。 使用应用程序_InitInstance_方法来初始化 Windows 组合和生成的内容。

1. 在 HelloComposition.cpp，包括_CompositionHost.h_文件的顶部。

    ```cppwinrt
    #include "CompositionHost.h"
    ```

1. 在`InitInstance`方法中，添加代码以初始化和使用 CompositionHost 类。

    将以下代码创建 HWND 后，在对调用之前添加`ShowWindow`。

    ```cppwinrt
    CompositionHost* compHost = CompositionHost::GetInstance();
    compHost->Initialize(hWnd);
    compHost->AddElement(150, 10, 10);
    ```

你现在可以生成并运行您的应用程序。 如果需要检查以确保所有代码都是正确的位置在本教程的结尾处的完整代码。

当您运行该应用程序时，应看到添加到 UI 中的蓝色正方形。

## <a name="additional-resources"></a>其他资源

- [Win32 HelloComposition 示例 (GitHub)](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloComposition)
- [开始使用 Win32 和C++](/windows/desktop/learnwin32/learn-to-program-for-windows)
- [开始使用 Windows 10 应用](/windows/uwp/get-started/)(UWP)
- [增强适用于 Windows 10 桌面应用程序](/windows/uwp/porting/desktop-to-uwp-enhance)(UWP)
- [Windows.UI.Composition 命名空间](/uwp/api/windows.ui.composition)(UWP)

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
    root.Offset({ 24, 24, 0 });
    m_target.Root(root);
}

void CompositionHost::AddElement(float size, float x, float y)
{
    if (m_target.Root())
    {
        auto visuals = m_target.Root().as<ContainerVisual>().Children();
        auto visual = m_compositor.CreateSpriteVisual();

        visual.Brush(m_compositor.CreateColorBrush({ 0xDC, 0x5B, 0x9B, 0xD5 }));
        visual.Size({ size, size });
        visual.Offset({ x, y, 0.0f, });

        visuals.InsertAtTop(visual);
    }
}
```

### <a name="hellocompositioncpp---initinstance"></a>HelloComposition.cpp - InitInstance

```cppwinrt
BOOL InitInstance(HINSTANCE hInstance, int nCmdShow)
{
   hInst = hInstance; // Store instance handle in our global variable

   HWND hWnd = CreateWindowW(szWindowClass, szTitle, WS_OVERLAPPEDWINDOW,
      CW_USEDEFAULT, 0, CW_USEDEFAULT, 0, nullptr, nullptr, hInstance, nullptr);

   if (!hWnd)
   {
      return FALSE;
   }

   CompositionHost* compHost = CompositionHost::GetInstance();
   compHost->Initialize(hWnd);
   compHost->AddElement(150, 10, 10);

   ShowWindow(hWnd, nCmdShow);
   UpdateWindow(hWnd);

   return TRUE;
}
```