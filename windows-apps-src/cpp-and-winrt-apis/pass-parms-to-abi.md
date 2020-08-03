---
description: C++/ WinRT 通过提供针对常见情况的自动转换，简化了将参数传递到 ABI 边界。
title: 将参数传递到 ABI 边界
ms.date: 07/10/2019
ms.topic: article
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, 传递, 参数, ABI
ms.localizationpriority: medium
ms.openlocfilehash: 51cde2332d3d9df9d1f488aa7f8246f9e1e2ed36
ms.sourcegitcommit: e1104689fc1db5afb85701205c2580663522ee6d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/23/2020
ms.locfileid: "86997974"
---
# <a name="passing-parameters-into-the-abi-boundary"></a>将参数传递到 ABI 边界

使用 **winrt::param** 命名空间中的类型，C++/ WinRT 通过提供针对常见情况的自动转换，简化了将参数传递到 ABI 边界。 可以在[字符串处理](/windows/uwp/cpp-and-winrt-apis/strings)和[标准 C++ 数据类型和 C++/WinRT](/windows/uwp/cpp-and-winrt-apis/std-cpp-data-types) 中看到更多详细信息和代码示例。

> [!IMPORTANT]
> 不应自行使用 **winrt::param** 命名空间中的类型。 它们的用途是为投影提供帮助。

许多类型都有同步和异步版本。 当你要将参数传递到同步方法时，C++/WinRT 将使用同步版本；当你要将参数传递到异步方法时，C++/WinRT 将使用异步版本。 异步版本采用额外的步骤，以使调用方在操作完成之前难于转变集合。 但是，请注意，这些变体都不会防止从另一个线程中转变集合。 防止从另一个线程中转变集合是你的责任。

## <a name="string-parameters"></a>字符串参数

**winrt::param::hstring** 简化了将参数传递给接受 **HSTRING** 的 API。

|可以传递的类型|注释|
|-|-|
|`{}`|传递空字符串。|
|**winrt::hstring**||
|**std::wstring_view**|对于文本，可以编写 `L"Name"sv`。 该视图在末尾后必须具有 null 终止符。|
|**std::wstring**|-|
|**wchar_t const\***|以 null 终止的字符串。|

不允许 `nullptr`。 请改用 `{}`。

编译器知道在编译时如何评估字符串文本上的 `wcslen`。 因此，对于文本，`L"Name"sv` 和 `L"Name"` 是等效的。

请注意，**std::wstring_view** 对象不以 null 终止，但 C++/WinRT 要求字符串末尾后的字符为 null。 如果你传递并非以 null 终止的 **std::wstring_view**，则过程将会终止。

## <a name="iterable-parameters"></a>可迭代的参数

winrt::param::iterable\<T\> 和 winrt::param::async_iterable\<T\> 简化了将参数传递给接受 IIterable\<T\> 的 API  。

Windows 运行时集合已是 **IIterable** 类型。

|可以传递的类型|同步|Async|注释|
|-|-|-|-|
| `nullptr` | 是 | 是 | 你必须验证基础方法是否支持 `nullptr`。|
| **IIterable\<T\>** | 是 | 是 | 或可转换为它的任何内容。|
| **std::vector\<T\> const&** | 是 | 否 ||
| **std::vector\<T\>&&** | 是 | 是 | 内容将移动到迭代器中，以防止变化。|
| **std::initializer_list\<T\>** | 是 | 是 | 异步版本会复制这些项。|
| **std::initializer_list\<U\>** | 是 | 否 | **U**必须可转换为 **T**。|
| `{ ForwardIt begin, ForwardIt end }` | 是 | 否 | `*begin` 必须可转换为 **T**。|

请注意，不允许使用 IIterable\<U\> 和 std::vector\<U\>，即使 U 可转换为 T 也是如此   。对于 std::vector\<U\>，可以使用双迭代器版本（详见下文）。

在某些情况下，你的对象可能会真正实现你需要的 **IIterable**。 例如，由 [FileOpenPicker.PickMultipleFilesAsync](/uwp/api/windows.storage.pickers.fileopenpicker.pickmultiplefilesasync) 生成的 IVectorView\<StorageFile\> 会实现 IIterable\<StorageFile\>  。 但它也实现 **IIterable\<IStorageItem\>** ；你只需显式请求它。

```cppwinrt
IVectorView<StorageFile> pickedFiles{ co_await filePicker.PickMultipleFilesAsync() };
requestData.SetStorageItems(storageItems.as<IIterable<IStorageItem>>());
```

在其他情况下，可以使用双迭代器版本。

```cppwinrt
std::vector<StorageFile> storageFiles;
requestData.SetStorageItems({ storageFiles.begin(), storageFiles.end() });
```

双迭代器更普遍适用于你有一个不适合以上任何方案的集合的情况，只要你可以循环访问它并生成可以转换为 **T** 的内容。我们在上面使用它来循环访问派生类型的向量。 此处，我们使用它来循环访问派生类型的非向量。

```cppwinrt
std::array<StorageFile, 3> storageFiles;
requestData.SetStorageItems(storageFiles); // This doesn't work.
requestData.SetStorageItems({ storageFiles.begin(), storageFiles.end() }); // But this works.
```

如果迭代器为 `RandomAcessIt`，[IIterator\<T\>.GetMany(T\[\])](/uwp/api/windows.foundation.collections.iiterator-1.getmany) 的实现会更高效。 否则，它会在该范围上进行多次传递。

|可以传递的类型|同步|Async|注释|
|-|-|-|-|
| `nullptr` | 是 | 是 | 你必须验证基础方法是否支持 `nullptr`。|
| **IIterable\<IKeyValuePair\<K, V\>\>** | 是 | 是 | 或可转换为它的任何内容。|
| **std::map\<K, V\> const&** | 是 | 否 ||
| **std::map\<K, V\>&&** | 是 | 是 | 内容将移动到迭代器中，以防止变化。|
| **std::unordered_map\<K, V\> const&** | 是 | 否 ||
| **std::unordered_map\<K, V\>&&** | 是 | 是 | 内容将移动到迭代器中，以防止变化。|
| **std::initializer_list\<std::pair\<K, V\>\>** | 是 | 是 | 类型 **K** 和 **V** 必须完全匹配。 键不能重复。 异步版本会复制这些项。|
| `{ ForwardIt begin, ForwardIt end }` | 是 | 否 | `begin->first` 和 `begin->second` 必须可分别转换为 **K** 和 **V**。|

## <a name="vector-view-parameters"></a>向量视图参数

winrt::param::vector_view\<T\> 和 winrt::param::async_vector_view\<T\> 简化了将参数传递给接受 IVectorView\<T\> 的 API  。

可以使用 [IVector\<T\>.GetView](/uwp/api/windows.foundation.collections.ivector-1.getview) 从 IVector 获取 IVectorView  。

|可以传递的类型|同步|Async|注释|
|-|-|-|-|
| `nullptr` | 是 | 是 | 你必须验证基础方法是否支持 `nullptr`。|
| **IVectorView\<T\>** | 是 | 是 | 或可转换为它的任何内容。|
| **std::vector\<T\>const&** | 是 | 否 ||
| **std::vector\<T\>&&** | 是 | 是 | 内容将移动到视图中，以防止变化。|
| **std::initializer_list\<T\>** | 是 | 是 | 类型必须完全匹配。 异步版本会复制这些项。|
| `{ ForwardIt begin, ForwardIt end }` | 是 | 否 | `*begin` 必须可转换为 **T**。|

双迭代器版本可用于从那些不满足直接传递的要求的内容创建矢量视图。 但是，请注意，由于向量支持随机访问，我们建议你传递 `RandomAcessIter`。

## <a name="map-view-parameters"></a>映射视图参数

winrt::param::map_view\<T\> 和 winrt::param::async_map_view\<T\> 简化了将参数传递给接受 IMapView\<T\> 的 API  。

可以使用 **IMap::GetView** 从**IMap** 获取 **IMapView**。

|可以传递的类型|同步|Async|注释|
|-|-|-|-|
| `nullptr` | 是 | 是 | 你必须验证基础方法是否支持 `nullptr`。|
| **IMapView\<K, V\>** | 是 | 是 | 或可转换为它的任何内容。|
| **std::map\<K, V\> const&** | 是 | 否 ||
| **std::map\<K, V\>&&** | 是 | 是 | 内容将移动到视图中，以防止变化。|
| **std::unordered_map\<K, V\> const&**  | 是 | 否 ||
| **std::unordered_map\<K, V\>&&** | 是 | 是 | 内容将移动到视图中，以防止变化。|
| **std::initializer_list\<std::pair\<K, V\>\>** | 是 | 是 | 同步和异步版本都复制这些项。 不能重复键。|

## <a name="vector-parameters"></a>向量参数

winrt::param::vector\<T\> 简化了将参数传递给接受 IVector\<T\> 的 API 。

|可以传递的类型|注释|
|-|-|
| `nullptr` | 你必须验证基础方法是否支持 `nullptr`。|
| **IVector\<T\>** | 或可转换为它的任何内容。|
| **std::vector\<T\>&&** | 内容将移动到参数中，以防止变化。 结果不会移回。|
| **std::initializer_list\<T\>** | 内容将复制到参数中，以防止变化。|

如果方法会转变向量，则观察变化的唯一方法是直接传递 **IVector**。 如果传递 **std:: vector**，那么方法会转变副本而非原始内容。

## <a name="map-parameters"></a>映射参数

winrt::param::map\<T\> 简化了将参数传递给接受 IMap\<T\> 的 API 。

|可以传递的类型|注释|
|-|-|
| `nullptr` | 你必须验证基础方法是否支持 `nullptr`。|
| **IMap\<T\>** | 或可转换为它的任何内容。|
| **std::map\<K, V\>&&** | 内容将移动到参数中，以防止变化。 结果不会移回。|
| **std::unordered_map\<K, V\>&&** | 内容将移动到参数中，以防止变化。 结果不会移回。|
| **std::initializer_list\<std::pair\<K, V\>\>** | 内容将复制到参数中，以防止变化。|

如果方法会转变映射，则观察变化的唯一方法是直接传递 **IMap**。 如果传递 **std::map** 或 **std::unordered_map**，那么方法会转变副本而非原始内容。

## <a name="array-parameters"></a>数组参数

winrt::array_view\<T\> 不在 winrt::param 命名空间中，但可以将其用于作为 C 样式数组（也称为“一致数组”）的参数 。

|可以传递的类型|注释|
|-|-|
| `{}` | 一个空数组。|
| **array** | C 的一致数组（即 `C array[N];`），其中 **C** 可转换为 **T**，并且 `sizeof(C) == sizeof(T)`。 |
| **std::array<C, N>** | **C** 的 C++ **std::array**，其中 **C** 可转换为 **T**，并且 `sizeof(C) == sizeof(T)`。 |
| **std::vector<C>** | **C** 的 C++ **std::vector**，其中 **C** 可转换为 **T**，并且 `sizeof(C) == sizeof(T)`。 |
| `{ T*, T* }` | 一对表示范围 [开始, 结束) 的指针。|
| **std::initializer_list\<T\>** ||

另请参阅博客文章[跨 Windows 运行时 ABI 边界传递 C 样式数组的多种模式](https://devblogs.microsoft.com/oldnewthing/20200205-00/?p=103398)。