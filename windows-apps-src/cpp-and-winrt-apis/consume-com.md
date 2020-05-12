---
description: 本主题通过一个完整的 Direct2D 代码示例展示了如何通过 C++/WinRT 来使用 COM 类和接口。
title: 通过 C++/WinRT 使用 COM 组件
ms.date: 04/24/2019
ms.topic: article
keywords: windows 10, uwp, 标准, c++, cpp, winrt, COM, 组件, 类, 接口
ms.localizationpriority: medium
ms.openlocfilehash: 1b6ce3ce56b4afbf4c45b406c8af369bee4b55bb
ms.sourcegitcommit: 2dbf4a3f3473c1d3a0ad988bcbae6e75dfee3640
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/01/2020
ms.locfileid: "82619321"
---
# <a name="consume-com-components-with-cwinrt"></a>通过 C++/WinRT 使用 COM 组件

可以通过 [ C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 库的工具来使用 COM 组件，例如 DirectX API 的高性能 2D 和 3D 图形。 C++/ WinRT 是在不影响性能的情况下使用 DirectX 的最简单方法。 本主题借助一个 Direct2D 代码示例演示如何通过 C++/WinRT 使用 COM 类和接口。 当然，你也可以在同一个 C++/WinRT 项目中混合使用 COM 和 Windows 运行时编程方法。

本主题的末尾提供了一个精简 Direct2D 应用程序的完整源代码列表。 我们将提取该代码的摘录内容，演示如何使用 C++/WinRT 库的各种工具通过 C++/WinRT 来使用 COM 组件。

## <a name="com-smart-pointers-winrtcom_ptr"></a>COM 智能指针 ([**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr))

使用 COM 编程时，你会直接使用接口而不是对象（Windows 运行时 API 在幕后也是如此，这是 COM 的一种演进）。 若要针对 COM 类调用函数（例如，激活该类），需先获取一个接口，然后针对该接口调用该函数。 若要访问对象的状态，请不要直接访问其数据成员，而应该针对某个接口调用取值函数和赋值函数。

更具体而言，我们将讨论如何与接口指针交互。  对于这种操作，我们可以受益于 C++/WinRT 中的 COM 智能指针类型 &mdash;[**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr) 类型。

```cppwinrt
#include <d2d1_1.h>
...
winrt::com_ptr<ID2D1Factory1> factory;
```

以上代码演示如何声明指向 [**ID2D1Factory1**](/windows/desktop/api/d2d1_1/nn-d2d1_1-id2d1factory1) COM 接口的未初始化智能指针。 该智能指针未初始化，因此暂时不会指向属于任何实际对象的 **ID2D1Factory1** 接口（根本不指向任何接口）。 但它可以做到这一点；（作为一个智能指针）它能够通过 COM 引用计数来管理它所指向的接口的拥有对象的生存期，并充当中介来让你针对该接口调用函数。

## <a name="com-functions-that-return-an-interface-pointer-as-void"></a>返回 **void** 形式的接口指针的 COM 函数

可以调用 [**com_ptr::put_void**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput_void-function) 函数写入到未初始化智能指针的基础原始指针。

```cppwinrt
D2D1_FACTORY_OPTIONS options{ D2D1_DEBUG_LEVEL_NONE };
D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    __uuidof(factory),
    &options,
    factory.put_void()
);
```

以上代码调用 [**D2D1CreateFactory**](/windows/desktop/api/d2d1/nf-d2d1-d2d1createfactory) 函数，该函数通过其最后一个参数返回 **void\*\*** 类型的 **ID2D1Factory1** 接口指针。 许多 COM 函数返回 **void\*\*** 。 对于此类函数，请按如下所示使用 [**com_ptr::put_void**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput_void-function)。

## <a name="com-functions-that-return-a-specific-interface-pointer"></a>返回特定接口指针的 COM 函数

[**D3D11CreateDevice**](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) 函数通过其倒数第三个参数返回 **ID3D11Device\*\*** 类型的 [**ID3D11Device**](/windows/desktop/api/d3d11/nn-d3d11-id3d11device) 接口指针。 对于返回此类特定接口指针的函数，请使用 [**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptr_put-function)。

```cppwinrt
winrt::com_ptr<ID3D11Device> device;
D3D11CreateDevice(
    ...
    device.put(),
    ...);
```

此节前面一节中的代码示例演示如何调用原始 **D2D1CreateFactory** 函数。 但实际上，当本主题中的代码示例调用 **D2D1CreateFactory** 时，它会使用一个帮助器函数模板来包装原始 API，因此，代码示例实际使用的是 [**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptr_put-function)。

```cppwinrt
winrt::com_ptr<ID2D1Factory1> factory;
D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    options,
    factory.put());
```

## <a name="com-functions-that-return-an-interface-pointer-as-iunknown"></a>返回 **IUnknown** 形式的接口指针的 COM 函数

[**DWriteCreateFactory**](/windows/desktop/api/dwrite/nf-dwrite-dwritecreatefactory) 函数通过其最后一个参数返回 [**IUnknown**](/windows/desktop/api/unknwn/nn-unknwn-iunknown) 类型的 DirectWrite 工厂接口指针。 对于此类函数，请使用 [**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptr_put-function)，但需要将其重新解释并转换为 **IUnknown**。

```cppwinrt
DWriteCreateFactory(
    DWRITE_FACTORY_TYPE_SHARED,
    __uuidof(dwriteFactory2),
    reinterpret_cast<IUnknown**>(dwriteFactory2.put()));
```

## <a name="re-seat-a-winrtcom_ptr"></a>重新定位 **winrt::com_ptr**

> [!IMPORTANT]
> 如果某个 [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr) 已定位（其内部原始指针已有目标），而你想要将其重新定位为指向不同的对象，则首先需要向其分配 `nullptr`&mdash; 如以下代码示例所示。 否则，已定位的 **com_ptr** 会断言其内部指针不为 null，从而产生需要关注的问题（调用 [**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptr_put-function) 或 [**com_ptr::put_void**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput_void-function) 时）。

```cppwinrt
winrt::com_ptr<ID2D1SolidColorBrush> brush;
...
    brush.put()
...
brush = nullptr; // Important because we're about to re-seat
target->CreateSolidColorBrush(
    color_orange,
    D2D1::BrushProperties(0.8f),
    brush.put()));
```

## <a name="handle-hresult-error-codes"></a>处理 HRESULT 错误代码

若要检查 COM 函数返回的 HRESULT 值，并在它显示错误代码时引发异常，请调用 [**winrt::check_hresult**](/uwp/cpp-ref-for-winrt/error-handling/check-hresult)。

```cppwinrt
winrt::check_hresult(D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    __uuidof(factory),
    options,
    factory.put_void()));
```

## <a name="com-functions-that-take-a-specific-interface-pointer"></a>采用特定接口指针的 COM 函数

可以调用 [**com_ptr::get**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrget-function) 函数，将 **com_ptr** 传递给采用相同类型的特定接口指针的函数。

```cppwinrt
... ExampleFunction(
    winrt::com_ptr<ID2D1Factory1> const& factory,
    winrt::com_ptr<IDXGIDevice> const& dxdevice)
{
    ...
    winrt::check_hresult(factory->CreateDevice(dxdevice.get(), ...));
    ...
}
```

## <a name="com-functions-that-take-an-iunknown-interface-pointer"></a>采用 **IUnknown** 接口指针的 COM 函数

可以使用 [com_ptr::get](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrget-function)，将 com_ptr 传递给采用 IUnknown 接口指针的函数    。

可以使用 [winrt::get_unknown](/uwp/cpp-ref-for-winrt/get-unknown) 自由函数返回投影类型的对象的基础原始 [IUnknown 接口](/windows/win32/api/unknwn/nn-unknwn-iunknown)的地址（也就是说，指向该接口的指针）  。 然后，可以将该地址传递给采用 IUnknown 接口指针的函数  。

有关投影类型  的信息，请参阅[通过 C++/WinRT 使用 API](/windows/uwp/cpp-and-winrt-apis/consume-apis)。

有关 get_unknown 的代码示例，请参阅 [winrt::get_unknown](/uwp/cpp-ref-for-winrt/get-unknown)，或本主题中的[一个精简 Direct2D 应用程序的完整源代码列表](/windows/uwp/cpp-and-winrt-apis/consume-com#full-source-code-listing-of-a-minimal-direct2d-application)   。

## <a name="passing-and-returning-com-smart-pointers"></a>传递和返回 COM 智能指针

采用 **winrt::com_ptr** 形式的 COM 智能指针的函数应该按常量引用或按引用来采用该指针。

```cppwinrt
... GetDxgiFactory(winrt::com_ptr<ID3D11Device> const& device) ...

... CreateDevice(..., winrt::com_ptr<ID3D11Device>& device) ...
```

返回 **winrt::com_ptr** 的函数应按值返回该指针。

```cppwinrt
winrt::com_ptr<ID2D1Factory1> CreateFactory() ...
```

## <a name="query-a-com-smart-pointer-for-a-different-interface"></a>查询不同接口的 COM 智能指针

可以使用 [**com_ptr::as**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptras-function) 函数查询不同接口的 COM 智能指针。 如果查询未成功，该函数将引发异常。

```cppwinrt
void ExampleFunction(winrt::com_ptr<ID3D11Device> const& device)
{
    ...
    winrt::com_ptr<IDXGIDevice> const dxdevice{ device.as<IDXGIDevice>() };
    ...
}
```

或者可以使用 [**com_ptr::try_as**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrtry_as-function)，该函数会返回一个值，将该值与 `nullptr` 进行比较可以确定查询是否成功。

## <a name="full-source-code-listing-of-a-minimal-direct2d-application"></a>一个精简 Direct2D 应用程序的完整源代码列表

若要生成并运行此源代码示例，请先在 Visual Studio 中创建一个新的 **Core 应用 (C++/WinRT)** 。 `Direct2D` 是项目的合理名称，但你可以指定任意名称。

打开 `pch.h`，并在包含 `windows.h` 后立即添加 `#include <unknwn.h>`。 这是因为我们使用的是 [winrt::get_unknown  ](/uwp/cpp-ref-for-winrt/get-unknown)。 在使用 winrt::get_unknown  时，即使该标头已包含在另一个标头中，显式执行 `#include <unknwn.h>` 仍是不错的做法。

打开 `App.cpp`，删除其整个内容，然后粘贴以下列表。

以下代码会尽量使用 [winrt::com_ptr::capture 函数](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrcapture-function)。 `WINRT_ASSERT` 是宏定义，并且扩展到 [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros)。

```cppwinrt
#include "pch.h"
#include <d2d1_1.h>
#include <d3d11.h>
#include <dxgi1_2.h>
#include <winrt/Windows.Graphics.Display.h>

using namespace winrt;

using namespace Windows;
using namespace Windows::ApplicationModel::Core;
using namespace Windows::UI;
using namespace Windows::UI::Core;
using namespace Windows::Graphics::Display;

namespace
{
    winrt::com_ptr<ID2D1Factory1> CreateFactory()
    {
        D2D1_FACTORY_OPTIONS options{};

#ifdef _DEBUG
        options.debugLevel = D2D1_DEBUG_LEVEL_INFORMATION;
#endif

        winrt::com_ptr<ID2D1Factory1> factory;

        winrt::check_hresult(D2D1CreateFactory(
            D2D1_FACTORY_TYPE_SINGLE_THREADED,
            options,
            factory.put()));

        return factory;
    }

    HRESULT CreateDevice(D3D_DRIVER_TYPE const type, winrt::com_ptr<ID3D11Device>& device)
    {
        WINRT_ASSERT(!device);

        return D3D11CreateDevice(
            nullptr,
            type,
            nullptr,
            D3D11_CREATE_DEVICE_BGRA_SUPPORT,
            nullptr, 0,
            D3D11_SDK_VERSION,
            device.put(),
            nullptr,
            nullptr);
    }

    winrt::com_ptr<ID3D11Device> CreateDevice()
    {
        winrt::com_ptr<ID3D11Device> device;
        HRESULT hr{ CreateDevice(D3D_DRIVER_TYPE_HARDWARE, device) };

        if (DXGI_ERROR_UNSUPPORTED == hr)
        {
            hr = CreateDevice(D3D_DRIVER_TYPE_WARP, device);
        }

        winrt::check_hresult(hr);
        return device;
    }

    winrt::com_ptr<ID2D1DeviceContext> CreateRenderTarget(
        winrt::com_ptr<ID2D1Factory1> const& factory,
        winrt::com_ptr<ID3D11Device> const& device)
    {
        WINRT_ASSERT(factory);
        WINRT_ASSERT(device);

        winrt::com_ptr<IDXGIDevice> const dxdevice{ device.as<IDXGIDevice>() };

        winrt::com_ptr<ID2D1Device> d2device;
        winrt::check_hresult(factory->CreateDevice(dxdevice.get(), d2device.put()));

        winrt::com_ptr<ID2D1DeviceContext> target;
        winrt::check_hresult(d2device->CreateDeviceContext(D2D1_DEVICE_CONTEXT_OPTIONS_NONE, target.put()));
        return target;
    }

    winrt::com_ptr<IDXGIFactory2> GetDxgiFactory(winrt::com_ptr<ID3D11Device> const& device)
    {
        WINRT_ASSERT(device);

        winrt::com_ptr<IDXGIDevice> const dxdevice{ device.as<IDXGIDevice>() };

        winrt::com_ptr<IDXGIAdapter> adapter;
        winrt::check_hresult(dxdevice->GetAdapter(adapter.put()));

        winrt::com_ptr<IDXGIFactory2> factory;
        factory.capture(adapter, &IDXGIAdapter::GetParent);
        return factory;
    }

    void CreateDeviceSwapChainBitmap(
        winrt::com_ptr<IDXGISwapChain1> const& swapchain,
        winrt::com_ptr<ID2D1DeviceContext> const& target)
    {
        WINRT_ASSERT(swapchain);
        WINRT_ASSERT(target);

        winrt::com_ptr<IDXGISurface> surface;
        surface.capture(swapchain, &IDXGISwapChain1::GetBuffer, 0);

        D2D1_BITMAP_PROPERTIES1 const props{ D2D1::BitmapProperties1(
            D2D1_BITMAP_OPTIONS_TARGET | D2D1_BITMAP_OPTIONS_CANNOT_DRAW,
            D2D1::PixelFormat(DXGI_FORMAT_B8G8R8A8_UNORM, D2D1_ALPHA_MODE_IGNORE)) };

        winrt::com_ptr<ID2D1Bitmap1> bitmap;

        winrt::check_hresult(target->CreateBitmapFromDxgiSurface(surface.get(),
            props,
            bitmap.put()));

        target->SetTarget(bitmap.get());
    }

    winrt::com_ptr<IDXGISwapChain1> CreateSwapChainForCoreWindow(winrt::com_ptr<ID3D11Device> const& device)
    {
        WINRT_ASSERT(device);

        winrt::com_ptr<IDXGIFactory2> const factory{ GetDxgiFactory(device) };

        DXGI_SWAP_CHAIN_DESC1 props{};
        props.Format = DXGI_FORMAT_B8G8R8A8_UNORM;
        props.SampleDesc.Count = 1;
        props.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;
        props.BufferCount = 2;
        props.SwapEffect = DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL;

        winrt::com_ptr<IDXGISwapChain1> swapChain;

        winrt::check_hresult(factory->CreateSwapChainForCoreWindow(
            device.get(),
            winrt::get_unknown(CoreWindow::GetForCurrentThread()),
            &props,
            nullptr, // all or nothing
            swapChain.put()));

        return swapChain;
    }

    constexpr D2D1_COLOR_F color_white{ 1.0f,  1.0f,  1.0f,  1.0f };
    constexpr D2D1_COLOR_F color_orange{ 0.92f,  0.38f,  0.208f,  1.0f };
}

struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    winrt::com_ptr<ID2D1Factory1> m_factory;
    winrt::com_ptr<ID2D1DeviceContext> m_target;
    winrt::com_ptr<IDXGISwapChain1> m_swapChain;
    winrt::com_ptr<ID2D1SolidColorBrush> m_brush;
    float m_dpi{};

    IFrameworkView CreateView()
    {
        return *this;
    }

    void Initialize(CoreApplicationView const&)
    {
    }

    void Load(hstring const&)
    {
        CoreWindow const window{ CoreWindow::GetForCurrentThread() };

        window.SizeChanged([&](auto&&...)
        {
            if (m_target)
            {
                ResizeSwapChainBitmap();
                Render();
            }
        });

        DisplayInformation const display{ DisplayInformation::GetForCurrentView() };
        m_dpi = display.LogicalDpi();

        display.DpiChanged([&](DisplayInformation const& display, IInspectable const&)
        {
            if (m_target)
            {
                m_dpi = display.LogicalDpi();
                m_target->SetDpi(m_dpi, m_dpi);
                CreateDeviceSizeResources();
                Render();
            }
        });

        m_factory = CreateFactory();
        CreateDeviceIndependentResources();
    }

    void Uninitialize()
    {
    }

    void Run()
    {
        CoreWindow const window{ CoreWindow::GetForCurrentThread() };
        window.Activate();

        Render();
        CoreDispatcher const dispatcher{ window.Dispatcher() };
        dispatcher.ProcessEvents(CoreProcessEventsOption::ProcessUntilQuit);
    }

    void SetWindow(CoreWindow const&) {}

    void Draw()
    {
        m_target->Clear(color_white);

        D2D1_SIZE_F const size{ m_target->GetSize() };
        D2D1_RECT_F const rect{ 100.0f, 100.0f, size.width - 100.0f, size.height - 100.0f };
        m_target->DrawRectangle(rect, m_brush.get(), 100.0f);

        char buffer[1024];
        (void)snprintf(buffer, sizeof(buffer), "Draw %.2f x %.2f @ %.2f\n", size.width, size.height, m_dpi);
        ::OutputDebugStringA(buffer);
    }

    void Render()
    {
        if (!m_target)
        {
            winrt::com_ptr<ID3D11Device> const device{ CreateDevice() };
            m_target = CreateRenderTarget(m_factory, device);
            m_swapChain = CreateSwapChainForCoreWindow(device);

            CreateDeviceSwapChainBitmap(m_swapChain, m_target);

            m_target->SetDpi(m_dpi, m_dpi);

            CreateDeviceResources();
            CreateDeviceSizeResources();
        }

        m_target->BeginDraw();
        Draw();
        m_target->EndDraw();

        HRESULT const hr{ m_swapChain->Present(1, 0) };

        if (S_OK != hr && DXGI_STATUS_OCCLUDED != hr)
        {
            ReleaseDevice();
        }
    }

    void ReleaseDevice()
    {
        m_target = nullptr;
        m_swapChain = nullptr;

        ReleaseDeviceResources();
    }

    void ResizeSwapChainBitmap()
    {
        WINRT_ASSERT(m_target);
        WINRT_ASSERT(m_swapChain);

        m_target->SetTarget(nullptr);

        if (S_OK == m_swapChain->ResizeBuffers(0, // all buffers
            0, 0, // client area
            DXGI_FORMAT_UNKNOWN, // preserve format
            0)) // flags
        {
            CreateDeviceSwapChainBitmap(m_swapChain, m_target);
            CreateDeviceSizeResources();
        }
        else
        {
            ReleaseDevice();
        }
    }

    void CreateDeviceIndependentResources()
    {
    }

    void CreateDeviceResources()
    {
        winrt::check_hresult(m_target->CreateSolidColorBrush(
            color_orange,
            D2D1::BrushProperties(0.8f),
            m_brush.put()));
    }

    void CreateDeviceSizeResources()
    {
    }

    void ReleaseDeviceResources()
    {
        m_brush = nullptr;
    }
};

int __stdcall wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
{
    CoreApplication::Run(winrt::make<App>());
}
```

## <a name="working-with-com-types-such-as-bstr-and-variant"></a>使用 BSTR 和 VARIANT 等 COM 类型

可以看到，C++/WinRT 支持实现和调用 COM 接口。 若要使用 BSTR 和 VARIANT 等 COM 类型，我们建议使用 [Windows 实现库 (WIL)](https://github.com/Microsoft/wil) 提供的包装器，例如 **wil::unique_bstr** 和 **wil::unique_variant**（用于管理资源生存期）。

[WIL](https://github.com/Microsoft/wil) 取代了活动模板库 (ATL) 等框架，以及 Visual C++ 编译器的 COM 支持。 我们建议覆盖你自己的包装器，或者按原始格式使用 BSTR 和 VARIANT 等 COM 类型（结合相应的 API）。

## <a name="avoiding-namespace-collisions"></a>避免命名空间冲突

正如本主题中列出的代码所示，在 C++/WinRT 中，常见的做法是大量使用 using 指令。 但在某些情况下，这可能导致将冲突名称导入全局命名空间这种问题。 下面是一个示例。

C++/WinRT 包含名为 [**winrt::Windows::Foundation::IUnknown**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown) 的类型，而 COM 则定义名为 [ **::IUnknown**](/windows/desktop/api/unknwn/nn-unknwn-iunknown) 的类型。 那么，我们考虑一下，在使用 COM 标头的 C++/WinRT 项目中使用以下代码会出现什么情况。

```cppwinrt
using namespace winrt::Windows::Foundation;
...
void MyFunction(IUnknown*); // error C2872:  'IUnknown': ambiguous symbol
```

未限定的名称 *IUnknown* 在全局命名空间中发生冲突，因此会出现“符号不明确”编译器错误。  可以改将 C++/WinRT 版名称隔离到 **winrt** 命名空间中，如下所示。

```cppwinrt
namespace winrt
{
    using namespace Windows::Foundation;
}
...
void MyFunctionA(IUnknown*); // Ok.
void MyFunctionB(winrt::IUnknown const&); // Ok.
```

或者，如果你希望利用 `using namespace winrt`，那么就可以那样做。 只需对全局版 *IUnknown* 进行限定即可，如下所示。

```cppwinrt
using namespace winrt;
namespace winrt
{
    using namespace Windows::Foundation;
}
...
void MyFunctionA(::IUnknown*); // Ok.
void MyFunctionB(winrt::IUnknown const&); // Ok.
```

自然，这适用于任何 C++/WinRT 命名空间。

```cppwinrt
namespace winrt
{
    using namespace Windows::Storage;
    using namespace Windows::System;
}
```

然后，你可以引用 **winrt::Windows::Storage::StorageFile**。例如，直接以 **winrt::StorageFile** 形式引用。

## <a name="important-apis"></a>重要的 API
* [winrt::check_hresult 函数](/uwp/cpp-ref-for-winrt/error-handling/check-hresult)
* [winrt::com_ptr 结构模板](/uwp/cpp-ref-for-winrt/com-ptr)
* [winrt::Windows::Foundation::IUnknown 结构](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)
