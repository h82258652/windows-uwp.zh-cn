---
description: 本主题使用完整的 Direct2D 代码示例，介绍如何使用 C + + /winrt 来使用 COM 类和接口。
title: 通过 C++/WinRT 使用 COM 组件
ms.date: 07/23/2018
ms.topic: article
keywords: windows 10，uwp，标准，c + +，cpp，winrt，COM、 组件、 类、 接口
ms.localizationpriority: medium
ms.openlocfilehash: 9000cad79e12a645689d90ef37a8ff43b9fc95b7
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/07/2018
ms.locfileid: "8808602"
---
# <a name="consume-com-components-with-cwinrt"></a>通过 C++/WinRT 使用 COM 组件

你可以使用的功能[C + + WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)库以使用 COM 组件，如高性能的 2d 和 3d 图形的 DirectX Api。 C + + WinRT 最简单方法是使用 DirectX 而不影响性能。 本主题使用 Direct2D 代码示例显示了如何使用 C + + /winrt 来使用 COM 类和接口。 当然，可以混用 COM 和 Windows 运行时编程中的相同的 C + + WinRT 项目。

在本主题末尾，你会发现的最小的 Direct2D 应用程序的完整源代码列表。 我们将抬起摘录该代码，并使用它们来演示如何使用 COM 组件使用 C + + WinRT 使用各种功能的 C + + /winrt 库。

## <a name="com-smart-pointers-winrtcomptruwpcpp-ref-for-winrtcom-ptr"></a>COM 智能指针 ([**winrt:: com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr))

当你计划使用 COM 时，您工作直接与接口，而不是与对象 （这也在幕后适用于 Windows 运行时 Api，它们是 COM 的进化 true）。 COM 类上调用的函数，例如，你激活的类，获取回来，接口，然后在该接口上调用函数。 若要访问对象的状态，不要直接调用访问其数据成员相反，你可以在接口上调用访问器和转变器函数。

若要更具体，我们谈与接口*指针*交互。 为此，我们受益于是否存在 COM 智能指针类型在 C + + WinRT&mdash; [**winrt:: com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr)类型。

```cppwinrt
winrt::com_ptr<ID2D1Factory1> factory;
```

上面的代码显示了如何声明为[**ID2D1Factory1**](https://msdn.microsoft.com/library/Hh404596) COM 接口的未初始化智能指针。 智能指针是未初始化，，因此它尚未指向属于任何实际对象 （它没有指向接口根本） **ID2D1Factory1**接口。 但它可能会执行此操作;并且 （正在智能指针） 可以通过 COM 引用计数管理的接口，它指向拥有对象的生存期和要依据调用的函数时该接口的媒体的功能。

## <a name="com-functions-that-return-an-interface-pointer-as-void"></a>返回为**void**接口指针的 COM 函数

你可以调用[**put_void**](/uwp/cpp-ref-for-winrt/com-ptr#comptrputvoid-function)函数以写入到未初始化智能指针的基础原始指针。

```cppwinrt
D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    __uuidof(factory),
    &options,
    factory.put_void()
);
```

上面的代码调用[**D2D1CreateFactory**](/windows/desktop/api/d2d1/nf-d2d1-d2d1createfactory)函数，它通过其最后一个参数，已返回**ID2D1Factory1**接口指针**void\ * \ *** 类型。 许多 COM 函数将返回**void\ * \ ***。 对于此类函数中，使用[**put_void**](/uwp/cpp-ref-for-winrt/com-ptr#comptrputvoid-function)所示。

## <a name="com-functions-that-return-a-specific-interface-pointer"></a>返回一个特定接口指针的 COM 函数

通过其 antepenultimate 参数，具有[**D3D11CreateDevice**](/windows/desktop/api/dwrite/nf-dwrite-dwritecreatefactory)函数返回的[**ID3D11Device**](https://msdn.microsoft.com/library/Hh404596)接口指针**ID3D11Device\ * \ *** 类型。 对于这样返回一个特定接口指针的函数，使用[**com_ptr:: put**](/uwp/cpp-ref-for-winrt/com-ptr#comptrput-function)。

```cppwinrt
winrt::com_ptr<ID3D11Device> device;
D3D11CreateDevice(
    ...
    device.put(),
    ...);
```

在此之前部分中的代码示例演示如何调用原始**D2D1CreateFactory**函数。 但实际上，当本主题的代码示例调用**D2D1CreateFactory**，它使用包装原始的 API，帮助程序函数模板，因此该代码示例实际使用[**com_ptr:: put**](/uwp/cpp-ref-for-winrt/com-ptr#comptrput-function)。

```cppwinrt
winrt::com_ptr<ID2D1Factory1> factory;
D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    options,
    factory.put());
```

## <a name="com-functions-that-return-an-interface-pointer-as-iunknown"></a>返回作为**IUnknown**接口指针的 COM 函数

[**DWriteCreateFactory**](/windows/desktop/api/dwrite/nf-dwrite-dwritecreatefactory)函数返回其最后一个参数，具有[**IUnknown**](https://msdn.microsoft.com/library/windows/desktop/ms680509)类型通过 DirectWrite 工厂接口指针。 对于此类函数，使用[**com_ptr:: put**](/uwp/cpp-ref-for-winrt/com-ptr#comptrput-function)，但重新解释强制转换，为**IUnknown**。

```cppwinrt
DWriteCreateFactory(
    DWRITE_FACTORY_TYPE_SHARED,
    __uuidof(dwriteFactory2),
    reinterpret_cast<IUnknown**>(dwriteFactory2.put()));
```

## <a name="re-seat-a-winrtcomptr"></a>重新席位**winrt:: com_ptr**

> [!IMPORTANT]
> 如果你拥有已固定[**winrt:: com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr) （其内部的原始指针已经有一个目标） 和你想要重新席位它为指向一个不同的对象，然后你需要先为分配`nullptr`对其&mdash;下面的代码示例中所示。 如果不这样做，然后已固定**com_ptr**将绘制问题 （当你调用[**com_ptr:: put**](/uwp/cpp-ref-for-winrt/com-ptr#comptrput-function)或[**put_void**](/uwp/cpp-ref-for-winrt/com-ptr#comptrputvoid-function)） 你注意到通过断言其内部指针不为 null。

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

若要检查从 COM 函数，返回的 HRESULT 值和抛出异常的错误代码，它表示，调用[**winrt:: check_hresult**](/uwp/cpp-ref-for-winrt/error-handling/check-hresult)。

```cppwinrt
winrt::check_hresult(D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    __uuidof(factory),
    options,
    factory.put_void()));
```

## <a name="com-functions-that-take-a-specific-interface-pointer"></a>执行特定的接口指针的 COM 函数

你可以调用[**com_ptr:: get**](/uwp/cpp-ref-for-winrt/com-ptr#comptrget-function)函数将**com_ptr**你传递到采用相同类型的特定接口指针的函数。

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

## <a name="com-functions-that-take-an-iunknown-interface-pointer"></a>**IUnknown**接口指针的 COM 函数

你可以调用[**winrt::get_unknown**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#getunknown-function)自由函数，将你**com_ptr**传递到采用**IUnknown**接口指针的函数。

```cppwinrt
winrt::check_hresult(factory->CreateSwapChainForCoreWindow(
    ...
    winrt::get_unknown(CoreWindow::GetForCurrentThread()),
    ...));
```

## <a name="passing-and-returning-com-smart-pointers"></a>传递和返回 COM 智能指针

采取的**winrt:: com_ptr**形式的 COM 智能指针的函数由常量引用，或参考应执行此操作。

```cppwinrt
... GetDxgiFactory(winrt::com_ptr<ID3D11Device> const& device) ...

... CreateDevice(..., winrt::com_ptr<ID3D11Device>& device) ...
```

返回**winrt:: com_ptr**的函数应该执行此操作的值。

```cppwinrt
winrt::com_ptr<ID2D1Factory1> CreateFactory() ...
```

## <a name="query-a-com-smart-pointer-for-a-different-interface"></a>查询使用不同的接口的 COM 智能指针

你可以使用[**com_ptr:: as**](/uwp/cpp-ref-for-winrt/com-ptr#comptras-function)函数来查询其他接口的 COM 智能指针。 如果查询不成功，该函数会引发异常。

```cppwinrt
void ExampleFunction(winrt::com_ptr<ID3D11Device> const& device)
{
    ...
    winrt::com_ptr<IDXGIDevice> const dxdevice{ device.as<IDXGIDevice>() };
    ...
}
```

或者，使用[**com_ptr::try_as**](/uwp/cpp-ref-for-winrt/com-ptr#comptrtryas-function)，从而返回一个值，你可以检查`nullptr`以查看是否已成功查询。

## <a name="full-source-code-listing-of-a-minimal-direct2d-application"></a>最小的 Direct2D 应用程序的完整源代码列表

如果你想要生成并运行此源的代码示例然后第一，在 Visual Studio 中，创建一个新**核心应用 (C + + WinRT)**。 `Direct2D` 合理的项目名称，但你可以你喜欢的任何对其进行命名。 打开`App.cpp`，删除其整个内容，并粘贴以下列表中。

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
        winrt::check_hresult(adapter->GetParent(__uuidof(factory), factory.put_void()));
        return factory;
    }

    void CreateDeviceSwapChainBitmap(
        winrt::com_ptr<IDXGISwapChain1> const& swapchain,
        winrt::com_ptr<ID2D1DeviceContext> const& target)
    {
        WINRT_ASSERT(swapchain);
        WINRT_ASSERT(target);

        winrt::com_ptr<IDXGISurface> surface;
        winrt::check_hresult(swapchain->GetBuffer(0, __uuidof(surface), surface.put_void()));

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

        WINRT_TRACE("Draw %.2f x %.2f @ %.2f\n", size.width, size.height, m_dpi);
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
    CoreApplication::Run(App());
}
```

## <a name="working-with-com-types-such-as-bstr-and-variant"></a>使用 COM 类型，如 BSTR 和变体

如你所见，C + + /winrt 提供支持实现和调用 COM 接口。 对于使用 COM 类型，如 BSTR 和变体，是始终在其原始形式 （以及相应的 Api) 中使用它们的选项。 或者，你可以使用包装器由[活动模板库 (ATL)](/cpp/atl/active-template-library-atl-concepts)，如框架或提供的 Visual c + + 编译器的[COM 支持](/cpp/cpp/compiler-com-support)，即使你自己的包装器。

## <a name="important-apis"></a>重要的 API
* [winrt::check_hresult 函数](/uwp/cpp-ref-for-winrt/error-handling/check-hresult)
* [winrt::com_ptr 结构模板](/uwp/cpp-ref-for-winrt/com-ptr)
* [winrt::Windows::Foundation::IUnknown 结构](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)
