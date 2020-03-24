---
description: 本主题介绍如何将 WRL 代码移植到 C++/WinRT 中的等效项。
title: 从 WRL 移动到 C++/WinRT
ms.date: 05/30/2018
ms.topic: article
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, 端口, 迁移, WRL
ms.localizationpriority: medium
ms.openlocfilehash: 663e0dddb9823e35e2d31ba9667bd3d16bd1d5de
ms.sourcegitcommit: cab95379459ad378163aa4469c9dc6c509cc8c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/18/2020
ms.locfileid: "79510990"
---
# <a name="move-to-cwinrt-from-wrl"></a>从 WRL 移动到 C++/WinRT
本主题介绍了如何将 [Windows 运行时 C++ 模板库 (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl) 代码移植到 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 中的等效项。

移植到 C++/WinRT 的第一步是向项目手动添加 C++/WinRT 支持（请参阅[针对 C++/WinRT 的 Visual Studio 支持](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)）。 为此，请在项目中安装 [Microsoft.Windows.CppWinRT NuGet 包](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/)。 在 Visual Studio 中打开项目，然后单击“项目”\>“管理 NuGet 包...”   \>“浏览”，在搜索框中键入或粘贴 Microsoft.Windows.CppWinRT  ，在搜索结果中选择该项，然后单击“安装”以安装该项目的包。   这一更改的一个效果是对 [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) 的支持在项目中关闭。 如果你在项目中使用 C++/CX，那么你可以让支持保持关闭状态，同时将 C++/CX 代码更新到 C++/WinRT（请参阅[从 C++/CX 移动到 C++/WinRT](move-to-winrt-from-cx.md)）。 或者可以重新启用支持（在项目属性中，“C/C++”  \>“常规”  \>“使用 Windows 运行时扩展”  \>  “是(/ZW)”），并首先关注移植 WRL 代码。 C++/CX 和 C++/WinRT 代码可以在同一个项目中共存，但是 XAML 编译器支持和 Windows 运行时组件除外（请参阅[从 C++/CX 移动到 C++/WinRT](move-to-winrt-from-cx.md)）。

将项目属性“常规”  \>  “目标平台版本”设置为 10.0.17134.0（Windows 10 版本 1803）或更高版本。

在预编译的标头文件（通常为 `pch.h`）中，包括 `winrt/base.h`。

```cppwinrt
#include <winrt/base.h>
```

如果你包括了任何 C++/WinRT 投影 Windows API 标头（例如，`winrt/Windows.Foundation.h`），那么你无需像这样明确包括 `winrt/base.h`，因为它将自动为你包含在内。

## <a name="porting-wrl-com-smart-pointers-microsoftwrlcomptr"></a>移植 WRL COM 智能指针 ([Microsoft::WRL::ComPtr](/cpp/windows/comptr-class))
移植任何使用 **Microsoft::WRL::ComPtr\<T\>** 的代码以使用 [**winrt::com_ptr\<T\>** ](/uwp/cpp-ref-for-winrt/com-ptr)。 下面是之前和之后的代码示例。 在*之后*的版本中，[**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput-function) 成员函数检索基础原始指针，以便可以进行设置。

```cpp
ComPtr<IDXGIAdapter1> previousDefaultAdapter;
DX::ThrowIfFailed(m_dxgiFactory->EnumAdapters1(0, &previousDefaultAdapter));
```

```cppwinrt
winrt::com_ptr<IDXGIAdapter1> previousDefaultAdapter;
winrt::check_hresult(m_dxgiFactory->EnumAdapters1(0, previousDefaultAdapter.put()));
```

> [!IMPORTANT]
> 如果某个 [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr) 已定位（其内部原始指针已有目标），而你想要将其重新定位为指向不同的对象，则首先需要向其分配 `nullptr`&mdash; 如以下代码示例所示。 否则，已定位的 **com_ptr** 会断言其内部指针不为 null，从而产生需要关注的问题（调用 [**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput-function) 或 [**com_ptr::put_void**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput_void-function) 时）。

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

在下一个示例中（*之后*版本），[**com_ptr::put_void**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput_void-function) 成员函数检索作为要避免的指针的基础原始指针。

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

将 **ComPtr::Get** 替换为 [**com_ptr::get**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrget-function)。

```cpp
m_d3dDevice->CreateDepthStencilView(m_depthStencil.Get(), &dsvDesc, m_dsvHeap->GetCPUDescriptorHandleForHeapStart());
```

```cppwinrt
m_d3dDevice->CreateDepthStencilView(m_depthStencil.get(), &dsvDesc, m_dsvHeap->GetCPUDescriptorHandleForHeapStart());
```

当你想要将基础原始指针传递到预期 **IUnknown** 指针的函数时，应使用 [**winrt::get_unknown**](/uwp/cpp-ref-for-winrt/get-unknown) 自由函数，如下一个示例所示。

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
可以将 C++/WinRT 代码逐渐添加到使用 WRL 来实现组件的现有项目，现有的 WRL 类会继续受支持。 此部分显示如何操作。

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
