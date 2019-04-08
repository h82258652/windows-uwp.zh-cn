---
description: 本主题介绍如何将 WRL 代码移植到 C++/WinRT 中的等效项。
title: 从 WRL 移动到 C++/WinRT
ms.date: 05/30/2018
ms.topic: article
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, 端口, 迁移, WRL
ms.localizationpriority: medium
ms.openlocfilehash: e81f82fe823ee0fdf81741c89576adf268940d91
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57630742"
---
# <a name="move-to-cwinrt-from-wrl"></a>从 WRL 移动到 C++/WinRT
本主题演示如何移植[Windows 运行时 c + + 模板库 (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl)的对等代码[C + + WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)。

第一步中移植到 C + + / WinRT 是手动添加 C + + WinRT 支持添加到你的项目 (请参阅[Visual Studio 支持 C + + WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package))。 为此，请安装[Microsoft.Windows.CppWinRT NuGet 包](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/)到你的项目。 打开 Visual Studio 项目中，单击**项目** \> **管理 NuGet 包...**\> **浏览**，键入或粘贴**Microsoft.Windows.CppWinRT**在搜索框中，在搜索结果中选择的项，然后单击**安装**若要安装该项目的包。 所做的更改的一个作用是支持[C + + /cli CX](/cpp/cppcx/visual-c-language-reference-c-cx)处于关闭状态的项目中。 如果你在项目中使用 C++/CX，那么你可以让支持保持关闭状态，同时将 C++/CX 代码更新到 C++/WinRT（请参阅[从 C++/CX 移动到 C++/WinRT](move-to-winrt-from-cx.md)）。 也可将支持返回 (在项目属性中， **C/c + +** \> **常规** \> **使用 Windows 运行时扩展** \>**是 (/ZW)**)，并移植 WRL 代码的第一个重点。 C + + /CX 和 C + + WinRT 代码可以共存于同一项目中，除 XAML 编译器支持和 Windows 运行时组件 (请参阅[将移动到 C + + WinRT 从 C + + /cli CX](move-to-winrt-from-cx.md))。

设置项目属性**常规** \> **目标平台版本**到 10.0.17134.0 (Windows 10，版本 1803年) 或更高版本。

在预编译的标头文件（通常为 `pch.h`）中，包括 `winrt/base.h`。

```cppwinrt
#include <winrt/base.h>
```

如果你包括了任何 C++/WinRT 投影 Windows API 标头（例如，`winrt/Windows.Foundation.h`），那么你无需像这样明确包括 `winrt/base.h`，因为它将自动为你包含在内。

## <a name="porting-wrl-com-smart-pointers-microsoftwrlcomptrcppwindowscomptr-class"></a>移植 WRL COM 智能指针 ([Microsoft::WRL::ComPtr](/cpp/windows/comptr-class))
使用任何代码移植**Microsoft::WRL::ComPtr\<T\>** 若要使用[ **winrt::com_ptr\<T\>**](/uwp/cpp-ref-for-winrt/com-ptr)。 下面是之前和之后的代码示例。 在*之后*的版本中，[**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#comptrput-function) 成员函数检索基础原始指针，以便可以进行设置。

```cpp
ComPtr<IDXGIAdapter1> previousDefaultAdapter;
DX::ThrowIfFailed(m_dxgiFactory->EnumAdapters1(0, &previousDefaultAdapter));
```

```cppwinrt
winrt::com_ptr<IDXGIAdapter1> previousDefaultAdapter;
winrt::check_hresult(m_dxgiFactory->EnumAdapters1(0, previousDefaultAdapter.put()));
```

> [!IMPORTANT]
> 如果有[ **winrt::com_ptr** ](/uwp/cpp-ref-for-winrt/com-ptr)已装 （其内部的原始指针都已经有一个目标），并且想要重新固定，以指向不同的对象，则首先需要将分配`nullptr`向其&mdash;如下面的代码示例中所示。 如果不这样做，则已-就位**com_ptr**将绘制到你的关注的问题 (当您调用[ **com_ptr::put** ](/uwp/cpp-ref-for-winrt/com-ptr#comptrput-function)或者[ **com_ptr::put_void**](/uwp/cpp-ref-for-winrt/com-ptr#comptrputvoid-function)) 通过声明其内部指针不为 null。

```cppwinrt
winrt::com_ptr<IDXGISwapChain1> m_pDXGISwapChain1;
...
// We execute the code below each time the window size changes.
m_pDXGISwapChain1 = nullptr; // Important because we're about to re-seat 
winrt::check_hresult(
    m_pDxgiFactory->CreateSwapChainForHwnd(
        m_pCommandQueue.get(), // For Direct3D 12, this is a pointer to a direct command queue, and not to the device.
        m_hWnd,
        &swapChainDesc,
        nullptr,
        nullptr,
        m_pDXGISwapChain1.put())
);
```

在下一个示例中（*之后*版本），[**com_ptr::put_void**](/uwp/cpp-ref-for-winrt/com-ptr#comptrputvoid-function) 成员函数检索作为要避免的指针的基础原始指针。

```cpp
ComPtr<ID3D12Debug> debugController;
if (SUCCEEDED(D3D12GetDebugInterface(IID_PPV_ARGS(&debugController))))
{
    debugController->EnableDebugLayer();
}
```

```cppwinrt
winrt::com_ptr<ID3D12Debug> debugController;
if (SUCCEEDED(D3D12GetDebugInterface(__uuidof(debugController), debugController.put_void())))
{
    debugController->EnableDebugLayer();
}
```

将 **ComPtr::Get** 替换为 [**com_ptr::get**](/uwp/cpp-ref-for-winrt/com-ptr#comptrget-function)。

```cpp
m_d3dDevice->CreateDepthStencilView(m_depthStencil.Get(), &dsvDesc, m_dsvHeap->GetCPUDescriptorHandleForHeapStart());
```

```cppwinrt
m_d3dDevice->CreateDepthStencilView(m_depthStencil.get(), &dsvDesc, m_dsvHeap->GetCPUDescriptorHandleForHeapStart());
```

如果想要传递到需要一个指向指针的函数的基础的原始指针**IUnknown**，使用[ **winrt::get_unknown** ](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#getunknown-function)自由函数，此下一步中所示示例。

```cpp
ComPtr<IDXGISwapChain1> swapChain;
DX::ThrowIfFailed(
    m_dxgiFactory->CreateSwapChainForCoreWindow(
        m_commandQueue.Get(),
        reinterpret_cast<IUnknown*>(m_window.Get()),
        &swapChainDesc,
        nullptr,
        &swapChain
    )
);
```

```cppwinrt
winrt::agile_ref<winrt::Windows::UI::Core::CoreWindow> m_window; 
winrt::com_ptr<IDXGISwapChain1> swapChain;
winrt::check_hresult(
    m_dxgiFactory->CreateSwapChainForCoreWindow(
        m_commandQueue.get(),
        winrt::get_unknown(m_window.get()),
        &swapChainDesc,
        nullptr,
        swapChain.put()
    )
);
```

## <a name="porting-a-wrl-module-microsoftwrlmodule"></a>移植 WRL 模块 (Microsoft::WRL::Module)
你可以将 C++/WinRT 代码逐渐添加到使用 WRL 来实现组件的现有项目，你现有的 WRL 类会继续受支持。 此部分显示如何操作。

如果你在 Visual Studio 中创建一个新的 **Windows 运行时组件 (C++/WinRT)** 投影类型并生成，则将为你生成文件 `Generated Files\module.g.cpp`。 该文件包含两个有用的 C++/WinRT 函数的定义（下方已列出），你可以将其复制并添加到项目中。 这些函数是 **WINRT_CanUnloadNow** 和 **WINRT_GetActivationFactory**，如你所见，它们有条件地调用 WRL 以便为你提供支持，不论你正处于哪个移植阶段。

```cppwinrt
HRESULT WINRT_CALL WINRT_CanUnloadNow()
{
#ifdef _WRL_MODULE_H_
    if (!::Microsoft::WRL::Module<::Microsoft::WRL::InProc>::GetModule().Terminate())
    {
        return S_FALSE;
    }
#endif

    if (winrt::get_module_lock())
    {
        return S_FALSE;
    }

    winrt::clear_factory_cache();
    return S_OK;
}

HRESULT WINRT_CALL WINRT_GetActivationFactory(HSTRING classId, void** factory)
{
    try
    {
        *factory = nullptr;
        wchar_t const* const name = WINRT_WindowsGetStringRawBuffer(classId, nullptr);

        if (0 == wcscmp(name, L"MoveFromWRLTest.Class"))
        {
            *factory = winrt::detach_abi(winrt::make<winrt::MoveFromWRLTest::factory_implementation::Class>());
            return S_OK;
        }

#ifdef _WRL_MODULE_H_
        return ::Microsoft::WRL::Module<::Microsoft::WRL::InProc>::GetModule().GetActivationFactory(classId, reinterpret_cast<::IActivationFactory**>(factory));
#else
        return winrt::hresult_class_not_available().to_abi();
#endif
    }
    catch (...) { return winrt::to_hresult(); }
}
```

在项目中加入这些函数后，不要直接调用 [**Module::GetActivationFactory**](/cpp/windows/module-getactivationfactory-method)，而应调用 **WINRT_GetActivationFactory**（这内部调用 WRL 函数）。 下面是之前和之后的代码示例。

```cpp
HRESULT WINAPI DllGetActivationFactory(_In_ HSTRING activatableClassId, _Out_ ::IActivationFactory **factory)
{
    auto & module = Microsoft::WRL::Module<Microsoft::WRL::InProc>::GetModule();
    return module.GetActivationFactory(activatableClassId, factory);
}
```

```cppwinrt
HRESULT __stdcall WINRT_GetActivationFactory(HSTRING activatableClassId, void** factory);
HRESULT WINAPI DllGetActivationFactory(_In_ HSTRING activatableClassId, _Out_ ::IActivationFactory **factory)
{
    return WINRT_GetActivationFactory(activatableClassId, reinterpret_cast<void**>(factory));
}
```

不要直接调用 [**Module::Terminate**](/cpp/windows/module-terminate-method)，而应调用 **WINRT_CanUnloadNow**（这将内部调用 WRL 函数）。 下面是之前和之后的代码示例。

```cpp
HRESULT __stdcall DllCanUnloadNow(void)
{
    auto &module = Microsoft::WRL::Module<Microsoft::WRL::InProc>::GetModule();
    HRESULT hr = (module.Terminate() ? S_OK : S_FALSE);
    if (hr == S_OK)
    {
        hr = ...
    }
    return hr;
}
```

```cppwinrt
HRESULT __stdcall WINRT_CanUnloadNow();
HRESULT __stdcall DllCanUnloadNow(void)
{
    HRESULT hr = WINRT_CanUnloadNow();
    if (hr == S_OK)
    {
        hr = ...
    }
    return hr;
}
```

## <a name="important-apis"></a>重要的 API
* [winrt::com_ptr 结构模板](/uwp/cpp-ref-for-winrt/com-ptr)
* [winrt::Windows::Foundation::IUnknown 结构](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)

## <a name="related-topics"></a>相关主题
* [C++/WinRT 简介](intro-to-using-cpp-with-winrt.md)
* [从 C++/CX 移动到 C++/WinRT](move-to-winrt-from-cx.md)
* [Windows 运行时 C++ 模板库 (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl)
