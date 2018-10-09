---
author: stevewhims
description: 本主题介绍了如何在应用程序二进制接口 (ABI) 和 C++/WinRT 对象之间转换。
title: 实现 C++/WinRT 与 ABI 之间的互操作
ms.author: stwhi
ms.date: 05/21/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, 端口, 迁移, 互操作, ABI
ms.localizationpriority: medium
ms.openlocfilehash: 098d182b9cc4cc51bda0a7959702e53accf2699f
ms.sourcegitcommit: 49aab071aa2bd88f1c165438ee7e5c854b3e4f61
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2018
ms.locfileid: "4468929"
---
# <a name="interop-between-cwinrt-and-the-abi"></a>实现 C++/WinRT 与 ABI 之间的互操作

本主题介绍如何在 SDK 应用程序二进制接口 (ABI) 之间转换和[C + + WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)对象。 你可以借助这些技术，为使用 Windows 运行时的这两种编程方式的代码实现互操作，也可以在将代码从 ABI 逐步迁移到 C++/WinRT 时使用这些技术。

## <a name="what-is-the-windows-runtime-abi-and-what-are-abi-types"></a>什么是 Windows 运行时 ABI？什么是 ABI 类型？
Windows 运行时类（运行时类）实际上是一种抽象。 这种抽象定义了一个二进制接口（应用程序二进制接口，或 ABI），它允许各种编程语言与一个对象进行交互。 不管使用何种编程语言，客户端代码与 Windows 运行时对象的交互发生在最低级别，在此客户端语言构造被转换为对象的 ABI 调用。

文件夹“%WindowsSdkDir%Include\10.0.17134.0\winrt”（必要时根据情况调整 SDK 版本号）中的 Windows SDK 头文件是 Windows 运行时 ABI 头文件。 它们由 MIDL 编译器生成。 下面是包含这些标头之一的示例。

```
#include <windows.foundation.h>
```

下面是你将在该特定 SDK 头文件发现的 ABI 类型之一的简化示例。 注意，**ABI** 命名空间、**Windows::Foundation** 和所有其他 Windows 命名空间由 **ABI** 命名空间中的 SDK 头文件声明。

```
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

**IUriRuntimeClass** 是 COM 接口。 此外（由于它的基是 **IInspectable**），**IUriRuntimeClass** 还是 Windows 运行时接口。 请注意 **HRESULT** 返回类型，而不是异常的引发。 还有 **HSTRING** 句柄等项目的使用（最好在使用完该句柄后将其设置回 `nullptr`）。 这在应用程序二进制文件级别呈现了 Windows 运行时应有的样子；换句话说，在 COM 编程级别。

Windows 运行时基于组件对象模型 (COM) API。 你可以用那种方式访问 Windows 运行时，也可以通过*语言投影* 访问它。 投影将隐藏 COM 详细信息，并为给定语言提供更自然的编程体验。

例如，如果你查看文件夹“%WindowsSdkDir%Include\10.0.17134.0\cppwinrt\winrt”（重复一下，必要时根据情况调整 SDK 版本号），就会发现 C++/WinRT 语言投影标头。 每个 Windows 命名空间都有一个标头，就像每个 Windows 命名空间都有一个 ABI 标头一样。 下面是包含 C++/WinRT 标头之一的示例。

```cppwinrt
#include <winrt/Windows.Foundation.h>
```

此外，在该标头中，这里（已简化）是我们刚才看到的 ABI 类型的 C++/WinRT 等效项。

```
namespace winrt::Windows::Foundation
{
    struct Uri : IUriRuntimeClass, ...
    {
        winrt::hstring AbsoluteUri() const { ... }
        ...
    };
}
```

此处的接口是新式标准 C++。 它去掉了 **HRESULT**（必要时，C++/WinRT 将引发异常）。 此外，访问器函数返回了一个简单字符串对象，该对象在其作用域的末端被清除。

本主题适用于希望与在应用程序二进制接口 (ABI) 层工作的代码进行互操作或进行移植的情况。

## <a name="converting-to-and-from-abi-types-in-code"></a>在代码中转换到/自 ABI 类型
为安全和简单起见，对于两个方向的转换，你都可以使用 [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr)、[**com_ptr::as**](/uwp/cpp-ref-for-winrt/com-ptr#comptras-function) 和 [**winrt::Windows::Foundation::IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)。 下面是代码示例（基于**控制台应用**项目模板），该示例说明了如何使用不同岛的命名空间别名处理 C++/WinRT 投影与 ABI 之间潜在的命名空间冲突。

```cppwinrt
// main.cpp
#include "pch.h"

#include <windows.foundation.h>
#include <winrt/Windows.Foundation.h>

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
    winrt::com_ptr<abi::IStringable> ptr = uri.as<abi::IStringable>();

    // Convert from an ABI type.
    uri = ptr.as<winrt::Uri>();
    winrt::IStringable uriAsIStringable = ptr.as<winrt::IStringable>();
}
```

**as** 函数的实现调用了 [**QueryInterface**](https://msdn.microsoft.com/library/windows/desktop/ms682521)。 如果你需要仅调用 [**AddRef**](https://msdn.microsoft.com/library/windows/desktop/ms691379) 的较低级别的转换，则可以使用 [**winrt::copy_to_abi**](/uwp/cpp-ref-for-winrt/copy-to-abi) 和 [**winrt::copy_from_abi**](/uwp/cpp-ref-for-winrt/copy-from-abi) 帮助程序函数。 后面这个代码示例向上面的代码示例添加了这些较低级别的转换。

```cppwinrt
int main()
{
    ...

    // Lower-level conversions that only call AddRef.

    // Convert to an ABI type.
    ptr = nullptr;
    winrt::copy_to_abi(uri, *ptr.put_void());

    // Convert from an ABI type.
    uri = nullptr;
    winrt::copy_from_abi(uri, ptr.get());
    ptr = nullptr;
}
```

下面同样是低级别转换方法，但使用了指向 ABI 接口类型（由 Windows SDK 标头定义）的原始指针。

```cppwinrt
    ...

    // Copy to an owning raw ABI pointer with copy_to_abi.
    abi::IStringable* owning = nullptr;
    winrt::copy_to_abi(uri, *reinterpret_cast<void**>(&owning));

    // Copy from a raw ABI pointer.
    uri = nullptr;
    winrt::copy_from_abi(uri, owning);
    owning->Release();
```

对于仅复制地址的低级别转换，你可以使用 [**winrt::get_abi**](/uwp/cpp-ref-for-winrt/get-abi)、[**winrt::detach_abi**](/uwp/cpp-ref-for-winrt/detach-abi) 和 [**winrt::attach_abi**](/uwp/cpp-ref-for-winrt/attach-abi) 帮助程序函数。

```cppwinrt
    ...

    // Lowest-level conversions that only copy addresses

    // Convert to a non-owning ABI object with get_abi.
    abi::IStringable* non_owning = static_cast<abi::IStringable*>(winrt::get_abi(uri));
    WINRT_ASSERT(non_owning);

    // Avoid interlocks this way.
    owning = static_cast<abi::IStringable*>(winrt::detach_abi(uri));
    WINRT_ASSERT(!uri);
    winrt::attach_abi(uri, owning);
    WINRT_ASSERT(uri);
```

## <a name="convertfromabi-function"></a>convert_from_abi 函数
此帮助程序函数以最低的开销将原始 ABI 接口指针转换为等效的 C++/WinRT 对象。

```cppwinrt
template <typename T>
T convert_from_abi(::IUnknown* from)
{
    T to{ nullptr };

    winrt::check_hresult(from->QueryInterface(winrt::guid_of<T>(),
        reinterpret_cast<void**>(winrt::put_abi(to))));

    return to;
}
```

该函数只需调用 [**QueryInterface**](https://msdn.microsoft.com/library/windows/desktop/ms682521) 来查询请求的 C++/WinRT 类型的默认接口。

正如我们所见，从 C++/WinRT 对象转换成等效的 ABI 接口指针不需要帮助程序函数。 只需使用 [**winrt::Windows::Foundation::IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)（或 [**try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntryas-function)）成员函数来查询请求的接口。 **as** 和 **try_as** 函数将返回环绕请求的 ABI 类型的 [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr) 对象。

## <a name="code-example-using-convertfromabi"></a>使用 convert_from_abi 的代码示例
下面是介绍此帮助程序函数的实际应用的代码示例。

```cppwinrt
// main.cpp
#include "pch.h"

#include <windows.foundation.h>
#include <winrt/Windows.Foundation.h>
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

template <typename T>
T convert_from_abi(::IUnknown* from)
{
    T to{ nullptr };

    winrt::check_hresult(from->QueryInterface(winrt::guid_of<T>(),
        reinterpret_cast<void**>(winrt::put_abi(to))));

    return to;
}

int main()
{
    winrt::init_apartment();

    winrt::Uri uri(L"http://aka.ms/cppwinrt");
    std::wcout << "C++/WinRT: " << uri.Domain().c_str() << std::endl;

    // Convert to an ABI type.
    winrt::com_ptr<abi::IUriRuntimeClass> ptr = uri.as<abi::IUriRuntimeClass>();
    winrt::hstring domain;
    winrt::check_hresult(ptr->get_Domain(put_abi(domain)));
    std::wcout << "ABI: " << domain.c_str() << std::endl;

    // Convert from an ABI type.
    winrt::Uri uri_from_abi = convert_from_abi<winrt::Uri>(ptr.get());

    WINRT_ASSERT(uri.Domain() == uri_from_abi.Domain());
    WINRT_ASSERT(uri == uri_from_abi);
}
```

## <a name="important-apis"></a>重要的 API
* [AddRef 函数](https://msdn.microsoft.com/library/windows/desktop/ms691379)
* [QueryInterface 函数](https://msdn.microsoft.com/library/windows/desktop/ms682521)
* [winrt:: attach_abi 函数](/uwp/cpp-ref-for-winrt/attach-abi)
* [winrt::com_ptr 结构模板](/uwp/cpp-ref-for-winrt/com-ptr)
* [winrt:: copy_from_abi 函数](/uwp/cpp-ref-for-winrt/copy-from-abi)
* [winrt:: copy_to_abi 函数](/uwp/cpp-ref-for-winrt/copy-to-abi)
* [winrt:: detach_abi 函数](/uwp/cpp-ref-for-winrt/detach-abi)
* [winrt::get_abi 函数](/uwp/cpp-ref-for-winrt/get-abi)
* [winrt::Windows::Foundation::IUnknown::as member 函数](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)
* [winrt::Windows::Foundation::IUnknown::try_as member 函数](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntryas-function)
