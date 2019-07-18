---
description: 利用 C++/WinRT，你可以使用标准 C++ 数据类型调用 Windows 运行时 API。
title: 标准 C++ 数据类型和 C++/WinRT
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, 数据, 类型
ms.localizationpriority: medium
ms.openlocfilehash: a87ba48a0853058ba1259e079c586b97af551656
ms.sourcegitcommit: 8b4c1fdfef21925d372287901ab33441068e1a80
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/12/2019
ms.locfileid: "67844334"
---
# <a name="standard-c-data-types-and-cwinrt"></a>标准 C++ 数据类型和 C++/WinRT

借助 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)，可以使用标准 C++ 数据类型（包括某些 C++ 标准库数据类型）调用 Windows 运行时 API。 可以将标准字符串传递到 API（请参阅 ](strings.md)C++/WinRT 中的字符串处理[），还可以将初始值列表和标准容器传递到 API，这些 API 需要语义上等价的集合。

另请参阅[将参数传递到 ABI 边界](/windows/uwp/cpp-and-winrt-apis/pass-parms-to-abi)。

## <a name="standard-initializer-lists"></a>标准初始值列表
初始值列表 (std::initializer_list) 是 C++ 标准库构造  。 调用特定的 Windows 运行时构造函数和方法时，可以使用初始值列表。 例如，可以使用初始值列表来调用 [DataWriter::WriteBytes](/uwp/api/windows.storage.streams.datawriter.writebytes)  。

```cppwinrt
#include <winrt/Windows.Storage.Streams.h>

using namespace winrt::Windows::Storage::Streams;

int main()
{
    winrt::init_apartment();

    InMemoryRandomAccessStream stream;
    DataWriter dataWriter{stream};
    dataWriter.WriteBytes({ 99, 98, 97 }); // the initializer list is converted to a winrt::array_view before being passed to WriteBytes.
}
```

此工作分为两个部分。 第一，DataWriter::WriteBytes 方法选取一个 [winrt::array_view](/uwp/cpp-ref-for-winrt/array-view) 类型的参数   。

```cppwinrt
void WriteBytes(winrt::array_view<uint8_t const> value) const
```

winrt::array_view 是用于安全地表示一系列连续值的自定义 C++/WinRT 类型（它在 C++/WinRT 基础库 `%WindowsSdkDir%Include\<WindowsTargetPlatformVersion>\cppwinrt\winrt\base.h` 中定义）  。

第二，winrt::array_view 有一个初始值列表构造函数  。

```cppwinrt
template <typename T> winrt::array_view(std::initializer_list<T> value) noexcept
```

在很多情况下，可以选择是否要注意编程中的 winrt::array_view  。 如果你选择不注意它，那么在等效类型出现在 C++ 标准库中时不用更改任何代码  。

可以将初始值列表传递给需要集合参数的 Windows 运行时 API。 以 StorageItemContentProperties::RetrievePropertiesAsync 为例  。

```cppwinrt
IAsyncOperation<IMap<winrt::hstring, IInspectable>> StorageItemContentProperties::RetrievePropertiesAsync(IIterable<winrt::hstring> propertiesToRetrieve) const;
```

可以使用类似如下的初始值列表调用该 API。

```cppwinrt
IAsyncAction retrieve_properties_async(StorageFile const& storageFile)
{
    auto properties{ co_await storageFile.Properties().RetrievePropertiesAsync({ L"System.ItemUrl" }) };
}
```

在这里，两个因子正在起作用。 第一个，被调用方从初始值列表构建 std::vector（此被调用方是异步的，因此能够拥有该对象，而这是必需的）  。 第二个，C++/WinRT 透明地（并且不会引入副本）将 std::vector 作为 Windows 运行时集合参数绑定  。

## <a name="standard-arrays-and-vectors"></a>标准数组和矢量
[winrt::array_view](/uwp/cpp-ref-for-winrt/array-view) 还有来自 std::vector 和 std::array 的转换构造函数    。

```cppwinrt
template <typename C, size_type N> winrt::array_view(std::array<C, N>& value) noexcept
template <typename C> winrt::array_view(std::vector<C>& vectorValue) noexcept
```

因此，可以改为使用 std::vector 调用 DataWriter::WriteBytes   。

```cppwinrt
std::vector<byte> theVector{ 99, 98, 97 };
dataWriter.WriteBytes(theVector); // theVector is converted to a winrt::array_view before being passed to WriteBytes.
```

或使用 std::array  。

```cppwinrt
std::array<byte, 3> theArray{ 99, 98, 97 };
dataWriter.WriteBytes(theArray); // theArray is converted to a winrt::array_view before being passed to WriteBytes.
```

C++/WinRT 将 std::vector 作为 Windows 运行时集合参数绑定  。 因此，可以传递一个 std::vector&lt;winrt::hstring&gt;，它将转换为 winrt::hstring 的合适 Windows 运行时集合   。 如果被调用方是异步的，则需要记住一个额外细节。 由于这种情况的实现细节，需要提供右值，因此必须提供矢量的副本或动作。 在以下代码示例中，我们将矢量的所有权移动到异步的被调用方所接受的参数类型的对象（然后在移动之后务必不再访问 `vecH`）。 如果想要了解有关右值的详细信息，请参阅[值类别以及对它们的引用](cpp-value-categories.md)。

```cppwinrt
IAsyncAction retrieve_properties_async(StorageFile const storageFile, std::vector<winrt::hstring> vecH)
{
    auto properties{ co_await storageFile.Properties().RetrievePropertiesAsync(std::move(vecH)) };
}
```

但你无法传递需要 Windows 运行时集合的 std::vector&lt;std::wstring&gt;  。 原因在于，由于已经转换为 std::wstring 的合适 Windows 运行时集合，C++ 语言随后不会强制转换该集合的类型参数  。 因此，将不会编译以下代码示例（解决方案将改为传递 std:: vector&lt;winrt::hstring&gt;，如上所示）  。

```cppwinrt
IAsyncAction retrieve_properties_async(StorageFile const& storageFile, std::vector<std::wstring> const& vecW)
{
    auto properties{ co_await storageFile.Properties().RetrievePropertiesAsync(std::move(vecW)) }; // error! Can't convert from vector of wstring to async_iterable of hstring.
}
```

## <a name="raw-arrays-and-pointer-ranges"></a>原始数组和指针范围
请记住，C++ 标准库中将来可能存在等效类型，如果你选择或需要直接使用 winrt::array_view，则还可以这样做  。

winrt::array_view 具有来自原始数组和来自一系列 T&ast;（指向元素类型的指针）的转换构造函数   。

```cppwinrt
using namespace winrt;
...
byte theRawArray[]{ 99, 98, 97 };
array_view<byte const> fromRawArray{ theRawArray };
dataWriter.WriteBytes(fromRawArray); // the winrt::array_view is passed to WriteBytes.

array_view<byte const> fromRange{ theArray.data(), theArray.data() + 2 }; // just the first two elements.
dataWriter.WriteBytes(fromRange); // the winrt::array_view is passed to WriteBytes.
```

## <a name="winrtarrayview-functions-and-operators"></a>winrt::array_view 函数和运算符
为 winrt::array_view 实现了大量构造函数、运算符、函数和迭代程序  。 winrt::array_view 是一个范围，因此可以将其与基于范围的 `for` 或与 std::for_each 一起使用   。

有关更多示例和信息，请参阅 [winrt::array_view](/uwp/cpp-ref-for-winrt/array-view) API 参考主题  。

## <a name="ivectorlttgt-and-standard-iteration-constructs"></a>IVector&lt;T&gt; 和标准迭代构造 
[SyndicationFeed.Items](/uwp/api/windows.web.syndication.syndicationfeed.items) 是 Windows 运行时 API，它返回类型 [IVector&lt;T&gt;](/uwp/api/windows.foundation.collections.ivector_t_) 的集合（作为 winrt::Windows::Foundation::Collections::IVector&lt;T&gt; 投影到 C++/WinRT）    。 可以将此类型与基于范围的 `for` 等标准迭代结构一起使用。

```cppwinrt
// main.cpp
#include "pch.h"
#include <winrt/Windows.Web.Syndication.h>
#include <iostream>

using namespace winrt;
using namespace Windows::Web::Syndication;

void PrintFeed(SyndicationFeed const& syndicationFeed)
{
    for (SyndicationItem const& syndicationItem : syndicationFeed.Items())
    {
        std::wcout << syndicationItem.Title().Text().c_str() << std::endl;
    }
}
```

## <a name="c-coroutines-with-asynchronous-windows-runtime-apis"></a>具有异步 Windows 运行时 API 的 C++ 协同程序
调用异步 Windows 运行时 API 时，可以继续使用[并行模式库 (PPL)](/cpp/parallel/concrt/parallel-patterns-library-ppl)。 但在许多情况下，C++ 协同程序为与异步对象进行交互提供了一种高效且易于编码的习惯用法。 详细信息和代码示例，请参阅[利用 C++/WinRT 实现的并发和异步运算](concurrency.md)。

## <a name="important-apis"></a>重要的 API
* [IVector&lt;T&gt; 接口](/uwp/api/windows.foundation.collections.ivector_t_)
* [winrt::array_view 结构模板](/uwp/cpp-ref-for-winrt/array-view)

## <a name="related-topics"></a>相关主题
* [C++/WinRT 中的字符串处理](strings.md)
