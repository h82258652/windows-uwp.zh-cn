---
description: 本主题介绍了如何在应用程序二进制接口 (ABI) 和 C++/WinRT 对象之间进行转换。
title: 实现 C++/WinRT 与 ABI 之间的互操作
ms.date: 11/30/2018
ms.topic: article
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, 端口, 迁移, 互操作, ABI
ms.localizationpriority: medium
ms.openlocfilehash: 4249618a4b26fd7e8129547a679c80c5e2ed6903
ms.sourcegitcommit: a2b340dc3a28e845830eeb9ce00342a3f7351d62
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/02/2020
ms.locfileid: "85834995"
---
# <a name="interop-between-cwinrt-and-the-abi"></a>实现 C++/WinRT 与 ABI 之间的互操作

本主题介绍了如何在 SDK 应用程序二进制接口 (ABI) 和 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 对象之间转换。 你可以借助这些技术，为使用 Windows 运行时的这两种编程方式的代码实现互操作，也可以在将代码从 ABI 逐步迁移到 C++/WinRT 时使用这些技术。

一般情况下，C++/WinRT 将 ABI 类型公开为 **void\*** ，以便不需要包括平台头文件。

## <a name="what-is-the-windows-runtime-abi-and-what-are-abi-types"></a>什么是 Windows 运行时 ABI？什么是 ABI 类型？
Windows 运行时类（运行时类）实际上是一种抽象。 这种抽象定义了一个二进制接口（应用程序二进制接口，或 ABI），它允许各种编程语言与一个对象进行交互。 不管使用何种编程语言，客户端代码与 Windows 运行时对象的交互发生在最低级别，在此客户端语言构造被转换为对象的 ABI 调用。

文件夹“%WindowsSdkDir%Include\10.0.17134.0\winrt”（必要时根据情况调整 SDK 版本号）中的 Windows SDK 头文件是 Windows 运行时 ABI 头文件。 它们由 MIDL 编译器生成。 下面是包含这些标头之一的示例。

```cpp
#include <windows.foundation.h>
```

下面是你将在该特定 SDK 头文件发现的 ABI 类型之一的简化示例。 注意，ABI 命名空间、Windows::Foundation 和所有其他 Windows 命名空间由 ABI 命名空间中的 SDK 头文件声明。

```cpp
namespace ABI::Windows::Foundation
{
    IUriRuntimeClass : public IInspectable
    {
    public:
        /* [propget] */ virtual HRESULT STDMETHODCALLTYPE get_AbsoluteUri(/* [retval, out] */__RPC__deref_out_opt HSTRING * value) = 0;
        ...
    }
}
```

IUriRuntimeClass 是 COM 接口。 此外（由于它的基是 IInspectable），IUriRuntimeClass 还是 Windows 运行时接口。 请注意 HRESULT 返回类型，而不是异常的引发。 还有 HSTRING 句柄等项目的使用（最好在使用完该句柄后将其设置回 `nullptr`）。 这在应用程序二进制文件级别呈现了 Windows 运行时应有的样子；换句话说，在 COM 编程级别。

Windows 运行时基于组件对象模型 (COM) API。 你可以用那种方式访问 Windows 运行时，也可以通过语言投影访问它。 投影将隐藏 COM 详细信息，并为给定语言提供更自然的编程体验。

例如，如果你查看文件夹“%WindowsSdkDir%Include\10.0.17134.0\cppwinrt\winrt”（重复一下，必要时根据情况调整 SDK 版本号），就会发现 C++/WinRT 语言投影标头。 每个 Windows 命名空间都有一个标头，就像每个 Windows 命名空间都有一个 ABI 标头一样。 下面是包含 C++/WinRT 标头之一的示例。

```cppwinrt
#include <winrt/Windows.Foundation.h>
```

此外，在该标头中，这里（已简化）是我们刚才看到的 ABI 类型的 C++/WinRT 等效项。

```cppwinrt
namespace winrt::Windows::Foundation
{
    struct Uri : IUriRuntimeClass, ...
    {
        winrt::hstring AbsoluteUri() const { ... }
        ...
    };
}
```

此处的接口是新式标准 C++。 它去掉了 HRESULT（必要时，C++/WinRT 将引发异常）。 此外，访问器函数返回了一个简单字符串对象，该对象在其作用域的末端被清除。

本主题适用于希望与在应用程序二进制接口 (ABI) 层工作的代码进行互操作或进行移植的情况。

## <a name="converting-to-and-from-abi-types-in-code"></a>在代码中转换到/自 ABI 类型
为安全和简单起见，对于两个方向的转换，你都可以使用 [winrt::com_ptr](/uwp/cpp-ref-for-winrt/com-ptr)、[com_ptr::as](/uwp/cpp-ref-for-winrt/com-ptr#com_ptras-function) 和 [winrt::Windows::Foundation::IUnknown::as](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)。 下面是代码示例（基于控制台应用项目模板），该示例说明了如何使用不同岛的命名空间别名处理 C++/WinRT 投影与 ABI 之间潜在的命名空间冲突。

```cppwinrt
// pch.h
#pragma once
#include <windows.foundation.h>
#include <unknwn.h>
#include "winrt/Windows.Foundation.h"

// main.cpp
#include "pch.h"

namespace winrt
{
    using namespace Windows::Foundation;
}

namespace abi
{
    using namespace ABI::Windows::Foundation;
};

int main()
{
    winrt::init_apartment();

    winrt::Uri uri(L"http://aka.ms/cppwinrt");

    // Convert to an ABI type.
    winrt::com_ptr<abi::IStringable> ptr{ uri.as<abi::IStringable>() };

    // Convert from an ABI type.
    uri = ptr.as<winrt::Uri>();
    winrt::IStringable uriAsIStringable{ ptr.as<winrt::IStringable>() };
}
```

as 函数的实现调用了 [QueryInterface](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))。 如果你需要仅调用 [AddRef](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-addref) 的较低级别的转换，则可以使用 [winrt::copy_to_abi](/uwp/cpp-ref-for-winrt/copy-to-abi) 和 [winrt::copy_from_abi](/uwp/cpp-ref-for-winrt/copy-from-abi) 帮助程序函数。 后面这个代码示例向上面的代码示例添加了这些较低级别的转换。

> [!IMPORTANT]
> 当与 ABI 类型进行互操作时，所使用的 ABI 类型必须与 C++/WinRT 对象的默认接口对应。 否则，调用 ABI 类型上的方法实际上最终将调用默认接口上同一 vtable 槽中的方法，并且会出现意外结果。 请注意，[winrt::copy_to_abi](/uwp/cpp-ref-for-winrt/copy-from-abi) 不会在编译时对其加以保护，因为它对所有 ABI 类型使用 void\*，并假设调用方注意避免与类型不匹配 。 这是为了避免在可能永远不会使用 ABI 类型时要求 C++/WinRT 标头引用 ABI 标头。

```cppwinrt
int main()
{
    // The code in main() already shown above remains here.

    // Lower-level conversions that only call AddRef.

    // Convert to an ABI type.
    ptr = nullptr;
    winrt::copy_to_abi(uriAsIStringable, *ptr.put_void());

    // Convert from an ABI type.
    uri = nullptr;
    winrt::copy_from_abi(uriAsIStringable, ptr.get());
    ptr = nullptr;
}
```

下面同样是低级别转换方法，但使用了指向 ABI 接口类型（由 Windows SDK 标头定义）的原始指针。

```cppwinrt
    // The code in main() already shown above remains here.

    // Copy to an owning raw ABI pointer with copy_to_abi.
    abi::IStringable* owning{ nullptr };
    winrt::copy_to_abi(uriAsIStringable, *reinterpret_cast<void**>(&owning));

    // Copy from a raw ABI pointer.
    uri = nullptr;
    winrt::copy_from_abi(uriAsIStringable, owning);
    owning->Release();
```

对于仅复制地址的低级别转换，你可以使用 [winrt::get_abi](/uwp/cpp-ref-for-winrt/get-abi)、[winrt::detach_abi](/uwp/cpp-ref-for-winrt/detach-abi) 和 [winrt::attach_abi](/uwp/cpp-ref-for-winrt/attach-abi) 帮助程序函数。

`WINRT_ASSERT` 是宏定义，并且它扩展到 [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros)。

```cppwinrt
    // The code in main() already shown above remains here.

    // Lowest-level conversions that only copy addresses

    // Convert to a non-owning ABI object with get_abi.
    abi::IStringable* non_owning{ static_cast<abi::IStringable*>(winrt::get_abi(uriAsIStringable)) };
    WINRT_ASSERT(non_owning);

    // Avoid interlocks this way.
    owning = static_cast<abi::IStringable*>(winrt::detach_abi(uriAsIStringable));
    WINRT_ASSERT(!uriAsIStringable);
    winrt::attach_abi(uriAsIStringable, owning);
    WINRT_ASSERT(uriAsIStringable);
```

## <a name="convert_from_abi-function"></a>convert_from_abi 函数
此帮助程序函数以最低的开销将原始 ABI 接口指针转换为等效的 C++/WinRT 对象。

```cppwinrt
template <typename T>
T convert_from_abi(::IUnknown* from)
{
    T to{ nullptr }; // `T` is a projected type.

    winrt::check_hresult(from->QueryInterface(winrt::guid_of<T>(),
        winrt::put_abi(to)));

    return to;
}
```

该函数只需调用 [QueryInterface](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)) 来查询请求的 C++/WinRT 类型的默认接口。

正如我们所见，从 C++/WinRT 对象转换成等效的 ABI 接口指针不需要帮助程序函数。 只需使用 [winrt::Windows::Foundation::IUnknown::as](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)（或 [try_as](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function)）成员函数来查询请求的接口。 as 和 try_as 函数将返回环绕请求的 ABI 类型的 [winrt::com_ptr](/uwp/cpp-ref-for-winrt/com-ptr) 对象。

## <a name="code-example-using-convert_from_abi"></a>使用 convert_from_abi 的代码示例
下面是介绍此帮助程序函数的实际应用的代码示例。

```cppwinrt
// pch.h
#pragma once
#include <windows.foundation.h>
#include <unknwn.h>
#include "winrt/Windows.Foundation.h"

// main.cpp
#include "pch.h"
#include <iostream>

using namespace winrt;
using namespace Windows::Foundation;

namespace winrt
{
    using namespace Windows::Foundation;
}

namespace abi
{
    using namespace ABI::Windows::Foundation;
};

namespace sample
{
    template <typename T>
    T convert_from_abi(::IUnknown* from)
    {
        T to{ nullptr }; // `T` is a projected type.

        winrt::check_hresult(from->QueryInterface(winrt::guid_of<T>(),
            winrt::put_abi(to)));

        return to;
    }
    inline auto put_abi(winrt::hstring& object) noexcept
    {
        return reinterpret_cast<HSTRING*>(winrt::put_abi(object));
    }
}

int main()
{
    winrt::init_apartment();

    winrt::Uri uri(L"http://aka.ms/cppwinrt");
    std::wcout << "C++/WinRT: " << uri.Domain().c_str() << std::endl;

    // Convert to an ABI type.
    winrt::com_ptr<abi::IUriRuntimeClass> ptr = uri.as<abi::IUriRuntimeClass>();
    winrt::hstring domain;
    winrt::check_hresult(ptr->get_Domain(sample::put_abi(domain)));
    std::wcout << "ABI: " << domain.c_str() << std::endl;

    // Convert from an ABI type.
    winrt::Uri uri_from_abi = sample::convert_from_abi<winrt::Uri>(ptr.get());

    WINRT_ASSERT(uri.Domain() == uri_from_abi.Domain());
    WINRT_ASSERT(uri == uri_from_abi);
}
```

## <a name="interoperating-with-abi-com-interface-pointers"></a>与 ABI COM 接口指针进行互操作

以下帮助器函数模板演示了如何将给定类型的 ABI COM 接口指针复制到其等效 C++/WinRT 投影智能指针类型。

```cppwinrt
template<typename To, typename From>
To to_winrt(From* ptr)
{
    To result{ nullptr };
    winrt::check_hresult(ptr->QueryInterface(winrt::guid_of<To>(), winrt::put_abi(result)));
    return result;
}
...
ID2D1Factory1* com_ptr{ ... };
auto cppwinrt_ptr {to_winrt<winrt::com_ptr<ID2D1Factory1>>(com_ptr)};
```

此下一个帮助程序函数模板是等效的，只不过它从 [Windows 实现库 (WIL)](https://github.com/Microsoft/wil) 中的智能指针类型进行复制。

```cppwinrt
template<typename To, typename From, typename ErrorPolicy>
To to_winrt(wil::com_ptr_t<From, ErrorPolicy> const& ptr)
{
    To result{ nullptr };
    if constexpr (std::is_same_v<typename ErrorPolicy::result, void>)
    {
        ptr.query_to(winrt::guid_of<To>(), winrt::put_abi(result));
    }
    else
    {
        winrt::check_result(ptr.query_to(winrt::guid_of<To>(), winrt::put_abi(result)));
    }
    return result;
}
```

另请参阅[通过 C++/WinRT 使用 COM 组件](/windows/uwp/cpp-and-winrt-apis/consume-com)。

### <a name="unsafe-interop-with-abi-com-interface-pointers"></a>与 ABI COM 接口指针进行的不安全互操作

下表显示给定类型的 ABI COM 接口指针和其等效 C++/WinRT 投影智能指针类型之间的不安全转换（以及其他操作）。 对于表中的代码，假定以下声明。

```cppwinrt
winrt::Sample s;
ISample* p;

void GetSample(_Out_ ISample** pp);
```

进一步假定 **ISample** 是 **Sample** 的默认接口。

可以在编译时使用此代码对其进行声明。

```cppwinrt
static_assert(std::is_same_v<winrt::default_interface<winrt::Sample>, winrt::ISample>);
```

| 操作 | 如何实现 | 注释 |
|-|-|-|
| 从 **winrt::Sample** 提取 **ISample\*** | `p = reinterpret_cast<ISample*>(get_abi(s));` | *s* 仍拥有该对象。 |
| 从 **winrt::Sample** 分离 **ISample\*** | `p = reinterpret_cast<ISample*>(detach_abi(s));` | *s* 不再拥有该对象。 |
| 将 **ISample\*** 传输到新的 **winrt::Sample** | `winrt::Sample s{ p, winrt::take_ownership_from_abi };` | *s* 取得该对象的所有权。 |
| 将 **ISample\*** 设置到 **winrt::Sample** 中 | `*put_abi(s) = p;` | *s* 取得该对象的所有权。 以前 *s* 所拥有的任何对象已泄露（将在调试中声明）。 |
| 将 **ISample\*** 接收到 **winrt::Sample** 中 | `GetSample(reinterpret_cast<ISample**>(put_abi(s)));` | *s* 取得该对象的所有权。 *s* 以前拥有的任何对象被泄露（将在调试中声明）。 |
| 替换 **winrt::Sample** 中的 **ISample\*** | `attach_abi(s, p);` | *s* 取得该对象的所有权。 *s* 以前拥有的对象被释放。 |
| 将 **ISample\*** 复制到 **winrt::Sample** 中 | `copy_from_abi(s, p);` | *s* 对该对象进行了新引用。 *s* 以前拥有的对象被释放。 |
| 将 **winrt::Sample** 复制到 **ISample\*** | `copy_to_abi(s, reinterpret_cast<void*&>(p));` | *p* 接收该对象的副本。 *s* 以前拥有的任何对象被泄露。 |

## <a name="interoperating-with-the-abis-guid-struct"></a>与 ABI 的 GUID 结构进行互操作

[**GUID**](/previous-versions/aa373931(v%3Dvs.80)) 投影为 **winrt::guid**。 对于实现的 API，必须将 winrt::guid 用于 GUID 参数。 否则，**winrt::guid** 与 **GUID** 之间存在自动转换，只要你在包括任何 C++/WinRT 头文件之前包括 `unknwn.h`（通过 <windows.h> 和许多其他头文件隐式包括）。

如果不这样做，则可以在它们之间执行硬 `reinterpret_cast` 操作。 对于下面的表，假定这些声明。

```cppwinrt
winrt::guid winrtguid;
GUID abiguid;
```

| 转换 | 有 `#include <unknwn.h>` | 没有 `#include <unknwn.h>` |
|-|-|-|
| 从 **winrt::guid** 到 **GUID** | `abiguid = winrtguid;` | `abiguid = reinterpret_cast<GUID&>(winrtguid);` |
| 从 **GUID** 到 **winrt::guid** | `winrtguid = abiguid;` | `winrtguid = reinterpret_cast<winrt::guid&>(abiguid);` |

## <a name="interoperating-with-the-abis-hstring"></a>与 ABI 的 HSTRING 进行互操作

下表显示 **winrt::hstring** 与 [**HSTRING**](/windows/win32/winrt/hstring) 之间的转换以及其他操作。 对于表中的代码，假定以下声明。

```cppwinrt
winrt::hstring s;
HSTRING h;

void GetString(_Out_ HSTRING* value);
```

| 操作 | 如何实现 | 注释 |
|-|-|-|
| 从 **hstring** 提取 **HSTRING** | `h = static_cast<HSTRING>(get_abi(s));` | *s* 仍拥有该字符串。 |
| 从 **hstring** 分离 **HSTRING** | `h = reinterpret_cast<HSTRING>(detach_abi(s));` | *s* 不再拥有该字符串。 |
| 将 **HSTRING** 设置到 **hstring** 中 | `*put_abi(s) = h;` | *s* 取得字符串的所有权。 *s* 以前拥有的任何字符串被泄露（将在调试中声明）。 |
| 将 **HSTRING** 接收到 **hstring** 中 | `GetString(reinterpret_cast<HSTRING*>(put_abi(s)));` | *s* 取得字符串的所有权。 *s* 以前拥有的任何字符串被泄露（将在调试中声明）。 |
| 替换 **hstring** 中的 **HSTRING** | `attach_abi(s, h);` | *s* 取得字符串的所有权。 *s* 以前拥有的字符串被释放。 |
| 将 **HSTRING** 复制到 **hstring** | `copy_from_abi(s, h);` | *s* 生成该字符串的私有副本。 *s* 以前拥有的字符串被释放。 |
| 将 **hstring** 复制到 **HSTRING** | `copy_to_abi(s, reinterpret_cast<void*&>(h));` | *h* 接收该字符串的副本。 *h* 以前拥有的任何字符串被泄露。 |

## <a name="important-apis"></a>重要的 API
* [AddRef 函数](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-addref)
* [QueryInterface 函数](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))
* [winrt::attach_abi 函数](/uwp/cpp-ref-for-winrt/attach-abi)
* [winrt::com_ptr 结构模板](/uwp/cpp-ref-for-winrt/com-ptr)
* [winrt::copy_from_abi 函数](/uwp/cpp-ref-for-winrt/copy-from-abi)
* [winrt::copy_to_abi 函数](/uwp/cpp-ref-for-winrt/copy-to-abi)
* [winrt::detach_abi 函数](/uwp/cpp-ref-for-winrt/detach-abi)
* [winrt::get_abi 函数](/uwp/cpp-ref-for-winrt/get-abi)
* [winrt::Windows::Foundation::IUnknown::as member 函数](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)
* [winrt::Windows::Foundation::IUnknown::try_as member 函数](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function)
