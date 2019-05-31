---
description: 本主题通过一个完整的 Direct2D 代码示例展示了如何通过 C++/WinRT 来使用 COM 类和接口。
title: 通过 C++/WinRT 使用 COM 组件
ms.date: 04/24/2019
ms.topic: article
keywords: windows 10、 uwp、 标准版、 c + +、 cpp、 winrt、 COM、 组件、 类、 接口
ms.localizationpriority: medium
ms.openlocfilehash: 2c36c7b896b4d08240f08e85570110b45e0a9f3c
ms.sourcegitcommit: ba24a6237355119ef3e36687417f390c8722bd67
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66421257"
---
# <a name="consume-com-components-with-cwinrt"></a>通过 C++/WinRT 使用 COM 组件

可以使用的设施[ C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)库以使用 COM 组件，如高性能 2d 和 3d 图形的 DirectX Api。 C++/ WinRT 是使用 DirectX，而不会影响性能的最简单方法。 本主题使用 Direct2D 的代码示例来演示如何使用C++/WinRT 以使用 COM 类和接口。 当然，可以混合使用 COM 和 Windows 运行时编程的相同的 C + WinRT 项目。

在本主题的末尾，您会发现最小的 Direct2D 应用程序的完整源代码代码的列表。 我们将提升部分内容摘自该代码并使用它们来演示了如何使用 COM 组件使用C++/WinRT，使用各种工具的C++/WinRT 库。

## <a name="com-smart-pointers-winrtcomptruwpcpp-ref-for-winrtcom-ptr"></a>COM 智能指针 ([**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr))

当程序使用 COM 时，可直接使用接口而不是使用对象 （对应还在后台为 Windows 运行时 Api，这是 COM 演变而来，则返回 true）。 若要调用的 COM 类的一个函数，例如，您激活类，获取一个接口，，然后在该接口上调用函数。 若要访问对象的状态，不直接调用访问其数据成员相反，您可以在接口上调用取值函数和赋值函数的函数。

若要更具体，我们在说什么接口与之进行交互*指针*。 并且为此，我们受益于 COM 智能指针类型中是否存在C++/WinRT&mdash; [ **winrt::com_ptr** ](/uwp/cpp-ref-for-winrt/com-ptr)类型。

```cppwinrt
#include <d2d1_1.h>
...
winrt::com_ptr<ID2D1Factory1> factory;
```

上面的代码演示如何声明指向未初始化智能指针[ **ID2D1Factory1** ](https://docs.microsoft.com/windows/desktop/api/d2d1_1/nn-d2d1_1-id2d1factory1) COM 接口。 智能指针是未初始化，因此尚不支持指向**ID2D1Factory1**属于 （它不指向接口根本） 任何实际对象的接口。 但它有可能如此操作。它具有通过 COM 引用计数来管理它指向的接口的所属对象的生存期和要按其调用的函数时该接口的介质的功能 （在智能指针）。

## <a name="com-functions-that-return-an-interface-pointer-as-void"></a>返回为一个接口指针的 COM 函数**void**

您可以调用[ **com_ptr::put_void** ](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput_void-function)要写入到未初始化的智能指针的函数的基本原始指针。

```cppwinrt
D2D1_FACTORY_OPTIONS options{ D2D1_DEBUG_LEVEL_NONE };
D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    __uuidof(factory),
    &options,
    factory.put_void()
);
```

调用上面的代码[ **D2D1CreateFactory** ](/windows/desktop/api/d2d1/nf-d2d1-d2d1createfactory)函数，它将返回**ID2D1Factory1**通过具有其最后一个参数的接口指针**void\* \*** 类型。 许多 COM 函数会返回**void\*\*** 。 对于此类函数中，使用[ **com_ptr::put_void** ](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput_void-function)所示。

## <a name="com-functions-that-return-a-specific-interface-pointer"></a>返回特定的接口指针的 COM 函数

[ **D3D11CreateDevice** ](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice)函数将返回[ **ID3D11Device** ](/windows/desktop/api/d3d11/nn-d3d11-id3d11device)通过其第三从最后一个参数，它具有的接口指针**ID3D11Device\* \*** 类型。 对于类似的返回特定的接口指针的函数，使用[ **com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptr_put-function)。

```cppwinrt
winrt::com_ptr<ID3D11Device> device;
D3D11CreateDevice(
    ...
    device.put(),
    ...);
```

此演示了如何调用原始之前的部分中的代码示例**D2D1CreateFactory**函数。 但事实上，在本主题的代码示例调用**D2D1CreateFactory**，它使用可包装原始 API 中，一个帮助程序函数模板，因此该代码示例实际上使用[ **com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptr_put-function).

```cppwinrt
winrt::com_ptr<ID2D1Factory1> factory;
D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    options,
    factory.put());
```

## <a name="com-functions-that-return-an-interface-pointer-as-iunknown"></a>返回为一个接口指针的 COM 函数**IUnknown**

[ **DWriteCreateFactory** ](/windows/desktop/api/dwrite/nf-dwrite-dwritecreatefactory)函数将返回其最后一个参数，已通过 DirectWrite 工厂接口指针[ **IUnknown** ](https://docs.microsoft.com/windows/desktop/api/unknwn/nn-unknwn-iunknown)类型。 对于此类函数中，使用[ **com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptr_put-function)，但重新解释强制转换到**IUnknown**。

```cppwinrt
DWriteCreateFactory(
    DWRITE_FACTORY_TYPE_SHARED,
    __uuidof(dwriteFactory2),
    reinterpret_cast<IUnknown**>(dwriteFactory2.put()));
```

## <a name="re-seat-a-winrtcomptr"></a>重新座位**winrt::com_ptr**

> [!IMPORTANT]
> 如果有[ **winrt::com_ptr** ](/uwp/cpp-ref-for-winrt/com-ptr)已装 （其内部的原始指针都已经有一个目标），并且想要重新固定，以指向不同的对象，则首先需要将分配`nullptr`向其&mdash;如下面的代码示例中所示。 如果不这样做，则已-就位**com_ptr**将绘制到你的关注的问题 (当您调用[ **com_ptr::put** ](/uwp/cpp-ref-for-winrt/com-ptr#com_ptr_put-function)或者[ **com_ptr::put_void**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput_void-function)) 通过声明其内部指针不为 null。

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

若要检查从 COM 函数返回的 HRESULT 值并引发异常，它表示一个错误代码，调用[ **winrt::check_hresult**](/uwp/cpp-ref-for-winrt/error-handling/check-hresult)。

```cppwinrt
winrt::check_hresult(D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    __uuidof(factory),
    options,
    factory.put_void()));
```

## <a name="com-functions-that-take-a-specific-interface-pointer"></a>将特定的接口指针的 COM 函数

您可以调用[ **com_ptr::get** ](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrget-function)函数来传递你**com_ptr**到采用相同的类型的特定接口指针的函数。

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

## <a name="com-functions-that-take-an-iunknown-interface-pointer"></a>COM 函数采用**IUnknown**接口指针

您可以调用[ **winrt::get_unknown** ](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#get_unknown-function)自由函数传递你**com_ptr**到该函数采用**IUnknown**接口指针。

```cppwinrt
winrt::check_hresult(factory->CreateSwapChainForCoreWindow(
    ...
    winrt::get_unknown(CoreWindow::GetForCurrentThread()),
    ...));
```

## <a name="passing-and-returning-com-smart-pointers"></a>传递和返回 COM 智能指针

中的窗体接受 COM 智能指针的函数**winrt::com_ptr**应执行此操作通过常量引用，或引用。

```cppwinrt
... GetDxgiFactory(winrt::com_ptr<ID3D11Device> const& device) ...

... CreateDevice(..., winrt::com_ptr<ID3D11Device>& device) ...
```

返回的函数**winrt::com_ptr**应执行此操作的值。

```cppwinrt
winrt::com_ptr<ID2D1Factory1> CreateFactory() ...
```

## <a name="query-a-com-smart-pointer-for-a-different-interface"></a>查询使用不同的接口的 COM 智能指针

可以使用[ **com_ptr::as** ](/uwp/cpp-ref-for-winrt/com-ptr#com_ptras-function)函数查询使用不同的接口的 COM 智能指针。 如果查询未成功，该函数将引发异常。

```cppwinrt
void ExampleFunction(winrt::com_ptr<ID3D11Device> const& device)
{
    ...
    winrt::com_ptr<IDXGIDevice> const dxdevice{ device.as<IDXGIDevice>() };
    ...
}
```

或者，使用[ **com_ptr::try_as**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrtry_as-function)，这会返回一个值，可以检查针对`nullptr`若要查看查询是否成功。

## <a name="full-source-code-listing-of-a-minimal-direct2d-application"></a>最小的 Direct2D 应用程序的完整源代码列表

如果你想要生成并运行此源的代码示例然后第一，在 Visual Studio 中，创建一个新**核心应用程序 (C++/WinRT)** 。 `Direct2D` 是一个合理的名称对于项目，但您可以命名为任何希望对其进行命名。 打开`App.cpp`，删除其整个内容，并粘贴下面的列表中。

以下代码使用[winrt::com_ptr::capture 函数](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrcapture-function)在可能的情况。

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

## <a name="working-with-com-types-such-as-bstr-and-variant"></a>使用 COM 类型，如 BSTR 和 VARIANT

正如您所看到的C++/WinRT 提供了实现和调用 COM 接口的支持。 使用 COM 类型，如 BSTR 和 VARIANT，我们建议使用提供的包装[Windows 实现库 (WIL)](https://github.com/Microsoft/wil)，如**wil::unique_bstr**和**wil::unique_变体**（管理资源生存期）。

[WIL](https://github.com/Microsoft/wil)取代了框架，例如活动模板库 (ATL) 和视觉对象C++编译器 COM 支持。 建议通过编写自己的包装器，或使用如 BSTR 和 variant 类型的值以原始形式 （以及相应的 Api) 的 COM 类型。

## <a name="important-apis"></a>重要的 API
* [winrt::check_hresult 函数](/uwp/cpp-ref-for-winrt/error-handling/check-hresult)
* [winrt::com_ptr 结构模板](/uwp/cpp-ref-for-winrt/com-ptr)
* [winrt::Windows::Foundation::IUnknown struct](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)
