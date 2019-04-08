---
description: 利用 C++/WinRT，你可以使用标准 C++ 数据类型调用 Windows 运行时 API。
title: 标准 C++ 数据类型和 C++/WinRT
ms.date: 05/07/2018
ms.topic: article
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, 数据, 类型
ms.localizationpriority: medium
ms.openlocfilehash: 7b0b529bbf397b76acb1eb589095a84f5c85745c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57654282"
---
# <a name="standard-c-data-types-and-cwinrt"></a>标准 C++ 数据类型和 C++/WinRT

与[C + + WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)，可以调用使用标准 c + + 数据类型，包括一些 c + + 标准库的数据类型的 Windows 运行时 Api。 可以将标准字符串传递给 Api (请参阅[字符串处理中 C + + WinRT](strings.md))，并且可以传递初始值设定项列表和标准容器到需要在语义上等效的集合 Api。

## <a name="standard-initializer-lists"></a>标准初始值列表
初始值列表 (**std::initializer_list**) 是 C++ 标准库构造。 在调用特定的 Windows 运行时构造函数和方法时，你可以使用初始值列表。 例如，你可以使用一个初始值列表来调用 [**DataWriter::WriteBytes**](/uwp/api/windows.storage.streams.datawriter.writebytes)。

```cppwinrt
#include <winrt/Windows.Storage.Streams.h>

using namespace winrt::Windows::Storage::Streams;

int main()
{
    winrt::init_apartment();

    InMemoryRandomAccessStream stream;
    DataWriter dataWriter{stream};
    dataWriter.WriteBytes({ 99, 98, 97 }); // the initializer list is converted to an array_view before being passed to WriteBytes.
}
```

此工作分为两个部分。 第一，**DataWriter::WriteBytes** 方法选取一个 [**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view) 类型的参数。

```cppwinrt
void WriteBytes(array_view<uint8_t const> value) const
```

 **array_view** 是用于安全地表示一系列连续值的自定义 C++/WinRT 类型（它在 C++/WinRT 基础库 `%WindowsSdkDir%Include\<WindowsTargetPlatformVersion>\cppwinrt\winrt\base.h` 中定义）。

第二，**array_view** 有一个初始值列表构造函数。

```cppwinrt
template <typename T> array_view(std::initializer_list<T> value) noexcept
```

在很多情况下，你可以选择是否要注意编程中的 **array_view**。 如果你选择*不* 注意它，那么在等效类型出现在 C++ 标准库中时不用更改任何代码。

你可以将初始值列表传递给需要集合参数的 Windows 运行时 API。 与 **StorageItemContentProperties::RetrievePropertiesAsync** 为例。

```cppwinrt
IAsyncOperation<IMap<winrt::hstring, IInspectable>> StorageItemContentProperties::RetrievePropertiesAsync(IIterable<winrt::hstring> propertiesToRetrieve) const;
```

你可以使用类似于下面的初始值列表调用该 API。

```cppwinrt
IAsyncAction retrieve_properties_async(StorageFile const& storageFile)
{
    auto properties{ co_await storageFile.Properties().RetrievePropertiesAsync({ L"System.ItemUrl" }) };
}
```

在这里，两个因子正在起作用 第一个，被调用方从初始值列表构建 **std::vector**（此被调用方是异步的，因此能够拥有该对象，而这是必需的）。 第二个，C++/WinRT 透明地（并且不会引入副本）将 **std::vector** 作为 Windows 运行时集合参数绑定。

## <a name="standard-arrays-and-vectors"></a>标准数组和矢量
**array_view** 还有来自 **std::vector** 和 **std::array** 的转换构造函数。

```cppwinrt
template <typename C, size_type N> array_view(std::array<C, N>& value) noexcept
template <typename C> array_view(std::vector<C>& vectorValue) noexcept
```

因此，你可以改为使用 **std::vector** 调用 **DataWriter::WriteBytes**。

```cppwinrt
std::vector<byte> theVector{ 99, 98, 97 };
dataWriter.WriteBytes(theVector); // theVector is converted to an array_view before being passed to WriteBytes.
```

或使用 **std::array**。

```cppwinrt
std::array<byte, 3> theArray{ 99, 98, 97 };
dataWriter.WriteBytes(theArray); // theArray is converted to an array_view before being passed to WriteBytes.
```

C++/WinRT 将 **std::vector** 作为 Windows 运行时集合参数绑定。 因此，你可以传递一个 **std::vector&lt;winrt::hstring&gt;**，它将转换为 **winrt::hstring** 的合适 Windows 运行时集合。 没有要牢记在心，如果被调用方是异步的额外详细信息。 由于这种情况下的实现详细信息，你将需要提供右值，因此，必须提供复制或移动的向量。 在下面的代码示例中，我们将所有权移动矢量的到异步被调用方所接受的参数类型的对象 (我们小心以免访问`vecH`后将其移)。 如果你想要了解有关右值的详细信息，请参阅[值的分类，并对其的引用](cpp-value-categories.md)。

```cppwinrt
IAsyncAction retrieve_properties_async(StorageFile const storageFile, std::vector<winrt::hstring> vecH)
{
    auto properties{ co_await storageFile.Properties().RetrievePropertiesAsync(std::move(vecH)) };
}
```

但你无法传递需要 Windows 运行时集合的 **std::vector&lt;std::wstring&gt;**。 原因在于，由于已经转换为 **std::wstring** 的合适 Windows 运行时集合，C++ 语言随后不会强制转换该集合的类型参数。 因此，将无法编译下面的代码示例 (和解决方案是传递**std:: vector&lt;winrt::hstring&gt;** 相反，如上所示)。

```cppwinrt
IAsyncAction retrieve_properties_async(StorageFile const& storageFile, std::vector<std::wstring> const& vecW)
{
    auto properties{ co_await storageFile.Properties().RetrievePropertiesAsync(std::move(vecW)) }; // error! Can't convert from vector of wstring to async_iterable of hstring.
}
```

## <a name="raw-arrays-and-pointer-ranges"></a>原始数组和指针范围
请记住，C++ 标准库中将来可能存在等效类型，如果你选择或需要直接使用 **array_view**，则还可以这样做。

**array_view**已从原始数组，并通过一系列的转换构造函数**T&ast;**  （指向的元素类型的指针）。

```cppwinrt
using namespace winrt;
...
byte theRawArray[]{ 99, 98, 97 };
array_view<byte const> fromRawArray{ theRawArray };
dataWriter.WriteBytes(fromRawArray); // the array_view is passed to WriteBytes.

array_view<byte const> fromRange{ theArray.data(), theArray.data() + 2 }; // just the first two elements.
dataWriter.WriteBytes(fromRange); // the array_view is passed to WriteBytes.
```

## <a name="winrtarrayview-functions-and-operators"></a>winrt::array_view 函数和运算符
为 **array_view** 实现了大量构造函数、运算符、函数和迭代程序。 **array_view** 是一个范围，因此你可以将其与基于范围的 `for` 或与 **std::for_each** 一起使用。

有关更多示例和信息，请参阅 [**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view) API 参考主题。

## <a name="ivectorlttgt-and-standard-iteration-constructs"></a>**IVector&lt;T&gt;** 和标准迭代构造
[**SyndicationFeed.Items** ](/uwp/api/windows.web.syndication.syndicationfeed.items)是一个 Windows 运行时 API，它返回的类型集合的一个示例[ **IVector&lt;T&gt;**  ](/uwp/api/windows.foundation.collections.ivector_t_) （投射到 C ++ 作为 WinRT **winrt::Windows::Foundation::Collections::IVector&lt;T&gt;**)。 您可以使用此类型与标准迭代构造，如基于范围的`for`。

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

## <a name="c-coroutines-with-asynchronous-windows-runtime-apis"></a>异步 Windows 运行时 api 的 c + + 协同程序
你可以继续使用[并行模式库 (PPL)](/cpp/parallel/concrt/parallel-patterns-library-ppl)时调用异步 Windows 运行时 Api。 但是，在许多情况下，c + + 协同例程与异步对象进行交互提供高效且更轻松地编码惯例。 有关详细信息和代码示例，请参阅[并发和异步操作使用 C + + WinRT](concurrency.md)。

## <a name="important-apis"></a>重要的 API
* [IVector&lt;T&gt;接口](/uwp/api/windows.foundation.collections.ivector_t_)
* [winrt::array_view 结构模板](/uwp/cpp-ref-for-winrt/array-view)

## <a name="related-topics"></a>相关主题
* [字符串处理中 C + + WinRT](strings.md)
