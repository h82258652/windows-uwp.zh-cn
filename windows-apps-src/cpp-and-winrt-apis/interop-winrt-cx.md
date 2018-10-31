---
author: stevewhims
description: 本主题介绍了可用于在 C++/CX 和 C++/WinRT 对象之间转换的两个帮助程序函数。
title: 实现 C++/WinRT 与 C++/CX 之间的互操作
ms.author: stwhi
ms.date: 10/09/2018
ms.topic: article
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, 端口, 迁移, 互操作, C++/CX
ms.localizationpriority: medium
ms.openlocfilehash: ca3cc69065ef2898aebafc832da1639985231d60
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2018
ms.locfileid: "5835802"
---
# <a name="interop-between-cwinrt-and-ccx"></a>实现 C++/WinRT 与 C++/CX 之间的互操作

有关逐渐将移植的代码中的策略您[C + + CX](/cpp/cppcx/visual-c-language-reference-c-cx)项目[C + + WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)中讨论[移动到 C + + WinRT 从 C + + CX](move-to-winrt-from-cx.md)。

本主题介绍了两个帮助程序函数，可用于转换为 C + + /CX 和 C + + /winrt 对象在同一项目中的。 你可以使用它们将使用两个语言投影，代码之间的互操作，或者你可以使用函数移植你的代码从 C + + CX 到 C + + WinRT。

## <a name="fromcx-and-tocx-functions"></a>from_cx 和 to_cx 函数
下面的帮助程序函数将 C++/CX 对象转换为等效的 C++/WinRT 对象。 该函数将 C++/CX 对象强制转换为其基础 [**IUnknown**](https://msdn.microsoft.com/library/windows/desktop/ms680509) 接口指针。 然后，它对该指针调用 [**QueryInterface**](https://msdn.microsoft.com/library/windows/desktop/ms682521) 来查询 C++/WinRT 对象的默认接口。 **QueryInterface** 是 C++/CX safe_cast 扩展的 Windows 运行时应用程序二进制接口 (ABI) 等效项。 此外，[**winrt::put_abi**](/uwp/cpp-ref-for-winrt/put-abi) 函数将检索 C++/WinRT 对象的基础 **IUnknown** 接口指针的地址，使该地址能够设置为其他值。

```cppwinrt
template <typename T>
T from_cx(Platform::Object^ from)
{
    T to{ nullptr };

    winrt::check_hresult(reinterpret_cast<::IUnknown*>(from)
        ->QueryInterface(winrt::guid_of<T>(),
            reinterpret_cast<void**>(winrt::put_abi(to))));

    return to;
}
```

下面的帮助程序函数将 C++/WinRT 对象转换为等效的 C++/CX 对象。 [**winrt::get_abi**](/uwp/cpp-ref-for-winrt/get-abi) 函数将检索指向 C++/WinRT 对象的基础 **IUnknown** 接口的指针。 在使用 C++/CX safe_cast 扩展查询请求的 C++/CX 类型之前，此函数会将该指针强制转换为 C++/CX 对象。

```cppwinrt
template <typename T>
T^ to_cx(winrt::Windows::Foundation::IUnknown const& from)
{
    return safe_cast<T^>(reinterpret_cast<Platform::Object^>(winrt::get_abi(from)));
}
```

## <a name="example-project-showing-the-two-helper-functions-in-use"></a>显示两个帮助程序函数中使用的示例项目

要重现，简单的方法，方案逐渐移植的代码中的 C + + CX 项目到 C + + WinRT，可以首先创建一个新项目在 Visual Studio 中使用 C + + WinRT 项目模板 (请参阅[Visual Studio 支持 C + + WinRT以及 VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix))。

此示例项目还说明了方式则可以使用命名空间别名为不同岛的代码中，以便处理潜在的命名空间冲突之间 C + + WinRT 投影和 C + + /CX 投影。

- 创建**Visual c + +** \> **Windows 通用** > **核心应用 (C + + WinRT)** 项目。
- 在项目属性中， **C/c + +** \> **常规** \> **使用 Windows 运行时扩展** \> **是 (/ZW)**。 这将打开项目支持 C + + CX。
- 内容替换为`App.cpp`使用下面的代码列表。

```cppwinrt
// App.cpp
#include "pch.h"
#include <sstream>

namespace cx
{
    using namespace Windows::Foundation;
}

namespace winrt
{
    using namespace Windows;
    using namespace Windows::ApplicationModel::Core;
    using namespace Windows::Foundation;
    using namespace Windows::Foundation::Numerics;
    using namespace Windows::UI;
    using namespace Windows::UI::Core;
    using namespace Windows::UI::Composition;
}

template <typename T>
T from_cx(Platform::Object^ from)
{
    T to{ nullptr };

    winrt::check_hresult(reinterpret_cast<::IUnknown*>(from)
        ->QueryInterface(winrt::guid_of<T>(),
            reinterpret_cast<void**>(winrt::put_abi(to))));

    return to;
}

template <typename T>
T^ to_cx(winrt::Windows::Foundation::IUnknown const& from)
{
    return safe_cast<T^>(reinterpret_cast<Platform::Object^>(winrt::get_abi(from)));
}

struct App : winrt::implements<App, winrt::IFrameworkViewSource, winrt::IFrameworkView>
{
    winrt::CompositionTarget m_target{ nullptr };
    winrt::VisualCollection m_visuals{ nullptr };
    winrt::Visual m_selected{ nullptr };
    winrt::float2 m_offset{};

    winrt::IFrameworkView CreateView()
    {
        return *this;
    }

    void Initialize(winrt::CoreApplicationView const &)
    {
    }

    void Load(winrt::hstring const&)
    {
    }

    void Uninitialize()
    {
    }

    void Run()
    {
        winrt::CoreWindow window = winrt::CoreWindow::GetForCurrentThread();
        window.Activate();

        winrt::CoreDispatcher dispatcher = window.Dispatcher();
        dispatcher.ProcessEvents(winrt::CoreProcessEventsOption::ProcessUntilQuit);
    }

    void SetWindow(winrt::CoreWindow const & window)
    {
        winrt::Compositor compositor;
        winrt::ContainerVisual root = compositor.CreateContainerVisual();
        m_target = compositor.CreateTargetForCurrentView();
        m_target.Root(root);
        m_visuals = root.Children();

        window.PointerPressed({ this, &App::OnPointerPressed });
        window.PointerMoved({ this, &App::OnPointerMoved });

        window.PointerReleased([&](auto && ...)
        {
            m_selected = nullptr;
        });
    }

    void OnPointerPressed(IInspectable const &, winrt::PointerEventArgs const & args)
    {
        winrt::float2 const point = args.CurrentPoint().Position();

        for (winrt::Visual visual : m_visuals)
        {
            winrt::float3 const offset = visual.Offset();
            winrt::float2 const size = visual.Size();

            if (point.x >= offset.x &&
                point.x < offset.x + size.x &&
                point.y >= offset.y &&
                point.y < offset.y + size.y)
            {
                m_selected = visual;
                m_offset.x = offset.x - point.x;
                m_offset.y = offset.y - point.y;
            }
        }

        if (m_selected)
        {
            m_visuals.Remove(m_selected);
            m_visuals.InsertAtTop(m_selected);
        }
        else
        {
            AddVisual(point);
        }
    }

    void OnPointerMoved(IInspectable const &, winrt::PointerEventArgs const & args)
    {
        if (m_selected)
        {
            winrt::float2 const point = args.CurrentPoint().Position();

            m_selected.Offset(
            {
                point.x + m_offset.x,
                point.y + m_offset.y,
                0.0f
            });
        }
    }

    void AddVisual(winrt::float2 const point)
    {
        winrt::Compositor compositor = m_visuals.Compositor();
        winrt::SpriteVisual visual = compositor.CreateSpriteVisual();

        static winrt::Color colors[] =
        {
            { 0xDC, 0x5B, 0x9B, 0xD5 },
            { 0xDC, 0xED, 0x7D, 0x31 },
            { 0xDC, 0x70, 0xAD, 0x47 },
            { 0xDC, 0xFF, 0xC0, 0x00 }
        };

        static unsigned last = 0;
        unsigned const next = ++last % _countof(colors);
        visual.Brush(compositor.CreateColorBrush(colors[next]));

        float const BlockSize = 100.0f;

        visual.Size(
        {
            BlockSize,
            BlockSize
        });

        visual.Offset(
        {
            point.x - BlockSize / 2.0f,
            point.y - BlockSize / 2.0f,
            0.0f,
        });

        m_visuals.InsertAtTop(visual);

        m_selected = visual;
        m_offset.x = -BlockSize / 2.0f;
        m_offset.y = -BlockSize / 2.0f;
    }
};

int __stdcall wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
{
    winrt::init_apartment();

    winrt::Uri uri(L"http://aka.ms/cppwinrt");
    std::wstringstream wstringstream;
    wstringstream << L"C++/WinRT: " << uri.Domain().c_str() << std::endl;

    // Convert from a C++/WinRT type to a C++/CX type.
    cx::Uri^ cx = to_cx<cx::Uri>(uri);
    wstringstream << L"C++/CX: " << cx->Domain->Data() << std::endl;
    ::OutputDebugString(wstringstream.str().c_str());

    // Convert from a C++/CX type to a C++/WinRT type.
    winrt::Uri uri_from_cx = from_cx<winrt::Uri>(cx);
    WINRT_ASSERT(uri.Domain() == uri_from_cx.Domain());
    WINRT_ASSERT(uri == uri_from_cx);

    winrt::CoreApplication::Run(winrt::make<App>());
}
```

## <a name="important-apis"></a>重要的 API
* [IUnknown 接口](https://msdn.microsoft.com/library/windows/desktop/ms680509)
* [QueryInterface 函数](https://msdn.microsoft.com/library/windows/desktop/ms682521)
* [winrt::get_abi 函数](/uwp/cpp-ref-for-winrt/get-abi)
* [winrt::put_abi 函数](/uwp/cpp-ref-for-winrt/put-abi)

## <a name="related-topics"></a>相关主题
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [从 C++/CX 移动到 C++/WinRT](move-to-winrt-from-cx.md)
