---
author: stevewhims
description: 利用 C++/WinRT，你可以使用标准 C++ 数据类型调用 Windows 运行时 API。
title: 标准 C++ 数据类型和 C++/WinRT
ms.author: stwhi
ms.date: 04/10/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, 数据, 类型
ms.localizationpriority: medium
ms.openlocfilehash: ccf79b1ec21688d9573e62777def8f15295c3fca
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/03/2018
ms.locfileid: "1832071"
---
# <a name="standard-c-data-types-and-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>标准 C++ 数据类型和 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)
借助 C++/WinRT，你可以使用标准 C++ 数据类型（包括某些 C++ 标准库数据类型）调用 Windows 运行时 API。

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
    auto properties = co_await storageFile.Properties().RetrievePropertiesAsync({ L"System.ItemUrl" });
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

C++/WinRT 将 **std::vector** 作为 Windows 运行时集合参数绑定。 因此，你可以传递一个 **std::vector&lt;winrt::hstring&gt;**，它将转换为 **winrt::hstring** 的合适 Windows 运行时集合。 如果被调用方是异步的，你必须复制或移动矢量。 在下面的代码示例中，我们将矢量的所有权转移到异步被调用方。

```cppwinrt
IAsyncAction retrieve_properties_async(StorageFile const& storageFile, std::vector<winrt::hstring> const& vecH)
{
    auto properties = co_await storageFile.Properties().RetrievePropertiesAsync(std::move(vecH));
}
```

但你无法传递需要 Windows 运行时集合的 **std::vector&lt;std::wstring&gt;**。 原因在于，由于已经转换为 **std::wstring** 的合适 Windows 运行时集合，C++ 语言随后不会强制转换该集合的类型参数。 因此，不会编译下面的代码示例。

```cppwinrt
IAsyncAction retrieve_properties_async(StorageFile const& storageFile, std::vector<std::wstring> const& vecW)
{
    auto properties = co_await storageFile.Properties().RetrievePropertiesAsync(std::move(vecW)); // error! Can't convert from vector of wstring to async_iterable of hstring.
}
```

## <a name="raw-arrays-and-pointer-ranges"></a>原始数组和指针范围
请记住，C++ 标准库中将来可能存在等效类型，如果你选择或需要直接使用 **array_view**，则还可以这样做。

**array_view** 具有来自原始数组和来自一系列 **T**（指向元素类型的指针）的转换构造函数。

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

## <a name="important-apis"></a>重要的 API
* [winrt::array_view 结构模板](/uwp/cpp-ref-for-winrt/array-view)
