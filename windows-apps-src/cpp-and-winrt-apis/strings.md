---
author: stevewhims
description: 利用 C++/WinRT，你可以使用标准 C++ 宽字符串类型来调用 Windows 运行时 API，或者也可以使用 winrt::hstring 类型。
title: C++/WinRT 中的字符串处理
ms.author: stwhi
ms.date: 05/21/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, 字符串
ms.localizationpriority: medium
ms.openlocfilehash: 332edcf17f2b6bbf595def67c9df7043f21828c7
ms.sourcegitcommit: 4f6dc806229a8226894c55ceb6d6eab391ec8ab6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/20/2018
ms.locfileid: "4085914"
---
# <a name="string-handling-in-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>[C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 中的字符串处理
利用 C++/WinRT，你可以使用 C++ 标准库宽字符串类型（如 **std::wstring**）调用 Windows 运行时 API（注：不要使用窄字符串类型，例如 **std::string**）。 C++/WinRT 确实有名为 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) 的自定义字符串类型（在 C++/WinRT 基础库 `%WindowsSdkDir%Include\<WindowsTargetPlatformVersion>\cppwinrt\winrt\base.h` 中定义）。 这是 Windows 运行时构造函数、函数和属性实际上采用并返回的字符串类型。 但在很多情况下（由于 **hstring** 的转换构造函数和转换运算符），你可以选择是否要注意客户端代码中的 **hstring**。 如果你要*创作* API，则很可能需要了解 **hstring**。

C++ 中有很多字符串类型。 除了 C++ 标准库中的 **std::basic_string** 之外，变体还存在于很多库中。 C++17 具有字符串转换实用程序和 **std::basic_string_view**，用来消除所有字符串类型之间的差别。  [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) 利用 **std::wstring_view** 提供了可转换性，以实现 **std::basic_string_view** 应有的互操作性。

## <a name="using-stdwstring-and-optionally-winrthstringuwpcpp-ref-for-winrthstring-with-uriuwpapiwindowsfoundationuri"></a>将 **std::wstring**（也可以选择 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring)）与 [**Uri**](/uwp/api/windows.foundation.uri) 结合使用
[**Windows::Foundation::Uri**](/uwp/api/windows.foundation.uri) 从 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) 构建。

```cppwinrt
public:
    Uri(winrt::hstring uri) const;
```

但 **hstring** 具有可让你使用它而无需注意它的[转换构造函数](/uwp/api/windows.foundation.uri#hstringhstring-constructor)。 下面是一个代码示例，展示了如何从宽字符串参数、从宽字符串视图和从 **std::wstring** 创建 **Uri**。

```cppwinrt
#include <winrt/Windows.Foundation.h>
#include <string_view>

using namespace winrt;
using namespace Windows::Foundation;

int main()
{
    using namespace std::literals;

    winrt::init_apartment();

    // You can make a Uri from a wide string literal.
    Uri contosoUri{ L"http://www.contoso.com" };

    // Or from a wide string view.
    Uri contosoSVUri{ L"http://www.contoso.com"sv };

    // Or from a std::wstring.
    std::wstring wideString{ L"http://www.adventure-works.com" };
    Uri awUri{ wideString };
}
```

属性访问器 [**Uri::Domain**](https://docs.microsoft.com/uwp/api/windows.foundation.uri.Domain) 属于类型 **hstring**。

```cppwinrt
public:
    winrt::hstring Domain();
```

但重复一下，由于 **hstring** 的 [**std::wstring_view** 的转换运算符](/uwp/api/hstring#hstringoperator-stdwstringview)，注意该细节是一个可选操作。

```cppwinrt
// Access a property of type hstring, via a conversion operator to a standard type.
std::wstring domainWstring{ contosoUri.Domain() }; // L"contoso.com"
domainWstring = awUri.Domain(); // L"adventure-works.com"

// Or, you can choose to keep the hstring unconverted.
hstring domainHstring{ contosoUri.Domain() }; // L"contoso.com"
domainHstring = awUri.Domain(); // L"adventure-works.com"
```

同样，[**IStringable::ToString**](https://msdn.microsoft.com/library/windows/desktop/dn302136) 将返回 hstring。

```cppwinrt
public:
    hstring ToString() const;
```

**Uri** 将实现 [**IStringable**](https://msdn.microsoft.com/library/windows/desktop/dn302135) 接口。

```cppwinrt
// Access hstring's IStringable::ToString, via a conversion operator to a standard type.
std::wstring tostringWstring{ contosoUri.ToString() }; // L"http://www.contoso.com/"
tostringWstring = awUri.ToString(); // L"http://www.adventure-works.com/"

// Or you can choose to keep the hstring unconverted.
hstring tostringHstring{ contosoUri.ToString() }; // L"http://www.contoso.com/"
tostringHstring = awUri.ToString(); // L"http://www.adventure-works.com/"
```

你可以使用 [**hstring::c_str function**](/uwp/api/windows.foundation.uri#hstringcstr-function) 函数从 **hstring** 获取标准宽字符串（正如你可以从 **std::wstring** 获取一样）。

```cppwinrt
#include <iostream>
std::wcout << tostringHstring.c_str() << std::endl;
```
如果你有 **hstring**，则可以通过它创建 **Uri**。

```cppwinrt
Uri awUriFromHstring{ tostringHstring };
```

考虑一个采用 **hstring** 的方法。

```cppwinrt
public:
    Uri CombineUri(winrt::hstring relativeUri) const;
```

你刚刚看到的所有选项在这类情况下也适用。

```cppwinrt
std::wstring contact{ L"contact" };
contosoUri = contosoUri.CombineUri(contact);
    
std::wcout << contosoUri.ToString().c_str() << std::endl;
```

**hstring** 具有成员 **std::wstring_view** 转换运算符，转换是免费实现的。

```cppwinrt
void legacy_print(std::wstring_view view);

void Print(winrt::hstring const& hstring)
{
    legacy_print(hstring);
}
```

## <a name="winrthstring-functions-and-operators"></a>**winrt::hstring** 函数和运算符
为 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) 实现了大量构造函数、运算符、函数和迭代程序。

**hstring** 是一个范围，因此你可以将其与基于范围的 `for` 或与 `std::for_each` 一起使用。 它还提供了一个比较运算符，用于自然、高效地与它在 C++ 标准库中的对应项进行比较。 它还包含将 **hstring** 用作关联容器的键所需的一切。

我们发现很多 C++ 库使用了 **std::string**，并且仅与 UTF-8 文本配合。 为方便起见，我们提供了 [**winrt::to_string**](/uwp/cpp-ref-for-winrt/to-string)、[**winrt::to_hstring**](/uwp/cpp-ref-for-winrt/to-hstring) 等用于来回转换的帮助程序。

```cppwinrt
winrt::hstring w{ L"Hello, World!" };

std::string c = winrt::to_string(w);
WINRT_ASSERT(c == "Hello, World!");

w = winrt::to_hstring(c);
WINRT_ASSERT(w == L"Hello, World!");
```

有关 **hstring** 函数和运算符的更多示例和信息，请参阅 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) API 参考主题。

## <a name="the-rationale-for-winrthstring-and-winrtparamhstring"></a>**winrt::hstring** 和 **winrt::param::hstring** 的原理
Windows 运行时根据 **wchar_t** 字符实现，但 Windows 运行时的应用程序二进制接口 (ABI) 不是 **std::wstring** 或 **std::wstring_view** 提供的内容的一部分。 使用这些将导致效率显著降低。 相反，C++/WinRT 提供了 **winrt::hstring**，它表示与基础 [HSTRING](https://msdn.microsoft.com/library/windows/desktop/br205775) 一致的不可变字符串，在与 **std::wstring** 的接口相似的接口后面实现。 

你可能会注意到在逻辑上应该接受 **winrt::hstring** 的 C++/WinRT 输入参数实际上需要 **winrt::param::hstring**。 **param** 命名空间包含一组类型，专用于优化输入参数以自然地绑定到 C++ 标准库类型，以及避免副本和其他低效率现象。 你不应直接使用这些类型。 如果你要对自己的函数使用优化，则应使用 **std::wstring_view**。

这样，你便可以在很大程度上忽略 Windows 运行时字符串管理的细节，并使用你了解的资源高效地工作。 考虑到在 Windows 运行时中使用字符串的频率，这一点很重要。

# <a name="formatting-strings"></a>格式化字符串
用于字符串格式化的一个选择是 **std::wstringstream**。 下面是格式化和显示简单调试跟踪消息的示例。

```cppwinrt
#include <sstream>
...
void OnPointerPressed(IInspectable const&, PointerEventArgs const& args)
{
    float2 const point = args.CurrentPoint().Position();
    std::wstringstream wstringstream;
    wstringstream << L"Pointer pressed at (" << point.x << L"," << point.y << L")" << std::endl;
    ::OutputDebugString(wstringstream.str().c_str());
}
```

## <a name="important-apis"></a>重要的 API
* [winrt::hstring 结构](/uwp/cpp-ref-for-winrt/hstring)
* [winrt:: to_hstring 函数](/uwp/cpp-ref-for-winrt/to-hstring)
* [winrt:: to_string 函数](/uwp/cpp-ref-for-winrt/to-string)
